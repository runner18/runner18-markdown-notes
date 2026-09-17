https://www.bostontech.net/wp-content/uploads/2020/10/Practical-Data-Communications-and-Networking-10.HART-protocol.pdf
^helpful resource
## What is HART?
HART is a way for devices to talk to each other. All devices sit on a single loop. Two signals:
1. 4-20mA analog signal
2. Digital analog signal

### Master vs Slave Devices
- One one master device
- One or more slave devices

Communication sequence:
- Master sends command to one specific Slave
- One specific Slave sends response
- Repeat

## HART Message Breakdown
![](Screenshot%202026-09-16%20164753.png)


| Sections of HART Command | Purpose                                                                                  |
| ------------------------ | ---------------------------------------------------------------------------------------- |
| Preamble                 | "a HART message is about to start!"                                                      |
| Start Delimiter          | "here is what the message frame/structure will look like"                                |
| Address                  | "here is the address for one specific slave"                                             |
| Command number           | "here is the command number"                                                             |
| Byte count               | "here is how long the data section will be"                                              |
| Status                   | "here is where any errors will go"                                                       |
| Data                     | "here is any data associated with this command number"                                   |
| CRC Checksum             | "if any bits accidentally changed, your CRC calculation will be different than this one" |

Important points:
- One message at a time! No one can talk over each other!
- Slaves only over talk in response to a Master Command
- Master Command and Slave response both must include the Slave's address

### Delimiter
| Bit: | 7                                                    | 6, 5                 | 4, 3                | 2, 1, 0                                                                               |
| ---- | ---------------------------------------------------- | -------------------- | ------------------- | ------------------------------------------------------------------------------------- |
|      | Address Type                                         | # of Expansion Bytes | Physical-layer type | Frame type                                                                            |
|      | 0 = short/polling address<br>1 = long/unique address | Usually all zeros    | Usually all zeroes  | 010 = STX/Start of Text<br>110 = ACK/Acknowledge<br>001 = BACK/Burst-mode acknowledge |

### Command
Tells you what the data section in a message is for.

HART Commands fall under the following categories:

1. Universal Commands (0-19) - your HART devices HAVE to be able to handle these
2. Common Practice Commands (34-110) - your HART devices SHOULD be able to handle these
3. Device Specific Commands (128+) - you, or anyone else making a device using HART, can define these if you want

## Short vs Long Addresses

| Slave's Address Type          | Format                                       | Frame Type  | Bit Length | Usage in Older Versions of HART                                                                                                                                           | Usage in Modern HART                                                                          |
| ----------------------------- | -------------------------------------------- | ----------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Short address/Polling Address | Single number, 0-63. <br><br>Typically 0.    | Short-frame | 4 bits     | Used for everything, long address didn't exist yet.<br><br>Short address of 0 is used when there is one slave only. 1-63 when there are multiple slaves (multi-drop mode) | Short address ONLY used inside Command 0, where Command 0 gets the Long Address of the slave. |
| Long Address/Unique ID        | Longer address, unique to that slave device. | Long-frame  | 38 bits    | Not used at all, wasn't invented yet                                                                                                                                      | Used by default                                                                               |
|                               |                                              |             |            |                                                                                                                                                                           |                                                                                               |

Quote from HART Clarifies:
	"Previous, obsolete revisions of the protocol utilized short frame addresses. In order to maintain backwards compatibility, only Universal Command 0 now supports short frame addressing."

## Ways to Connect Master to Slave Device

In modern HART, Master connects to Slave by getting its "long address" and using that.

| Command                                              | Long or Short Address?                                                                                                                             | How is Slave identified? |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| 0 - Read Unique Identifier                           | Short                                                                                                                                              | Via Short address        |
| 11 - Read Unique Identifier Associated With Tag      | Long - all the bits are zero<br><br>Bits are all zero because we do not know the long address/unique ID. Slave is identifier via tag instead.      | Via HART Tag             |
| 21 - Read Unique Identifier Associated With Long Tag | Long - all the bits are zero<br><br>Bits are all zero because we do not know the long address/unique ID. Slave is identifier via long tag instead. | Via HART Long Tag        |

### Command 0 w/ Short Address
- Master Sends Command 0
- Command 0 contains "short" address, probably just 0
- Slave receives Command 0
- Slave responds with "full" address

### Command 11 w/ Broadcast Address
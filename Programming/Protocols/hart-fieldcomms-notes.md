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
|                          |                                                                                          |

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
Every HART message falls under a "Command"
- Command tells you what the data in a message is for
	- Are these 1's and 0's a Number? Text? A date? The weather? Something else?
- HART has some Commands pre-defined
- Other commands are left open for YOU to define if you want! Wow!

HART Commands fall under the following categories:

1. Universal Commands (0-19) - your HART devices HAVE to be able to handle these
2. Common Practice Commands (34-110) - your HART devices SHOULD be able to handle these
3. Device Specific Commands (128+) - you, or anyone else making a device using HART, can define these if you want

## Short vs Long Address, Command 0, and more

| Slave's Address Type | Format                                       | Frame Type  | Bit Length | Usage in Older Versions of HART                                                                                                                    | Usage in Modern HART                                                                          |
| -------------------- | -------------------------------------------- | ----------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Short address        | Single number, 0-63. Typically 0.            | Short-frame | 4 bits     | Used for everything, there was not long address yet.<br><br>Short address 0 used when there is one slave only. 1-63 when there are multiple slaves | Short address ONLY used inside Command 0, where Command 0 gets the Long Address of the slave. |
| Long Address         | Longer address, unique to that slave device. | Long-frame  | 38 bits    | Not used at all, wasn't invented yet                                                                                                               | Used by default                                                                               |
|                      |                                              |             |            |                                                                                                                                                    |                                                                                               |

### Old vs New HART
Older versions of HART only used the short/polling address, part of a "short-frame" message.
Modern HART introduced the long/unique address, part of a "long-frame" message.

Short vs Long address
	The Slave's "short" address is a single number, 0-63. Typically 0.
	The Slave's "long" address is a unique id that no other slave will have.

Modern HART uses "long" address, not "short" address
- Modern HART introduced the "long" address
- A Modern HART Master only uses the "short" address to fetch the "long/unique" address via Command 0.

Quote from HART Clarifies:
	"Previous, obsolete revisions of the protocol utilized short frame addresses. In order to maintain backwards compatibility, only Universal Command 0 now supports short frame addressing."

### Don't Confuse Broadcast Address 0 with Command 0
Broadcast Address is a LONG "address" that means "I don't know what your full address is, but respond if "

### Command 0 w/ Short Address
- Master Sends Command 0
- Command 0 contains "short" address, probably just 0
- Slave receives Command 0
- Slave responds with "full" address

### Command 11 w/ Broadcast Address
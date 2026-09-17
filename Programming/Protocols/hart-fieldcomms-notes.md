
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
Address Type notes:
- Older versions of HART only used the "short"/polling address/short frame
- Modern HART introduced the "long" address/long frame

Quote from HART that clarifies:
"Previous, obsolete revisions of the protocol utilized short frame addresses. In order to maintain backwards compatibility, only Universal Command 0 now supports short frame addressing."

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


## How to Connect to Slave Device
The Master connects to a Slave by getting the Slave's "full address".

There are multiple ways the Master can get the Slave's "full address":

### Command 0 w/ Short Address
Command 0 can get the Slave's "full address" by sending Command 0 and using a "Short Address"
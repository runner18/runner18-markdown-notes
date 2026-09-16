
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

| Sections of HART Command | Purpose                                                                      |
| ------------------------ | ---------------------------------------------------------------------------- |
| Preamble                 | "a HART message is about to start!"                                          |
| Start Delimiter          | "here is what the message frame/structure will look like"                    |
| Address                  | "here is the address for one specific slave"                                 |
| Command number           | "here is the command number"                                                 |
| Byte count               | "here is how long the data section will be"                                  |
| Status                   | "here is where any errors will go"                                           |
| Data                     | "here is any data associated with this command number"                       |
| CRC                      | "if any bits accidentally changed, your CRC will be different than this one" |

Important points:
- One message at a time! No one can talk over each other!
- Slaves only over talk in response to a Master Command
- Master Command and Slave response both must include the Slave's address

### Delimiter
| Bit: | 7                                                    | 6, 5                 | 4, 3                | 2, 1, 0    |
| ---- | ---------------------------------------------------- | -------------------- | ------------------- | ---------- |
|      | Address Type                                         | # of Expansion Bytes | Physical-layer type | Frame type |
|      | 0 = short/polling address<br>1 = long/unique address |                      |                     |            |


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

=======

Thank you. Some follow up questions.
I notice the start delimiter has a few fields:
 - bit 7 - 0 is short/polling, 1 is long/unique. that makes sense
 - bits 6-5 - what are the expansion bytes? What are the bytes expanding?
 - bits 4-3 - would this be wired vs wireless HART or something?
 - 2-0 - what are the different frame types? oh never mind you talk about that below

010 - STX. what does the STX stand for? do the individual bits means something for the frame type? When you say 02 vs 82, ahh okay, you're saying both are the STX frame type except one is short address and the other is long address. 

Also, if Command 0 is used to get the long address/unique identifier, did Command 0 not exist through HART Revision 4? Or was Command 0 used for something else through HART Revision 4?

So, why is the "short" address called a polling address? Also is multidrop only used with the "short" addresses, or is it also used with the long/unique addresses? Is there an advantage to using the "short" addresses in multi-drop mode? Does mutli-drop mode also have the same structure where it's Master then Slave then Master then Slave?

Do most modern HART slave devices accept short/polling addresses for commands other than command zero, then?



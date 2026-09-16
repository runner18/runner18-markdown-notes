
## What is HART?
HART is a way for devices to talk to each other. All the devices sit on the same circuit loop. The loop is called a 4-20mA loop, because the current going around on the circuit ranges from 4-20 milliamps. Two ways info is communicated:
1. 4-20mA signal - devices read in the current amount and convert that into a variable
2. Digital signal - digital pulses are also sent across the loop alongside the 4-20mA signal

## Single Slave Setup
This setup consists of one "Master" device and one "Slave" device on the same loop circuit.

The communication sequence between these two devices goes like this:
1. Master sends Command 0 ("give me ur full addr plz") to Slave
2. Slave responds with full address
3. Master receives slave's full address
4. Master will include the slave's full address in future commands
5. The Slave will include its full address in responses to the Master

Other important points:
- The Slave only talks in response to the Master
- The Master must ask the slave for something with a Command to get it

## Multi-Slave Setup (Multi-drop Mode)
This setup consists of one "Master" device and multiple "Slave" devices. 

The Master assigns short-style address to each slave device, ranging from address 1-63. 

NOTE: Address 0 is reserved for when a Master sends Command 0 in a single slave setup. Master never sends Command 0 or specifies address 0 in multi-drop mode.

I don't understand how this one works quite as much.

### Slave "Short" Addresses vs "Full" Addresses
Not as sure on this one, but this is my understanding.
Slaves have TWO addresses they will respond to:
1. It's full address
2. It's short address

Normally, slave devices have a short addresses of 0.

In a single-slave setup:
- Master does not know Slave's full address yet
- Master sends Command 0 with "Short" address of 0
- Slave probably has "Short" address of 0
- Slave responds to Master with "full" address
- Master uses Slave's "full" address moving forward

In Multi-drop mode:
- Master sets each Slave's "Short" address to 1,2,3,4, whatever
- Slave now responds to "Short" address
- Easier for Master to use "Short" addresses when there are multiple Slaves

What I don't understand:
- Why wouldn't the master just always use the "Short" address if there's only one slave device
- In multi-drop mode, how does the Master target each individual slave when setting their "Short" addresses?
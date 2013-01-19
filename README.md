#YCommunicator

Version 1.0

A protocol and implementation of "device to device" serial communication, originally planned for connecting Arduino and other devices via serial communication.

## Goal
 - Setup simple communication between Arduino and other devices
 - 2 way communication, both parties are equal: both can initialize a command to the other party, both can respond to commands sent from the other party.
 - Simple API: allow developers to define commands and register functions on those commands.
 - Availability: support multiple devices by having creating libraries for all major OSs and languages.


##Protocol Definitions

###Packet:
A packet is the minimum transmitable unit. It wraps a payload - a byte array of unspecified data

#####Structure:

 1. unsigned char **payload_len**
 - unsigned char[payload_len] **payload** - actual instructions
 - unsigned short int **checksum**

#####Notes:

 - Packet size is always payload size + 3
 - Max payload size is 255

###Instruction:
An Instruction is the most basic operational syntax that can be exchanged between the devices.

#####Structure:
 1. unsigned char **type** - types are COMMAND | RETURN 
 - unsigned char **command** - predefined instruction to be preformed
 - unsigned char[] **data** - additional values

#####Notes:
 - data length is always **payload_len - 2**

#####Suggested Commands:
 1. 0x00 Ping
 - 0x01 Clock
 - ...

##Functional (API) Prototype

#####YCommunicator
 - **+void write(byte)** - write incoming bytes into the communicator
 - **+byte read()** - read outgoing bytes from the communicator
 - **+int available()** - Get number of bytes available for output
 - **+dispatch(byte type, byte command, byte[] data, byte data_langth)** - Dispatch an instruction
 - **+registerDefaultCallback(callback_function(byte type, byte command, byte[] data, byte data_langth))** - Defualt function to trigger when an instruction arrives. Only triggered if callback function was not defined for the instruction's command
 - **+registerCallback(byte command, callback_function(byte type, byte command, byte[] data, byte data_langth))** - Trigger this function when an instruction arrives with a specific command value.


## Help is needed!
For this project to be attractive, we need an implementation of the above API on several devices and languages: Arduino (daa...), Android, iOS, Python, Java, etc.
Also, I suggest to create an application for out-of-the-box integration:

 1. Arduino example sketch.
 - Mobile App that supports adding/defining controls and sensors (see below)
 - PC application with graphic UI that supports adding/defining controls.
 - 

#####Suggestion for Mobile app:
 1. Allow user to add UI controls to his app by on-screen selection. Every contol is identified by a number. Controls can be buttons, text fiels, chackboxes, scrolls, etc.
 - Define an instruction that is dispatched on a control value change. For example: type=0, command=0x2, data=[(byte)control number,(4 byte) control value]
 - Allow user to add sensors data - GPS, Compas, Gyro/Accelometer, etc. Need to define a command/data struct for each sensor seperatly.  



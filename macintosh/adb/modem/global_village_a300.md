# Global Village A300 ADB Modem Protocol

This document contains experimentally-observed behavior of the Global Village A300 modem's ADB protocol.  It contains enough information to satisfactorally emulate the serial port, however, many unknowns exist.

The modem's default address may be 0x5 (observed on firmware 1.5) or 0x7 (observed on firmware 1.4) and its default handler ID is 0x36.

Consistent with ADB standards, register 0 is the primary communications channel with the modem.

## Talk 0

A Talk 0 command will read status information or data received by the serial port.  If no payload is received, there is no data or status to receive.  If received, the payload is always eight bytes in length, and the eighth byte determines the meaning of the payload:

| 8th Byte  | Meaning                                                                                                |
| --------- | ------------------------------------------------------------------------------------------------------ |
| 0x00-0x7F | Payload contains 8 bytes received by serial port in 1st through 8th bytes                              |
| 0x80      | Payload contains no data                                                                               |
| 0x81      | Payload contains 1 byte received by serial port in 1st byte, 2nd through 7th are invalid               |
| 0x82      | Payload contains 2 bytes received by serial port in 1st through 2nd bytes, 3rd through 7th are invalid |
| 0x83      | Payload contains 3 bytes received by serial port in 1st through 3rd bytes, 4th through 7th are invalid |
| 0x84      | Payload contains 4 bytes received by serial port in 1st through 4th bytes, 5th through 7th are invalid |
| 0x85      | Payload contains 5 bytes received by serial port in 1st through 5th bytes, 6th through 7th are invalid |
| 0x86      | Payload contains 6 bytes received by serial port in 1st through 6th bytes, 7th is invalid              |
| 0x87      | Payload contains 7 bytes received by serial port in 1st through 7th bytes                              |
| 0x88      | Modem-to-Mac status A in 1st through 5th bytes, 6th and 7th are invalid                                |
| 0x89      | Modem-to-Mac status B in 1st byte, 2nd through 7th are invalid                                         |
| 0x8A-0x8F | Undetermined                                                                                           |
| 0x90-0xFF | Payload contains 8 bytes received by serial port in 1st through 8th bytes                              |

The byte 0x95 is treated specially by the driver; in order to reflect the reception of a single 0x95 byte, the device must send two 0x95 bytes in a row.  They do not have to be in the same ADB payload.

### Modem-to-Mac Status A

Default state (with driver installed) appears to be 0xF0 0x01 0x00 0x30 0x0C.  First four bytes are the same as Talk 1, second byte may be the same as second byte of Listen 1.

| Bit (big endian) | Meaning                                   |
| ---------------- | ----------------------------------------- |
| 39-38            | Undetermined                              |
| 37               | OH (off-hook) indicator, active low       |
| 36-33            | Undetermined                              |
| 32               | AA (auto answer) indicator, active high   |
| 31-26            | Undetermined                              |
| 25               | Modem on?                                 |
| 24               | Modem driver installed?                   |
| 23-0             | Undetermined                              |

36-34 change when connection is being established, 36 active low and 35-34 active high.  One of these drives the CD (carrier detect) indicator).

### Modem-to-Mac Status B

| Bit | Meaning                                         |
| --- | ----------------------------------------------- |
| 7-4 | Undetermined                                    |
| 3-0 | Baud rate of established connection (see below) |

| Bits 3-0 | Baud Rate |
| -------- | --------- |
| 0x8      | 2400      |
| 0x7      | 1200      |
| 0x6      | 300       |

Other values are undetermined.

## Listen 0

Listen 0 will write data to be sent by the serial port.  The payload must always be eight bytes in length, with the eighth byte determining the meaning of the payload:

| 8th Byte  | Meaning                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------- |
| 0x00-0x7F | Payload contains 8 bytes to be sent to serial port in 1st through 8th bytes                              |
| 0x80      | Undetermined                                                                                             |
| 0x81      | Payload contains 1 byte to be sent to serial port in 1st byte, 2nd through 7th are invalid               |
| 0x82      | Payload contains 2 bytes to be sent to serial port in 1st through 2nd bytes, 3rd through 7th are invalid |
| 0x83      | Payload contains 3 bytes to be sent to serial port in 1st through 3rd bytes, 4th through 7th are invalid |
| 0x84      | Payload contains 4 bytes to be sent to serial port in 1st through 4th bytes, 5th through 7th are invalid |
| 0x85      | Payload contains 5 bytes to be sent to serial port in 1st through 5th bytes, 6th through 7th are invalid |
| 0x86      | Payload contains 6 bytes to be sent to serial port in 1st through 6th bytes, 7th is invalid              |
| 0x87      | Payload contains 7 bytes to be sent to serial port in 1st through 7th bytes                              |
| 0x88-0x8F | Undetermined                                                                                             |
| 0x90-0xFF | Payload contains 8 bytes to be sent to serial port in 1st through 8th bytes                              |

## Talk 1

Payload is the same as the first four bytes of Modem-to-Mac Status A.  Post-reset state is 0xF0000000.

## Listen 1

Payload appears always to be two bytes in length.  Second byte may be the same as second byte of Modem-to-Mac Status A.

| Bits (big endian) | Meaning                 |
| ----------------- | ----------------------- |
| 15-2              | Undetermined            |
| 1                 | Modem on?               |
| 0                 | Modem driver installed? |

## Talk 2

Payload appears always to be six bytes in length.  Appears to communicate the information given by the driver's 'about' box:

| Bits (big endian) | Example Value | Meaning                                                                       |
| ----------------- | ------------- | ----------------------------------------------------------------------------- |
| 47-44             | 0x1           | Firmware major revision (0-9)                                                 |
| 43-40             | 0x4           | Firmware minor revision (0-9)                                                 |
| 39-16             | 0x019F8E      | ID number (shown as decimal)                                                  |
| 15-10             | 0b011001      | Undetermined                                                                  |
| 9-0               | 0x03A         | Manufacture date, expressed as number of weeks since Sunday, 31 December 1989 |

Example values are from a modem where the control panel displayed "Firmware 1.4, Made 1991-02-10, ID 106382".

## Listen 2

Payload appears always to be four bytes in length.

| Bits (big endian) | Meaning                                                           |
| ----------------- | ----------------------------------------------------------------- |
| 31-15             | Undetermined                                                      |
| 14                | Break (active high; set and then clear to send a break character) |
| 13-0              | Undetermined                                                      |

# Global Village A300 ADB Modem Protocol

This document contains experimentally-observed behavior of the Global Village A300 modem's ADB protocol.  It contains enough information to satisfactorally emulate the serial port, however, many unknowns exist.

The modem's default address is 0x5, which is the normal default address for low-speed serial devices, and its default handler ID is 0x36.

Consistent with ADB standards, register 0 is the primary communications channel with the modem.

## Talk 0

A Talk 0 command will read status information or data received by the serial port.  If no payload is received, there is no data or status to receive.  If received, the payload is always eight bytes in length, and the eighth byte determines the meaning of the payload:

| 8th Byte  | Meaning                                                                                                |
| --------- | ------------------------------------------------------------------------------------------------------ |
| 0x00-0x7F | Payload contains 8 bytes received by serial port in 1st through 8th bytes                              |
| 0x80      | Undetermined                                                                                           |
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

Default state appears to be 0xF0 0x01 0x00 0x30 0x0C 0x88 0x88 0x88.  First four bytes may be the same as Talk 1, second byte may be the same as second byte of Listen 1.

| Bit (big endian) | Meaning                                       |
| ---------------- | --------------------------------------------- |
| 63-62            | Undetermined                                  |
| 61               | OH (off-hook) indicator, active low           |
| 60               | CD (carrier detect) indicator, active low (?) |
| 59-57            | Undetermined                                  |
| 56               | AA (auto answer) indicator, active high       |
| 55-0             | Undetermined                                  |

### Modem-to-Mac Status B

Undetermined.

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

Payload appears always to be four bytes in length.  May be the same as first four bytes of Modem-to-Mac Status A.

## Listen 1

Payload appears always to be two bytes in length.

| Bits (big endian) | Meaning                 |
| ----------------- | ----------------------- |
| 15-2              | Undetermined            |
| 1                 | Modem on?               |
| 0                 | Modem driver installed? |

## Talk 2

Payload appears always to be eight bytes in length.  Appears to communicate the information given by the driver's 'about' box:

| Bits (big endian) | Example Value | Meaning                                                                       |
| ----------------- | ------------- | ----------------------------------------------------------------------------- |
| 63-60             | 0x1           | Firmware major revision (0-9)                                                 |
| 59-56             | 0x5           | Firmware minor revision (0-9)                                                 |
| 55-32             | 0x01E8C7      | ID number (shown as decimal)                                                  |
| 31-26             | 0b010100      | Undetermined                                                                  |
| 25-16             | 0x279         | Manufacture date, expressed as number of weeks since Sunday, 31 December 1989 |
| 15-0              | 0xE1E1        | Undetermined                                                                  |

Example values are from a modem where the control panel displayed "Firmware 1.5, Made 2/17/2, ID 125127", however the stated manufacture date being in 2002 is difficult to believe.

## Listen 2

Payload appears always to be four bytes in length.

| Bits (big endian) | Meaning                                                           |
| ----------------- | ----------------------------------------------------------------- |
| 31-15             | Undetermined                                                      |
| 14                | Break (active high; set and then clear to send a break character) |
| 13-0              | Undetermined                                                      |

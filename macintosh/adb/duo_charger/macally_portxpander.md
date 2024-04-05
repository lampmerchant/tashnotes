# MacAlly PortXpander

The PortXpander presents itself on the ADB with address 0x6 and handler ID 0x9A.

Why the PortXpander uses address 0x6 instead of 0x7 is unknown.  It is possible, though only speculation, that Apple began allocating handler IDs on address 0x6 after all handler IDs on address 0x7 were used.


## Device Modes

| Byte | Mode                     |
| ---- | ------------------------ |
| 0x01 | First serial port        |
| 0x02 | Second serial port       |
| 0x03 | Third serial port        |
| 0x04 | Detection mode (default) |


## Talk 0

Talk 0 always responds with two bytes, both equal to the device's current mode.


## Listen 3

Unusually, all commands sent to the PortXpander are sent via two-byte Listen 3 commands.  The effect of a Listen 3 command is determined by the second byte.

| Second Byte | Effect                                      |
| ----------- | ------------------------------------------- |
| 0x00        | No apparent effect                          |
| 0x01        | Change mode to 0x01                         |
| 0x02        | Change mode to 0x02                         |
| 0x03        | Change mode to 0x03                         |
| 0x04        | Change mode to 0x04                         |
| 0x05-0x0F   | No apparent effect                          |
| 0x10-0xEF   | Send detection bytes                        |
| 0xF0-0xFD   | No apparent effect                          |
| 0xFE        | Change ADB address if no collision detected |
| 0xFF        | No apparent effect                          |

When changing to modes 0x01-0x03, the PortXpander's driver sets the first byte to 0x00; when changing to mode 0x04, the driver sets the first byte to 0x04.


### Send Detection Bytes

When the PortXpander is in mode 0x04 and the second byte of a Listen 3 command is between 0x10 and 0xEF, inclusive, the first and second byte are sent to the Macintosh over the serial port at a baud rate of 2400 Hz.  The PortXpander's driver uses this with the string "AB" (0x41 0x42) to detect whether a given PortXpander is plugged into the printer or modem port.


### Change ADB Address

In accordance with the ADB specification, 0xFE as a second byte to Listen 3 changes the device's address to the value given by the low nibble of the first byte if no collision on the ADB has been detected, allowing for multiple PortXpanders to be connected to the same Macintosh (one to the printer port, one to the modem port).

According to the ADB specification, 0x00 as a second byte to Listen 3 should change the device's SRQ-enable bit and address unconditionally.  The PortXpander appears to ignore such Listen 3 commands.

# Gravis Mac GamePad

The Gravis Mac GamePad first appears with address 0x2 and handler ID 0x02, emulating the protocol of an Extended Keyboard.  The directional pad buttons on the controller emulate the arrow keys on the keyboard while the colored buttons do nothing.  The Gravis cdev changes the handler ID to 0x34 when it loads, enabling the following output formats.

## Talk 0

| Bit | Description           | Encoding                                               |
| --- | --------------------- | ------------------------------------------------------ |
| 15  | Yellow (down) button  | 0 = down, 1 = up                                       |
| 14  | Green (right) button  | 0 = down, 1 = up                                       |
| 13  | Red (left) button     | 0 = down, 1 = up                                       |
| 12  | Blue (up) button      | 0 = down, 1 = up                                       |
| 11  | Directional pad up    | 0 = down, 1 = up                                       |
| 10  | Directional pad right | 0 = down, 1 = up                                       |
| 9   | Directional pad down  | 0 = down, 1 = up                                       |
| 8   | Directional pad left  | 0 = down, 1 = up                                       |
| 7-1 | Always 1              |                                                        |
| 0   | Control switch        | 0 = toward directional pad, 1 = toward colored buttons |

## Talk 1

Always responds with 0x03 0x00.  This may be a protocol or device identifier.

## Talk 2

Always responds with 0xFF 0xFF.  This may be an artifact of the GamePad's keyboard emulation.

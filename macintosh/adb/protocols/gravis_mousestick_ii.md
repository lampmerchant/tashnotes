# Gravis MouseStick II

The Gravis MouseStick II first appears with address 0x3 and handler ID 0x01, but the Gravis cdev changes the handler ID to 0x23 when it loads, which enables the following output formats.

## Talk 1

This is apparently a protocol identifier.  Known responses from MouseStick II joysticks are 0x03 0x00 and 0x04 0x00.

| Response  | Talk 0 Protocol Type |
| --------- | -------------------- |
| 0x03 0x00 | 7-byte protocol      |
| 0x04 0x00 | 3-byte protocol      |

## Talk 0 (7-Byte Protocol)

| Byte    | Description         |
| ------- | ------------------- |
| 1st-2nd | Mouse movement data |
| 3rd-4th | Joystick X position |
| 5th-6th | Joystick Y position |
| 7th     | Joystick buttons    |

Joystick positions are given as signed 2's complement 16-bit big-endian integers, with 0x0000 representing the center, negative integers representing left or up, and positive integers representing right or down.  The maximum throw of the joystick appears to be approximately -600 to +600.

### Mouse Movement Data

| Bit  | Description            |
| ---- | ---------------------- |
| 15   | Mouse button status    |
| 14-8 | Mouse Y movement delta |
| 7    | Always 1               |
| 6-0  | Mouse X movement delta |

The mouse movement data appears to be the same as in the ADB mouse protocol, with relative cursor movements given as signed 2's complement 7-bit integers, with 0x00 representing no movement, negative integers representing left or up, and positive integers representing right or down.  The mouse button status bit is 0 when down and 1 when up.

### Joystick Buttons

| Bit | Description                           |
| --- | ------------------------------------- |
| 7-5 | Always 1                              |
| 4   | Right button atop joystick (button 5) |
| 3   | Left button atop joystick (button 4)  |
| 2   | Trigger button (button 1)             |
| 1   | Bottom circular button (button 3)     |
| 0   | Top circular button (button 2)        |

Joystick button bits are 0 when down and 1 when up.

## Talk 0 (3-Byte Protocol)

| Byte | Description         |
| ---- | ------------------- |
| 1st  | Joystick X position |
| 2nd  | Joystick Y position |
| 3rd  | Joystick buttons    |

Joystick positions are given as unsigned 8-bit integers, with 0x80 representing the center, 0x00 representing the left or top extreme, and 0xFF representing the right or bottom extreme.  The MouseStick II control panel apparently scales this to the same -600 to +600 range used by the 7-byte protocol.

### Joystick Buttons

| Bit | Description                           |
| --- | ------------------------------------- |
| 7-5 | Always 1                              |
| 4   | Right button atop joystick (button 5) |
| 3   | Left button atop joystick (button 4)  |
| 2   | Trigger button (button 1)             |
| 1   | Bottom circular button (button 3)     |
| 0   | Top circular button (button 2)        |

Joystick button bits are 0 when down and 1 when up.

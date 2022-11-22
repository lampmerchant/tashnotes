# Gravis MouseStick II

The Gravis MouseStick II first appears with address 0x3 and handler ID 0x01, but the Gravis cdev changes the handler ID to 0x23 when it loads, which enables the following output formats.

## Talk 0

| Byte    | Description                                      |
| ------- | ------------------------------------------------ |
| 1st-2nd | Mouse movement data                              |
| 3rd-4th | Joystick X position (2's complement, big endian) |
| 5th-6th | Joystick Y position (2's complement, big endian) |
| 7th     | Joystick buttons                                 |

### Mouse Movement Data

| Bit  | Description                                                             |
| ---- | ----------------------------------------------------------------------- |
| 15   | Mouse button status (0 = down, 1 = up)                                  |
| 14-8 | Mouse Y movement counts (2's complement, negative up, positive down)    |
| 7    | Always 1                                                                |
| 6-0  | Mouse X movement counts (2's complement, negative left, positive right) |

### Joystick Buttons

0 = down, 1 = up

| Bit | Description                |
| --- | -------------------------- |
| 7-5 | Always 1                   |
| 4   | Right button atop joystick |
| 3   | Left button atop joystick  |
| 2   | Trigger button             |
| 1   | Bottom circular button     |
| 0   | Top circular button        |

## Talk 1

This may be an identifier of the type of Gravis joystick, allowing different joysticks to be represented by the 0x23 handler ID.

| Byte | Value Returned By MouseStick II |
| ---- | ------------------------------- |
| 1st  | 0x03                            |
| 2nd  | 0x00                            |

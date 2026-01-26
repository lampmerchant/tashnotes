# Edmark TouchWindow

The TouchWindow appears on the ADB at address 0x4 with handler 0x2F.  Apart from the standard use of register 3, the device only communicates using Talk 0.


## Talk 0

When the TouchWindow has data to send, Talk 0 is responded to with a four-byte packet:

| Bit(s) | Description                                                 |
| ------ | ----------------------------------------------------------- |
| 31-25  | Bits 8-2 of X position                                      |
| 24     | Always 1                                                    |
| 23-22  | Bits 1-0 of X position                                      |
| 21-17  | Always 0                                                    |
| 16     | Always 1                                                    |
| 15-9   | Bits 8-2 of Y position                                      |
| 8      | Always 1                                                    |
| 7-6    | Bits 1-0 of Y position                                      |
| 5      | 1 if left button is down                                    |
| 4      | 1 if right button is down                                   |
| 3      | 1 if window is not being touched and neither button is down |
| 2-0    | Always 0                                                    |

X and Y position increase from left to right and top to bottom, respectively.  Observed X and Y values ranged from 0x094 to 0x1C0 and 0x08E to 0x16A, respectively.

When the window is no longer being touched and both buttons are released, the TouchWindow responds to Talk 0 with a packet where the X and Y positions are 0 and bit 3 is set; thereafter, Talk 0 is met with no response until the window is touched or a button is pressed.

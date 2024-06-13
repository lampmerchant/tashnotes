# Microsoft SideWinder 3D Pro

The joystick appears on the ADB as two devices - one with address 0x3 and handler ID 0x01, one with address 0x4 and handler ID 0x5D.  The device with handler ID 0x01 can be switched to handler ID 0x02 (for 200 cpi mouse emulation), but the device with handler ID 0x5D is used to read the joystick state through Talk 0 as described below.


## Talk 0

Talk 0 gives the current state of the joystick's analog and digital controls in a 7-byte response:

| Bit(s) | Description                          | Encoding                                                     |
| ------ | ------------------------------------ | ------------------------------------------------------------ |
| 55     | Bottom left base button              | 0 = down, 1 = up                                             |
| 54     | Bottom right base button             | 0 = down, 1 = up                                             |
| 53     | Top right base button                | 0 = down, 1 = up                                             |
| 52     | Top left base button                 | 0 = down, 1 = up                                             |
| 51-42  | Handle horizontal position           | 0x000 = left, 0x3FF = right                                  |
| 41-32  | Handle vertical position             | 0x000 = up, 0x3FF = down                                     |
| 31-28  | Directional hat position             | 0 = center, 1 = up, 2 = up-right, 3 = right, ... 8 = up-left |
| 27-25  | Always 0                             |                                                              |
| 24-16  | Rudder (twist position)              | 0x000 = anticlockwise, 0x1FF = clockwise                     |
| 15     | Bottom handle side button            | 0 = down, 1 = up                                             |
| 14     | Top handle side button               | 0 = down, 1 = up                                             |
| 13     | Top handle button (opposite trigger) | 0 = down, 1 = up                                             |
| 12     | Trigger button                       | 0 = down, 1 = up                                             |
| 11-10  | Always 0                             |                                                              |
| 9-0    | Throttle                             | 0x000 = up, 0x3FF = down                                     |


## Thanks

*Thanks to demik on #68kmla for compiling this information!*

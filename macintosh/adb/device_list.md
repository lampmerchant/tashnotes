# ADB Device List

This is an obviously-incomplete list of known ADB devices and their identifying information (default address and default handler ID).

| Address | Handler ID | Make                   | Model Name              | Model Number | Description                                 |
| ------- | ---------- | ---------------------- | ----------------------- | ------------ | ------------------------------------------- |
| 0x1     | 0x34       | Rainbow Technologies   | Sentinel Eve3           |              | License dongle                              |
| 0x2     | 0x01       | Apple                  | Keyboard                | M0116        | Keyboard                                    |
| 0x2     | 0x02       | Apple                  | Extended Keyboard       | M0115        | Keyboard                                    |
| 0x2     | 0x02       | Apple                  | Extended Keyboard II    | M3501        | Keyboard                                    |
| 0x2     | 0x02       | Apple                  | AppleDesign Keyboard    | M2980        | Keyboard                                    |
| 0x2     | 0x08       | Apple                  | Keyboard II             | M0487        | Keyboard                                    |
| 0x3     | 0x1        | Apple                  | Mouse                   | A9M0031      | Mouse                                       |
| 0x3     | 0x32       | Kensington             | Turbo Mouse             | 64210        | Trackball 4-button device[^3]               |
| 0x4     | 0x3A       | Wacom                  | ArtZ 6x8 / Digitizer II | UD-0608-A    | Graphics tablet[^3]                         |
| 0x5[^1] | 0x36       | Global Village         | TelePort ADB            | A300         | 2400-baud modem                             |
| 0x7     | 0x02       | Apple                  | Adjustable Keyboard     | M1242        | Media keys device of AAK                    |
| 0x7     | 0x22       | Sophisticated Circuits | PowerKey                | PK-1         | Relay-controlled power strip                |
| 0x7     | 0x35       | Neotech                | Colour Adapter Module   |              | Module for Neotech Image Grabber NuBus card |
| 0x7     | 0x41       | JLCooper Electronics   | Media Control Station   | 300007       | Jog dial and media control device           |
| 0x7[^2] | 0x79[^2]   | LaCie                  | FM Radio                |              | FM radio                                    |
| 0x7     | 0x7A       | BeeHive Technologies   | ADB I/O                 |              | Analog and digital I/O interface            |

[^1]: Also known to use address 0x7.
[^2]: [Source](https://vintagegeek.wordpress.com/2021/01/03/lacie-fm-radio-tuner-for-system-7-0-to-os9-macintosh-w-adb-port/#comment-1107)
[^3]: Also emulates a standard mouse on address 0x3.

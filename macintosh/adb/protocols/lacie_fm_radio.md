# LaCie FM Radio

This document contains experimentally-observed behavior of the ADB protocol of the LaCie FM Radio.  It contains enough information to satisfactorally emulate the device, however, a few unknowns exist.

The FM Radio's default address is 0x7 and its default handler ID is 0x79.

## Register 1

Register 1 sets the volume and tone controls and is three bytes in length.

| Bit   | Description                                                         |
| ----- | ------------------------------------------------------------------- |
| 23-16 | Volume, apparently set in increments of 0x11 by the application     |
| 15    | Always zero                                                         |
| 14-12 | Treble                                                              |
| 11    | Always zero                                                         |
| 10-8  | Bass                                                                |
| 7     | Mute, active high                                                   |
| 6-5   | Sound fading, 0 = normal, 1 = low, 2 = medium, 3 = high             |
| 4-1   | Undetermined, apparently always zero                                |
| 0     | Undetermined, initially zero but always set high by the application |

Talk 1 has a fourth byte appended which is the firmware revision of the FM Radio.

## Register 2

Register 2 sets the FM station and is three bytes in length.  Talk 0 has a response (apparently immediately, but possibly only when tuning is finished) to every Listen 2 that is identical to Talk 2.

### First Byte

In a Listen 2 command, the first byte seems to always be 0x41.  In a Talk command, the bits of the first byte are as follows:

| Bit | Description                            |
| --- | -------------------------------------- |
| 7   | FM stereo indicator, active high       |
| 6-1 | Undetermined, apparently always zero   |
| 0   | Signal strength indicator, active high |

### Second and Third Bytes

The second and third bytes indicate the FM station.  Starting from a base of -10.7 MHz, the following values are added to it:

| Bit | Value                     |
| --- | ------------------------- |
| 15  | Undetermined, always zero |
| 14  | Undetermined, always zero |
| 13  | 102.4 MHz                 |
| 12  | 51.2 MHz                  |
| 11  | 25.6 MHz                  |
| 10  | 12.8 MHz                  |
| 9   | 6.4 MHz                   |
| 8   | 3.2 MHz                   |
| 7   | 1.6 MHz                   |
| 6   | 800 kHz                   |
| 5   | 400 kHz                   |
| 4   | 200 kHz                   |
| 3   | 100 kHz                   |
| 2   | 50 kHz                    |
| 1   | Undetermined, always zero |
| 0   | Undetermined, always zero |

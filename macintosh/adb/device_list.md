# ADB Device List

This is an obviously-incomplete list of known ADB devices and their identifying information (default address and handler ID).

## License Dongles

Original Address 0x1

| Handler ID | Make                 | Model Name    | Model Number | Description    | Source   |
| ---------- | -------------------- | ------------- | ------------ | -------------- | -------- |
| 0x34       | Rainbow Technologies | Sentinel Eve3 |              | License dongle | Tashtari |

## Encoded Devices

Original Address 0x2

| Handler ID | Make            | Model Name                         | Model Number | Description     | Source              |
| ---------- | --------------- | ---------------------------------- | ------------ | --------------- | ------------------- |
| 0x01       | Apple           | Keyboard                           | M0116        | Keyboard        | Tashtari            |
| 0x02       | Apple           | Extended Keyboard                  | M0115        | Keyboard        | Tashtari            |
| 0x02       | Apple           | Extended Keyboard II               | M3501        | Keyboard        | Tashtari            |
| 0x02       | Apple           | AppleDesign Keyboard               | M2980        | Keyboard        | Tashtari            |
| 0x02       | Alps            | GlidePoint Keypad                  | FGS018-00    | Keypad/trackpad | Tashtari            |
| 0x04       | Apple           | Keyboard ISO                       | M0118        | Keyboard        | [Link][1] [Link][2] |
| 0x05       | Apple           | Extended Keyboard ISO              | M0115        | Keyboard        | [Link][1] [Link][2] |
| 0x05       | Apple           | Extended Keyboard II ISO           | M3501        | Keyboard        | demik@68kMLA        |
| 0x05       | Apple           | AppleDesign Keyboard ISO           | M2980        | Keyboard        | demik@68kMLA        |
| 0x06       | Apple           | Portable Keyboard                  | ?            | Keyboard        | [Link][1]           |
| 0x07       | Apple           | Portable Keyboard ISO              | ?            | Keyboard        | [Link][1]           |
| 0x08       | Apple           | Keyboard II                        | M0487        | Keyboard        | Tashtari            |
| 0x09       | Apple           | Keyboard II ISO                    | ?            | Keyboard        | [Link][1]           |
| 0x0A/0x0C  | Apple           | PowerBook 100/140/170 Keyboard     | ?            | Keyboard        | [Link][1]           |
| 0x0C       | Apple           | PowerBook Duo 230 Keyboard         | ?            | Keyboard        | mdeverhart@68kMLA   |
| 0x0D       | Apple           | PowerBook 100/140/170 Keyboard ISO | ?            | Keyboard        | [Link][1]           |
| 0x0E       | Apple           | Adjustable Keyboard Keypad         | M1242        | Keypad          | [Link][1] [Link][2] |
| 0x10       | Apple           | Adjustable Keyboard                | M1242        | Keyboard        | [Link][1] [Link][2] |
| 0x11       | Apple           | Adjustable Keyboard ISO            | ?            | Keyboard        | [Link][1]           |
| 0x12       | Apple           | Adjustable Keyboard JIS            | ?            | Keyboard        | [Link][2]           |
| 0x16       | Apple           | Keyboard II JIS                    | M0487        | Keyboard        | [Link][2]           |
| 0x34[^2]   | Advanced Gravis | Mac GamePad                        |              | Game controller | Tashtari            |

## Relative Pointing Devices

Original Address 0x3

| Handler ID   | Make            | Model Name                  | Model Number      | Description                 | Source            |
| ------------ | --------------- | --------------------------- | ----------------- | --------------------------- | ----------------- |
| 0x01         | Apple           | Apple Desktop Bus Mouse     | A9M0031           | Mouse                       | Tashtari          |
| 0x01         | Apple           | Apple Desktop Bus Mouse     | G5431             | Mouse                       | Tashtari          |
| 0x01         | Apple           | Apple Desktop Bus Mouse II  | M2706             | Mouse                       | Tashtari          |
| 0x01         | NeXT            | Mouse                       | N8003             | Mouse                       | demik@68kMLA      |
| 0x01         | Logitech        | TrackMan Stationary Mouse   | T-AE2 / 804071-00 | Trackball                   | Tashtari          |
| 0x01         | MacAlly         | One Button Mouse            |                   | Mouse                       | Tashtari          |
| 0x02         | Apple           | PowerBook Duo 230 Trackball |                   | Trackball                   | mdeverhart@68kMLA |
| 0x23         | Advanced Gravis | MouseStick GMPU             |                   | 3-button joystick interface | Tashtari          |
| 0x23[^1]     | Advanced Gravis | MouseStick II               |                   | 5-button joystick           | Tashtari          |
| 0x2F         | MicroSpeed      | MacTRAC 2.0                 | 9507              | Trackball                   | Tashtari          |
| 0x32[^3]     | Kensington      | Mouse-in-a-Box              | 65206             | Mouse                       | demik@68kMLA      |
| 0x32[^3]     | Kensington      | Mouse ADB                   | 64205             | Mouse                       | Tashtari          |
| 0x32[^3]     | Kensington      | Turbo Mouse                 | 64210             | Trackball 4-button device   | Tashtari          |
| 0x40[^1]     | Alps            | GlidePoint                  |                   | Trackpad                    | mdeverhart@68kMLA |
| 0x40[^1][^4] | Alps            | GlidePoint Keypad           | FGS018-00         | Keypad/trackpad             | Tashtari          |
| 0x46[^1]     | Atmark          | Pippin Controller           |                   | Game controller             | Tashtari          |
| 0x4E[^1]     | Advanced Gravis | Blackhawk                   |                   | 4-button joystick           | demik@68kMLA      |
| 0x4E[^1][^5] | Advanced Gravis | Firebird                    |                   | 17-button joystick          | Tashtari          |

## Absolute Pointing Devices

Original Address 0x4

| Handler ID | Make  | Model Name              | Model Number | Description     | Source            |
| ---------- | ----- | ----------------------- | ------------ | --------------- | ----------------- |
| 0x21       | Kurta | IS/ADB 12x17            | ?            | Graphics tablet | ArmorAlley@68kMLA |
| 0x3A[^3]   | Wacom | ArtPad II               | KT-0405-A    | Graphics tablet | ArmorAlley@68kMLA |
| 0x3A[^3]   | Wacom | ArtZ 6x8 / Digitizer II | UD-0608-A    | Graphics tablet | Tashtari          |

## Low-Speed Serial Devices

Original Address 0x5

| Handler ID | Make           | Model Name   | Model Number | Description              | Source   |
| ---------- | -------------- | ------------ | ------------ | ------------------------ | -------- |
| 0x03       | MacAlly        | JoyStick     |              | Joystick                 | Tashtari |
| 0x36[^7]   | Global Village | TelePort ADB | A300         | 2400-baud modem          | Tashtari |
| 0x37       | Global Village | TelePort ADB | A400         | 4800/9600-baud fax/modem | Tashtari |

## PowerBook Duo Charger

Original Address 0x6

| Handler ID | Make  | Model Name            | Model Number | Description           | Source            |
| ---------- | ----- | --------------------- | ------------ | --------------------- | ----------------- |
| 0x01       | Apple | PowerBook Duo Charger | M1812        | PowerBook Duo Charger | mdeverhart@68kMLA |

## Other Devices

Original Address 0x7

| Handler ID | Make                   | Model Name            | Model Number | Description                                 | Source               |
| ---------- | ---------------------- | --------------------- | ------------ | ------------------------------------------- | -------------------- |
| 0x02       | Apple                  | Adjustable Keyboard   | M1242        | Media keys device of AAK                    | Velociraptors@68kMLA |
| 0x22       | Sophisticated Circuits | PowerKey              | PK-1         | Relay-controlled power strip                | Tashtari             |
| 0x35       | Neotech                | Colour Adapter Module |              | Module for Neotech Image Grabber NuBus card | Jockelill@68kMLA     |
| 0x41       | JLCooper Electronics   | Media Control Station | 300007       | Jog dial and media control device           | Tashtari             |
| 0x67       | HASP                   | MacHASP-M             | KSWAVESO B   | License dongle                              | ArmorAlley@68kMLA    |
| 0x79       | LaCie                  | FM Radio              |              | FM radio                                    | Tashtari             |
| 0x7A       | BeeHive Technologies   | ADB I/O               |              | Analog and digital I/O interface            | [Link][3]            |
| 0x87       | Sophisticated Circuits | PowerKey Rebound!     | PKRB-M       | Watchdog timer                              | Tashtari             |

[1]: https://github.com/elliotnunn/boot3/blob/f5582f37d04819abec51525ade1e021858a914e1/OS/Keyboard/Kbd.r
[2]: https://github.com/tmk/tmk_keyboard/wiki/Apple-Desktop-Bus#keyboard-handler-id
[3]: https://web.archive.org/web/19980501032801/http://www.bzzzzzz.com:80/BeeHive/ADB_IO/Downloads/ADB_IO_manual.pdf

[^1]: Initially has handler ID 0x01.
[^2]: Initially has handler ID 0x02.
[^3]: Also emulates a standard mouse on address 0x3.
[^4]: Only mouse device can be set to handler ID 0x40.
[^5]: Also responds to handler ID 0x23.
[^7]: Also known to use default address 0x7.

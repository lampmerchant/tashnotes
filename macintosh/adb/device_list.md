# ADB Device List

This is an obviously-incomplete list of known ADB devices and their identifying information (default address and handler ID).

## License Dongles

Original Address 0x1

| Address | Handler ID | Make                 | Model Name    | Model Number | Description    |
| ------- | ---------- | -------------------- | ------------- | ------------ | -------------- |
| 0x1     | 0x34       | Rainbow Technologies | Sentinel Eve3 |              | License dongle |

## Encoded Devices

Original Address 0x2

| Handler ID    | Make  | Model Name                         | Model Number | Description |
| ------------- | ----- | ---------------------------------- | ------------ | ----------- |
| 0x01          | Apple | Keyboard                           | M0116        | Keyboard    |
| 0x02          | Apple | Extended Keyboard                  | M0115        | Keyboard    |
| 0x02          | Apple | Extended Keyboard II               | M3501        | Keyboard    |
| 0x02          | Apple | AppleDesign Keyboard               | M2980        | Keyboard    |
| 0x04[^5]      | Apple | Keyboard ISO                       | M0118[^6]    | Keyboard    |
| 0x05[^5]      | Apple | Extended Keyboard ISO              | M0115[^6]    | Keyboard    |
| 0x05[^9]      | Apple | Extended Keyboard II ISO           | M3501        | Keyboard    |
| 0x05[^9]      | Apple | AppleDesign Keyboard ISO           | M2980        | Keyboard    |
| 0x06[^5]      | Apple | Portable Keyboard                  | ?            | Keyboard    |
| 0x07[^5]      | Apple | Portable Keyboard ISO              | ?            | Keyboard    |
| 0x08          | Apple | Keyboard II                        | M0487        | Keyboard    |
| 0x09[^5]      | Apple | Keyboard II ISO                    | ?            | Keyboard    |
| 0x0A/0x0C[^5] | Apple | Powerbook 100/140/170 Keyboard     | ?            | Keyboard    |
| 0x0D[^5]      | Apple | Powerbook 100/140/170 Keyboard ISO | ?            | Keyboard    |
| 0x0E[^5]      | Apple | Adjustable Keyboard Keypad         | M1242[^6]    | Keypad      |
| 0x10[^5]      | Apple | Adjustable Keyboard                | M1242[^6]    | Keyboard    |
| 0x11[^5]      | Apple | Adjustable Keyboard ISO            | ?            | Keyboard    |
| 0x12[^6]      | Apple | Adjustable Keyboard JIS            | ?            | Keyboard    |
| 0x16[^6]      | Apple | Keyboard II JIS                    | M0487[^6]    | Keyboard    |

## Relative Pointing Devices

Original Address 0x3

| Handler ID   | Make            | Model Name        | Model Number | Description                 |
| ------------ | --------------- | ----------------- | ------------ | --------------------------- |
| 0x01         | Apple           | Mouse             | A9M0031      | Mouse                       |
| 0x01         | Apple           | Mouse             | G5431        | Mouse                       |
| 0x01         | Apple           | Mouse II          | M2706        | Mouse                       |
| 0x01[^9]     | NeXT            | Mouse             | N8003        | Mouse                       |
| 0x23         | Advanced Gravis | MouseStick GMPU   |              | 3-button joystick interface |
| 0x23[^4]     | Advanced Gravis | MouseStick II     |              | 5-button joystick           |
| 0x2F         | MicroSpeed      | MacTRAC 2.0       | FUUTB02      | Trackball                   |
| 0x32[^3][^7] | Kensington      | Turbo Mouse       | 64210        | Trackball 4-button device   |
| 0x46[^4]     | Atmark          | Pippin Controller |              | Game controller             |

## Absolute Pointing Devices

Original Address 0x4

| Handler ID | Make  | Model Name              | Model Number | Description     |
| ---------- | ----- | ----------------------- | ------------ | --------------- |
| 0x3A[^3]   | Wacom | ArtZ 6x8 / Digitizer II | UD-0608-A    | Graphics tablet |

## Low-Speed Serial Devices

Original Address 0x5

| Handler ID | Make           | Model Name   | Model Number | Description     |
| ---------- | -------------- | ------------ | ------------ | --------------- |
| 0x36[^1]   | Global Village | TelePort ADB | A300         | 2400-baud modem |

## Other Devices

Original Address 0x7

| Handler ID | Make                   | Model Name            | Model Number | Description                                 |
| ---------- | ---------------------- | --------------------- | ------------ | ------------------------------------------- |
| 0x02[^8]   | Apple                  | Adjustable Keyboard   | M1242        | Media keys device of AAK                    |
| 0x22       | Sophisticated Circuits | PowerKey              | PK-1         | Relay-controlled power strip                |
| 0x35[^7]   | Neotech                | Colour Adapter Module |              | Module for Neotech Image Grabber NuBus card |
| 0x41       | JLCooper Electronics   | Media Control Station | 300007       | Jog dial and media control device           |
| 0x79[^2]   | LaCie                  | FM Radio              | ?            | FM radio                                    |
| 0x7A[^10]  | BeeHive Technologies   | ADB I/O               |              | Analog and digital I/O interface            |
| 0x87       | Sophisticated Circuits | PowerKey Rebound!     | PKRB-M       | Watchdog timer                              |

[^1]: Also known to use default address 0x7.
[^2]: [Source](https://vintagegeek.wordpress.com/2021/01/03/lacie-fm-radio-tuner-for-system-7-0-to-os9-macintosh-w-adb-port/#comment-1107)
[^3]: Also emulates a standard mouse on address 0x3.
[^4]: Initially has handler ID 0x01.
[^5]: [Source](https://github.com/elliotnunn/boot3/blob/f5582f37d04819abec51525ade1e021858a914e1/OS/Keyboard/Kbd.r)
[^6]: [Source](https://github.com/tmk/tmk_keyboard/wiki/Apple-Desktop-Bus#keyboard-handler-id)
[^7]: Source Jockelill on 68kMLA
[^8]: Source Velociraptors on 68kMLA
[^9]: Source demik on 68kMLA
[^10]: [Source](https://web.archive.org/web/19980501032801/http://www.bzzzzzz.com:80/BeeHive/ADB_IO/Downloads/ADB_IO_manual.pdf)

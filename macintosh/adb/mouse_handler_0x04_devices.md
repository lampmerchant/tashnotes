# Handler 0x04 Relative Pointing Devices

## Device Characteristics

| Device                              | Identifier          | Resolution     | Class            | Buttons | Source            |
| ----------------------------------- | ------------------- | -------------- | ---------------- | ------- | ----------------- |
| Alps GlidePoint Keypad              | 0x414C5033 ("ALP3") | 480 units/inch | 0x01 (Mouse)     | 3[^2]   | Tashtari          |
| Apple Desktop Bus Mouse II          | 0x40323030 ("@200") | 200 units/inch | 0x01 (Mouse)     | 1       | demik@68kMLA      |
| Kensington Mouse-in-a-Box           | 0x4B4D4C31 ("KML1") | 400 units/inch | 0x01 (Mouse)     | 2[^1]   | demik@68kMLA      |
| Kensington Mouse ADB                | 0x4B4D4C31 ("KML1") | 400 units/inch | 0x01 (Mouse)     | 2       | Tashtari          |
| Kensington Thinking Mouse           | 0x4B4D4C31 ("KML1") | 400 units/inch | 0x01 (Mouse)     | 4       | demik@68kMLA      |
| Kensington Turbo Mouse              | 0x4B4D4C31 ("KML1") | 200 units/inch | 0x02 (Trackball) | 4       | Tashtari          |
| Logitech MouseMan Macintosh Version | 0x4C543031 ("LT01") | 400 units/inch | 0x01 (Mouse)     | 3       | nyef@68kMLA       |
| Logitech TrackMan                   | 0x4C543031 ("LT01") | 200 units/inch | 0x02 (Trackball) | 3       | Tashtari          |
| MacAlly One Button Mouse            | 0x4B4F4954 ("KOIT") | 200 units/inch | 0x01 (Mouse)     | 1       | Tashtari          |
| PowerBook 520 Trackpad              | 0x74706164 ("tpad") | 387 units/inch | 0x03[^3]         | 2[^1]   | demik@68kMLA      |
| PowerBook Duo 230 Trackball         | 0x50472645 ("PG&E") | 100 units/inch | 0x02 (Trackball) | 2       | mdeverhart@68kMLA |

[^1]: Device actually has only one button
[^2]: Device actually has only two buttons
[^3]: Class 0x03 is not defined by Technical Note HW01 (dated 28 Sep 1998)

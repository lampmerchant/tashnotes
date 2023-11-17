# MicroSpeed MacTRAC 2.0

The MacTRAC 2.0 presents itself on the ADB with address 0x3 and handler 0x2F.  The handler cannot be changed.

## Talk 0

Talk 0 generally follows the standard Macintosh mouse protocol, with the vertical delta in bits 14-8, the horizontal delta in bits 6-0, and the button state in bits 15 and 7, however, the button state does not correspond directly to the state of the trackball's three buttons.

With the middle button up:

| Left Button | Right Button | Bit 15 | Bit 7 |
| ----------- | ------------ | ------ | ----- |
| Up          | Up           | 1      | 1     |
| Down        | Up           | 0      | 1     |
| Up          | Down         | 0      | 0     |
| Down        | Down         | 1      | 0     |

Pressing and releasing the middle button without pressing the left or right button locks the left button down until any button is pressed and released.

With the middle button held down, bit 15 is 1 and bit 7 is 0, as though both the left and right buttons were being held down, and instead of the movement deltas, the button state is reflected in bits 8 and 0 while bits 14-9 and 6-1 are always 0:

| Left Button | Right Button | Bit 8 | Bit 0 |
| ----------- | ------------ | ----- | ----- |
| Up          | Up           | 0     | 0     |
| Down        | Up           | 0     | 1     |
| Up          | Down         | 1     | 0     |
| Down        | Down         | 1     | 1     |

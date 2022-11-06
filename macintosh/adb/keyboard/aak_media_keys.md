# Apple Adjustable Keyboard Media Keys

The AAK appears as two devices, a keyboard (with original address 0x2 and handler ID 0x10) and an auxillary device (with original address 0x7 and handler ID 0x2) which handles the media keys (volume up/down, mute, and mic.)

The auxillary device's registers 1 and 2 appear to mirror those of the keyboard.  Register 0 behaves like a keyboard (returns two key events per Talk command, MSB clear on press and set on release) but with only four keys:

| Key      | Code |
| -------- | ---- |
| Volume ↑ | 0x03 |
| Volume ↓ | 0x02 |
| Mute     | 0x01 |
| Mic      | 0x00 |

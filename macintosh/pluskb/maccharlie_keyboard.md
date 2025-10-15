# Dayna MacCharlie Keyboard Codes

The Dayna MacCharlie keyboard extension imitates and extends the protocol of the M0120 keypad: key event codes are preceded with `0x79`.


## Numeric Keypad Keys

The numeric keypad key event codes are the same as on the M0120 keypad, but the keys are marked with additional functions.  Functions common to Macintosh and MacCharlie are black, functions unique to Macintosh are red, functions unique to MacCharlie are blue.

| Code   | Common Function | Macintosh Function | MacCharlie Function |
| ------ | --------------- | ------------------ | ------------------- |
| `0x0F` |                 | Clear              | PrtSc *             |
| `0x1D` |                 | -                  | Esc                 |
| `0x0D` |                 | + ←                | Num Lock            |
| `0x05` |                 | * →                | Scroll Lock / Break |
| `0x33` | 7               |                    | Home                |
| `0x37` | 8               |                    | ↑                   |
| `0x39` | 9               |                    | Page Up             |
| `0x1B` |                 | / ↑                | -                   |
| `0x2D` | 4               |                    | ←                   |
| `0x2F` | 5               |                    |                     |
| `0x31` | 6               |                    | →                   |
| `0x11` |                 | , ↓                | +                   |
| `0x27` | 1               |                    | End                 |
| `0x29` | 2               |                    | ↓                   |
| `0x2B` | 3               |                    | Page Down           |
| `0x25` | 0               |                    | Insert              |
| `0x03` | .               |                    | Delete              |
| `0x19` | Enter           |                    |                     |


## Function Keys

The 10 function keys on the MacCharlie keyboard extension act as additional keys on the keypad.

| Code   | MacCharlie Function |
| ------ | ------------------- |
| `0x41` | F1                  |
| `0x43` | F2                  |
| `0x45` | F3                  |
| `0x47` | F4                  |
| `0x49` | F5                  |
| `0x4B` | F6                  |
| `0x4D` | F7                  |
| `0x4F` | F8                  |
| `0x51` | F9                  |
| `0x53` | F10                 |


## Thanks

Thanks to KennyPowers on TinkerDifferent for this information!

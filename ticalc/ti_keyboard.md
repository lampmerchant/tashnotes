# TI Graphing Calculator Keyboard

## D-Bus Protocol

With every key event, the keyboard transmits the following D-Bus events:
 - 0xE0
 - Error/Abort (both lines pulled low) for 80 µs
 - 0x01
 - Key/Event Code (see below)

Typematic repeat is handled by the calculator; the keyboard only sends one
event per key press/release.

While Caps Lock is active, the MSb of all key/event codes is set, starting
with the event where Caps Lock is pressed to activate it and ending with the
event where Caps Lock is pressed to deactivate it.


## Key/Event Codes

### Normal Keys

| Code | Description     |
| ---- | --------------- |
| 0x00 | (keys released) |
| 0x01 | `!` `1`         |
| 0x02 | `~` `\``        |
| 0x03 | Tab             |
| 0x04 | `Q`             |
| 0x05 | `A`             |
| 0x06 | Caps Lock       |
| 0x07 | `Z`             |
| 0x08 | Del             |
| 0x09 | `@` `2`         |
| 0x0A | `#` `3`         |
| 0x0B | `E`             |
| 0x0C | `W`             |
| 0x0D | `S`             |
| 0x0E | `X`             |
| 0x0F | `C`             |
| 0x10 | Page Up         |
| 0x11 | `$` `4`         |
| 0x12 | ?               |
| 0x13 | `R`             |
| 0x14 | `T`             |
| 0x15 | `D`             |
| 0x16 | `F`             |
| 0x17 | `V`             |
| 0x18 | Page Down       |
| 0x19 | `%` `5`         |
| 0x1A | ?               |
| 0x1B | `U`             |
| 0x1C | `Y`             |
| 0x1D | `H`             |
| 0x1E | `G`             |
| 0x1F | `B`             |
| 0x20 | Space           |
| 0x21 | `^` `6`         |
| 0x22 | `&` `7`         |
| 0x23 | `I`             |
| 0x24 | ?               |
| 0x25 | `J`             |
| 0x26 | `K`             |
| 0x27 | `N`             |
| 0x28 | ?               |
| 0x29 | `(` `9`         |
| 0x2A | `*` `8`         |
| 0x2B | `O`             |
| 0x2C | ?               |
| 0x2D | `:` `;`         |
| 0x2E | `L`             |
| 0x2F | `M`             |
| 0x30 | ?               |
| 0x31 | `)` `0`         |
| 0x32 | `_` `-`         |
| 0x33 | `P`             |
| 0x34 | `{` `[`         |
| 0x35 | `"` `'`         |
| 0x36 | ?               |
| 0x37 | `<` `,`         |
| 0x38 | ?               |
| 0x39 | Backspace       |
| 0x3A | `+` `=`         |
| 0x3B | `|` `\`         |
| 0x3C | `}` `]`         |
| 0x3D | Enter           |
| 0x3E | `?` `/`         |
| 0x3F | `>` `.`         |
| 0x40 | ?               |


### Modifier Keys

When a modifier key is pressed or released, an event byte of the following
form is sent:

| Bit | Description              |
| --- | ------------------------ |
| 7   | Always 0                 |
| 6   | Always 1                 |
| 5   | Always 0                 |
| 4   | Always 1                 |
| 3   | 1 if ? is Down           |
| 2   | 1 if ? is Down           |
| 1   | 1 if Right Shift is Down |
| 0   | 1 if Left Shift is Down  |

# JLCooper Media Control Station

This document contains experimentally-observed behavior of the JLCooper Media Control Station. It contains enough information to program the device, however, some unknowns exist.

The device's default address is 0x7 and its default handler ID is 0x41, though much of the effect of using its controls is given as responses to Talk 0 commands on addresses 0x2 and 0x3, which are assumed to represent the keyboard and mouse, respectively.

## Selecting Banks

This is done with Listen 2 with the following payloads:

| Payload   | Bank Selected  |
| --------- | -------------- |
| 0x80 0x00 | Default bank 0 |
| 0xA0 0x00 | Default bank 1 |
| 0xC0 0x00 | Custom bank 0  |
| 0xE0 0x00 | Custom bank 1  |

Custom banks are stored in non-volatile memory.  Custom Bank 0 is selected on startup regardless of the last selected bank.

### Default Bank 0

Default Bank 0 contains idiosyncratic codes, most of which are read through Talk 0 commands on the MCS's own address.

### Default Bank 1

Default Bank 1 assigns the jog wheel to horizontal mouse movement and assigns the buttons as such:

| Button   | No Modifier | Shift    | Option    | Both            |
| -------- | ----------- | -------- | --------- | --------------- |
| (<<)     | F1          | Shift+F1 | Option+F1 | Shift+Option+F1 |
| (>>)     | F2          | Shift+F2 | Option+F2 | Shift+Option+F2 |
| (Stop)   | F3          | Shift+F3 | Option+F3 | Shift+Option+F3 |
| (Play)   | F4          | Shift+F4 | Option+F4 | Shift+Option+F4 |
| (Record) | F5          | Shift+F5 | Option+F5 | Shift+Option+F5 |

## Custom Banks

### Reading

When a custom bank is selected, 12 successive Talk 2 commands will read out the contents of the custom bank, four bytes at a time.

### Writing

Writing a custom bank is done with Listen 2 commands of five bytes each.  The first byte controls where the next four are to be written:

| Bit | Description                         |
| --- | ----------------------------------- |
| 7   | Always 0                            |
| 6   | Number of custom bank to be written |
| 5:0 | Offset within bank (multiple of 4)  |

### Memory Layout

| Offset | +0                           | +1                           | +2                           | +3                           |
| ------ | ---------------------------- | ---------------------------- | ---------------------------- | ---------------------------- |
| 0x00   | (<<) Modifier                | (<<) Key Code                | (>>) Modifier                | (>>) Key Code                |
| 0x04   | (Stop) Modifier              | (Stop) Key Code              | (Play) Modifier              | (Play) Key Code              |
| 0x08   | (Record) Modifier            | (Record) Key Code            | Shift+(<<) Modifier          | Shift+(<<) Key Code          |
| 0x0C   | Shift+(>>) Modifier          | Shift+(>>) Key Code          | Shift+(Stop) Modifier        | Shift+(Stop) Key Code        |
| 0x10   | Shift+(Play) Modifier        | Shift+(Play) Key Code        | Shift+(Record) Modifier      | Shift+(Record) Key Code      |
| 0x14   | Option+(<<) Modifier         | Option+(<<) Key Code         | Option+(>>) Modifier         | Option+(>>) Key Code         |
| 0x18   | Option+(Stop) Modifier       | Option+(Stop) Key Code       | Option+(Play) Modifier       | Option+(Play) Key Code       |
| 0x1C   | Option+(Record) Modifier     | Option+(Record) Key Code     | Shift+Option+(<<) Modifier   | Shift+Option+(<<) Key Code   |
| 0x20   | Shift+Option+(>>) Modifier   | Shift+Option+(>>) Key Code   | Shift+Option+(Stop) Modifier | Shift+Option+(Stop) Key Code |
| 0x24   | Shift+Option+(Play) Modifier | Shift+Option+(Play) Key Code | Ignored                      | Ignored                      |
| 0x28   | Always 0x00                  | Jog Wheel Mode               | Always 0x00                  | Shift+Jog Wheel Mode         |
| 0x2C   | Always 0x00                  | Option+Jog Wheel Mode        | Always 0x00                  | Option+Shift+Jog Wheel Mode  |

### Modifier Key Bitmap

| Bit | Description  |
| --- | ------------ |
| 7:6 | Undetermined |
| 5   | Mouse button |
| 4   | Control      |
| 3   | Option       |
| 2   | Undetermined |
| 1   | Shift        |
| 0   | Command      |

### Jog Wheel Modes

| Mode | Description                                                                                                |
| ---- | ---------------------------------------------------------------------------------------------------------- |
| 0x00 | Jog wheel moves mouse horizontally                                                                         |
| 0x01 | Jog wheel moves mouse vertically                                                                           |
| 0x02 | Jog wheel presses 4 on numeric keypad when turned anticlockwise, + on numeric keypad when turned clockwise |
| 0x03 | Jog wheel moves mouse horizontally while holding down command key                                          |
| 0x05 | Talk 0 produces 0x3F 0xFF 0xFF when turned anticlockwise, 0x3F 0x00 0x01 when turned clockwise             |
| 0x06 | Talk 0 produces 0x3F plus 16-bit relative numbers (negative for anticlockwise, positive for clockwise)     |
| 0x07 | Jog wheel in apparent shuttle mode, behavior not understood                                                |
| 0x08 | Jog wheel presses left arrow when turned anticlockwise, right arrow when turned clockwise                  |

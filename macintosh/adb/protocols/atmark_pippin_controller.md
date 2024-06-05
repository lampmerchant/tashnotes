# Atmark Pippin Controller

## Mouse Mode

The controller initially presents itself on the ADB on address 0x3 with handler ID 0x01 and emulates the standard 100 cpi mouse protocol, returning two bytes in response to Talk 0 when the data has changed since Talk 0 was last issued:

| Bit  | Description                  | Encoding                                            |
| ---- | ---------------------------- | --------------------------------------------------- |
| 15   | Left shoulder button status  | 0 = down, 1 = up                                    |
| 14-8 | Trackball Y movement counts  | Two's complement; negative = up, positive = down    |
| 7    | Right shoulder button status | 0 = down, 1 = up                                    |
| 6-0  | Trackball X movement counts  | Two's complement; negative = left, positive = right |

Pressing or releasing any other button will cause two bytes to be returned in response to Talk 0 as though the data had changed although the data returned does not reflect any change.

## Controller Mode

When its handler ID is set to 0x46, the response to Talk 0 is extended to four bytes:

| Bit   | Description                         | Encoding                                            |
| ----- | ----------------------------------- | --------------------------------------------------- |
| 31    | Left shoulder button status         | 0 = down, 1 = up                                    |
| 30-24 | Trackball Y movement counts         | Two's complement; negative = up, positive = down    |
| 23    | Right shoulder button status        | 0 = down, 1 = up                                    |
| 22-16 | Trackball X movement counts         | Two's complement; negative = left, positive = right |
| 15    | Green/1 button status               | 0 = down, 1 = up                                    |
| 14    | Red/2 button status                 | 0 = down, 1 = up                                    |
| 13    | Directional pad down button status  | 0 = down, 1 = up                                    |
| 12    | Directional pad right button status | 0 = down, 1 = up                                    |
| 11    | Directional pad left button status  | 0 = down, 1 = up                                    |
| 10    | Directional pad up button status    | 0 = down, 1 = up                                    |
| 9     | Yellow/4 button status              | 0 = down, 1 = up                                    |
| 8     | Blue/3 button status                | 0 = down, 1 = up                                    |
| 7-3   | Always 1                            |                                                     |
| 2     | Diamond button status               | 0 = down, 1 = up                                    |
| 1     | Circle button status                | 0 = down, 1 = up                                    |
| 0     | Square button status                | 0 = down, 1 = up                                    |

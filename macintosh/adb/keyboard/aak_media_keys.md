# Apple Adjustable Keyboard Media Keys

The Apple Adjustable Keyboard appears as two devices:

| Original Address | Handler ID | Function         |
| ---------------- | ---------- | ---------------- |
| 0x2              | 0x10       | Keyboard         |
| 0x7              | 0x02       | Media Key Device |

## Media Key Device

### Talk 0

Talk 0 behaves the same way as on a standard keyboard: it reports key press events as bytes with the MSB set for a release event and clear for a press event, and scan codes in the lower seven bits.  It reports a maximum of two such event bytes per Talk 0 request.  If it has only one event to report, the event byte is followed by an 0xFF byte; if it has no events to report, no reply is given.

The scan codes used by the media key device are as follows:

| Key      | Code |
| -------- | ---- |
| Volume ↑ | 0x03 |
| Volume ↓ | 0x02 |
| Mute     | 0x01 |
| Mic      | 0x00 |

### Talk 1

Talk 1 appears to always return 0xFF02.

### Talk 2

Talk 2 appears to always return 0xFFFF.

## Thanks

*Thanks to Velociraptors and treellama on #68kmla for their help in compiling and verifying this information!*

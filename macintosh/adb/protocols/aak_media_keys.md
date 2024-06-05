# Apple Adjustable Keyboard Media Keys

The Apple Adjustable Keyboard appears as two devices:

| Original Address | Handler ID | Function         |
| ---------------- | ---------- | ---------------- |
| 0x2              | 0x10       | Keyboard         |
| 0x7              | 0x02       | Media Key Device |

## Media Key Device

### Talk 0

Talk 0 behaves the same way as on a standard keyboard: it reports key events as bytes with the MSB set for a key release and clear for a key press, and scan codes in the lower seven bits.  It reports a maximum of two such event bytes per Talk 0 request.  If it has only one event to report, the event byte is followed by an 0xFF byte; if it has no events to report, no reply is given.

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

### System Software Support

Native support for the Volume Up/Down and Mute keys (but not the Mic key) was added with System 7.1 System Update 3.0[^1].  An extension called Record Button[^2] exists to provide support for the Mic key.

## Thanks

*Thanks to Velociraptors and treellama on #68kmla for their help in compiling and verifying this information!*

[^1]: <https://obsolete.macfixer.com/vintage-software/notes/System_Update_3.0_1.4MB.html>
[^2]: <http://www.geocities.ws/ddurant/ext.html>

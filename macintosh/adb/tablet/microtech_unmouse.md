# Microtech UnMouse

The UnMouse presents itself on address `0x4` with handler ID `0x02`.

## Talk 0

Talk 0 returns a six-byte response:

| Byte | Description                                                                 |
| ---- | --------------------------------------------------------------------------- |
| 0-1  | Absolute X coordinate of touch point from `0x000` (left) to `0x3FF` (right) |
| 2-3  | Absolute Y coordinate of touch point from `0x000` (bottom) to `0x3FF` (top) |
| 4    | `0xFF` when touch panel click is up, `0x3C` when held down                  |
| 5    | Red button and panel touch events, see below                                |

### Byte 5

The fifth byte of Talk 0 contains a bit field that reports on events associated with the UnMouse's red button and panel.  Note that the latter refers to when the user's finger makes contact with the touch surface, not when it presses hard enough to actuate the click.

| Bit | Description       |
| --- | ----------------- |
| 7-4 | Always 0          |
| 3-2 | Red button event  |
| 1-0 | Panel touch event |

| Event Bits | Description   |
| ---------- | ------------- |
| `00`       | Pressed       |
| `01`       | Still pressed |
| `10`       | Released      |
| `11`       | No event      |

## Talk 2

The UnMouse responds to Talk 2 with data payload `0x28 0x50 0x80 0x05 0xC0 0x00`.  The meaning of this is not known, but it is queried by the host software on startup.  It does not appear to correspond to the UnMouse's serial number.

## Calibration

The UnMouse's calibration process is not entirely understood.  The observed sequence of events is as follows:

* The host sends Listen 1 with data payload `0x00 0x01`.
* The host asks the user to touch the lower-left corner of the panel.
* The host begins repeatedly sending Talk 1, to which the UnMouse responds with data payload `0x00 0x02`.
* The user touches the panel's lower-left corner.
* The UnMouse begins responding to Talk 1 with data payload `0x00 0x01`.
* The host sends Listen 0 with one of three possible data payloads (`0x96 0x00`, `0xC2 0x00`, or `0x00 0x01`).
* The host asks the user to touch the upper-right corner of the panel.
* The host resumes repeatedly sending Talk 1, to which the UnMouse continues to respond with data payload `0x00 0x01`.
* The user touches the panel's upper-right corner.
* The UnMouse responds to Talk 1 usually with data payload `0x00 0x00` (but sometimes with `0x00 0xFF`).
* The host reports that calibration is complete.

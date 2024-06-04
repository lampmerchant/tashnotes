# Gravis Blackhawk

The Gravis Blackhawk first appears with address 0x3 and handler ID 0x01, but the Gravis Blackhawk INIT changes the handler ID to 0x4E when it loads, which enables the following output formats.  The INIT may also change the handler ID to 0x23 if it is configured to MouseStick mode, but this does not appear to change the format of its output.


## Registers

### Talk 0

Talk 0 gives the current state of the joystick's analog and digital controls in an 8-byte response:

| Bit(s) | Description                      | Encoding                     |
| ------ | -------------------------------- | ---------------------------- |
| 63-44  | Always 1                         |                              |
| 43     | Handle trigger button            | 0 = down, 1 = up             |
| 42     | Handle thumb button              | 0 = down, 1 = up             |
| 41     | Handle lower button              | 0 = down, 1 = up             |
| 40     | Base button                      | 0 = down, 1 = up             |
| 39-32  | Handle horizontal position       | 0x00 = left, 0xFF = right    |
| 31-24  | Handle vertical position         | 0x00 = up, 0xFF = down       |
| 23-16  | Throttle position                | 0x00 = up, 0xFF = down       |
| 15-0   | Always 0                         |                              |


### Talk 1

Talk 1 always gives the response 0x0A 0x06 0x01.  This is likely a protocol identifier of some kind.


## Relationship to Gravis Firebird

The Blackhawk's protocol is similar to the Firebird's, though stripped down considerably.  The driver's readme (dated 12 November 1996) notes that the Blackhawk driver should load before the Firebird driver if both are in use - this is likely because they use the same ADB handler ID and the Blackhawk is newer than the Firebird.


## Thanks

*Thanks to demik on #68kmla for compiling this information!*

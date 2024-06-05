# Gravis Firebird

The Gravis Firebird first appears with address 0x3 and handler ID 0x01, but the Gravis Firebird INIT changes the handler ID to 0x4E when it loads, which enables the following output formats.  The INIT may also change the handler ID to 0x23 if it is configured to MouseStick mode, but this does not appear to change the format of its output.


## Registers

### Talk 0

Talk 0 gives the current state of the joystick's analog and digital controls in an 8-byte response:

| Bit(s) | Description                      | Encoding                     |
| ------ | -------------------------------- | ---------------------------- |
| 63-57  | Always 1                         |                              |
| 56     | Base lower left button           | 0 = down, 1 = up             |
| 55     | Base (-) button                  | 0 = down, 1 = up             |
| 54     | Base lower right button          | 0 = down, 1 = up             |
| 53     | Base middle left button          | 0 = down, 1 = up             |
| 52     | Base (+) button                  | 0 = down, 1 = up             |
| 51     | Base middle right button         | 0 = down, 1 = up             |
| 50     | Base upper left button           | 0 = down, 1 = up             |
| 49     | Base upper middle button         | 0 = down, 1 = up             |
| 48     | Base upper right button          | 0 = down, 1 = up             |
| 47     | Directional hat left             | 0 = pressed, 1 = not pressed |
| 46     | Handle upper button              | 0 = down, 1 = up             |
| 45     | Handle middle button             | 0 = down, 1 = up             |
| 44     | Directional hat down             | 0 = pressed, 1 = not pressed |
| 43     | Directional hat up               | 0 = pressed, 1 = not pressed |
| 42     | Handle trigger button            | 0 = down, 1 = up             |
| 41     | Handle lower button              | 0 = down, 1 = up             |
| 40     | Directional hat right            | 0 = pressed, 1 = not pressed |
| 39-32  | Handle horizontal position       | 0x00 = left, 0xFF = right    |
| 31-24  | Handle vertical position         | 0x00 = up, 0xFF = down       |
| 23-16  | T-grip throttle control position | 0x00 = up, 0xFF = down       |
| 15-8   | Elevator trim adjuster position  | 0x00 = up, 0xFF = down       |
| 7-0    | Always 0?*                       |                              |

\* Bits 7-0 are speculated to correspond to values read from the rudder pedal jack.


### Talk 1

Talk 1 always gives the response 0x0A 0x01 0x30.  This is likely a protocol identifier of some kind.

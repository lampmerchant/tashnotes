# PowerKey Pro 200

## Y-Cable Pinout

### Connector Diagrams

```
5-Pin Mini-DIN      4-Pin Mini-DIN     4-Pin Mini-DIN
Male to PowerKey:   Male to Mac:       Female to Keyboard:
  .--_--.             .--_--.            .--_--.
 / 4   5 \           / 3   4 \          / 4   3 \
( 2  |  3 )         ( 1     2 )        ( 2     1 )
 \_ 1   _/           \_ --- _/          \_ --- _/
   '---'               '---'              '---'
```

### Signals

| PowerKey | Mac | Keyboard | Function                                                                 |
| -------- | --- | -------- | ------------------------------------------------------------------------ |
| 1        | 2   | -        | Soft power pin used by PowerKey to power up soft power Mac               |
| 2        | 1   | 1        | Apple Desktop Bus                                                        |
| 3        | -   | 2        | Soft power pin used by PowerKey to detect press of power key on keyboard |
| 5        | 4   | 4        | Ground                                                                   |
| -        | 3   | 3        | 5VDC                                                                     |

Pin 4 on the PowerKey connector is not connected.

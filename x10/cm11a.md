# CM11A

CM11A is a PC RS-232 interface for X10.

## Cable

The CM11A unit is a DCE (modem) for purposes of RS-232 communication with the PC, which is a DTE (terminal).  It includes a cable
with a 4P4C (sometimes incorrectly called an RJ9, RJ10, or RJ22) connector on one end to connect to the CM11A and a DE-9 female
connector on the other to connect to the PC, wired as detailed below:

```
 DE-9 Female              4P4C
                               1234
                      __      ______
.-----------.       _|  |_   | |||| |
\ 5 4 3 2 1 /      |      |  |.----.|
 \ 9 8 7 6 /       | oooo |  ||    ||
  '-------'        |_||||_|  |'----'|
                     1234    |_|  |_|
                               |  |
                   (front)    (top)
```

| DE-9 Female Pin | 4P4C Pin   | RS-232 Function     | CM11A Function   |
| --------------- | ---------- | ------------------- | ---------------- |
| 2               | 1 (Yellow) | RXD (Receive Data)  | CM11A to PC Data |
| 3               | 3 (Red)    | TXD (Transmit Data) | PC to CM11A Data |
| 5               | 4 (Black)  | Ground              | Ground           |
| 9               | 2 (Green)  | RI (Ring Indicator) | Poll Indicator   |

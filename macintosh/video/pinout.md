# Macintosh and Apple IIgs Video Pinout

| DA-15 Pin | Macintosh Video                | Apple IIgs Video      | DE-15 (VGA) Pin |
| --------- | ------------------------------ | --------------------- | --------------- |
| 1         | Red Ground                     | Red Ground            | 6               |
| 2         | Red Signal                     | Red Signal            | 1               |
| 3         | Composite Sync Signal          | Composite Sync Signal | -               |
| 4         | Monitor Sense 0                | (not connected)       | -               |
| 5         | Green Signal                   | Green Signal          | 2               |
| 6         | Green Ground                   | Green Ground          | 7               |
| 7         | Monitor Sense 1                | -5 VDC                | -               |
| 8         | (not connected)                | +12 VDC               | -               |
| 9         | Blue Signal                    | Blue Signal           | 3               |
| 10        | Monitor Sense 2                | (not connected)       | -               |
| 11        | Vertical/Composite Sync Ground | Sound                 | 10              |
| 12        | Vertical Sync Signal           | Monochrome Signal     | 14              |
| 13        | Blue Ground                    | Blue Ground           | 8               |
| 14        | Horizontal Sync Ground         | (not connected)       | 5               |
| 15        | Horizontal Sync Signal         | (not connected)       | 13              |
| Shield    | Ground                         | System Ground         | Shield          |

VGA also has monitor sense pins, but they do not follow the same scheme as the Macintosh monitor sense pins, so the preceding chart leaves them unconnected.

Use caution when connecting hardware meant for the Macintosh on Apple IIgs machines.  The presence of -5 VDC and +12 VDC rails on DA-15 pins may cause damage to the connected hardware or the IIgs itself.

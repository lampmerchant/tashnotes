Chassis Intrusion Function of [Nuvoton W83795G]
===============================================

The [Nuvoton W83795G] Hardware Monitor IC at address 0x2F of the southbridge's secondary SMBus controller has a chassis intrusion
function exposed on the D16's AUX_PANEL1 header:

[Nuvoton W83795G]: http://www.nuvoton.com/resource-files/Nuvoton_W83795G_W83795ADG_Datasheet_V1.43.pdf

```
o o   o o o o o o o
o  [o o]o o o o o o
    | |
    | GND
    CASEOPEN
```

The D16 ships with a jumper connecting CASEOPEN to GND. If CASEOPEN is ever allowed to float (i.e. the jumper is removed or a
microswitch connecting the two pins is opened), even when power is not connected to the board, the chassis intrusion alarm is set
and must be  unset manually.

The function is exposed as intrusion0 by the Linux w83795 driver.

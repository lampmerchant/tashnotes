Suggestions for KGPE-D16 System Builders
========================================

Fan Layout
----------

Raptor Computing recommends [here][1] that that the CPU fans be configured as 4-pin and the case fans as 3-pin to allow independent
control of the CPU fans and the case fans.

[1]: https://www.raptorengineering.com/coreboot/kgpe-d16-bmc-port-status.php#host-thermal-management-notes

To do this, configure the CPUFAN_SEL1 and CHAFAN_SEL1 jumpers (which are non-obviously labelled on the board, but are adjacent to
the FRNT_FAN5 connector) like so:

```
                                    |
CHAFAN_SEL1   o [o  o]              |
CPUFAN_SEL1  [o  o] o    FRNT_FAN5  |
                         o  o  o  o |
                                    |
```

Northbridge Cooling
-------------------

The SR5690's heatsink was ostensibly chosen to be low profile so it wouldn't interfere with nearby expansion slots, but as a result,
it runs VERY hot (in the 70°C range).  This can be substantially improved (down to the 40°C range) by mounting a 60mm fan over it.
This can be done by using zipties to bind two of the fan's corner holes to the tension wires that hold the heatsink down.

The SR5690's absolute maximum temperature is 95°C, per the [databook](https://www.amd.com/en/support/tech-docs/amd-sr5690-databook).

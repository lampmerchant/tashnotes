Cooling Functions of [Nuvoton W83795G]
======================================

The D16's temperature sensors, fan tachometers, and fan PWM controls are connected to a [Nuvoton W83795G] Hardware Monitor IC at
address 0x2F of the southbridge's secondary SMBus controller.

[Nuvoton W83795G]: http://www.nuvoton.com/resource-files/Nuvoton_W83795G_W83795ADG_Datasheet_V1.43.pdf

Temperature Sensors
-------------------

| Chip Input | Type             | w83795 Driver Input | Device                   | Comments                 |
| ---------- | ---------------- | ------------------- | ------------------------ | ------------------------ |
| TD1        | Thermal Diode    | temp1               | SR5690 (Northbridge)     |                          |
| TR2        | Thermistor       | temp2               | Thermistor on TR1 header |                          |
| TR3        | Thermistor       | temp3               | Thermistor on TR2 header |                          |
| DTS1       | Digital (SB-TSI) | temp7               | CPU 0                    | Same as k10temp-pci-00c3 |
| DTS2       | Digital (SB-TSI) | temp8               | CPU 1                    | Same as k10temp-pci-00d3 |

[1]: https://www.amd.com/en/support/tech-docs/amd-sr5690-databook

Thermistors
-----------

The board has two three-pin headers, TR1 and TR2, into which 10kΩ thermistors can be plugged, connecting to the W83795G's TR2 and
TR3 inputs.  Two 10kΩ thermistors with leads ending in two-pin connectors are included with the boxed version of the board.  The
two-pin connectors can be plugged into either side of the three-pin headers.  Thermistors have no polarity and can be plugged in 
with either orientation.

```
                        |
             FRNT_FAN3  |
             o  o  o  o |
                        |
TR2  o  o  o            |
TR1  o  o  o            |
             FRNT_FAN4  |
             o  o  o  o |
                        |
```

Fan Tachometers
---------------

| Connector Label | w83795 Driver Input |
| --------------- | ------------------- |
| CPU_FAN1        | fan1                |
| CPU_FAN2        | fan2                |
| FRNT_FAN1       | fan3                |
| FRNT_FAN2       | fan4                |
| FRNT_FAN3       | fan5                |
| FRNT_FAN4       | fan6                |
| FRNT_FAN5       | fan7                |
| REAR_FAN1       | fan8                |

[source](https://review.coreboot.org/cgit/coreboot.git/tree/src/mainboard/asus/kgpe-d16/spd_notes.txt?id=8d6d3fa109ca6895008639e12b0eb48d700e8665)

PWM Outputs
-----------

Despite the W83795G having 8 PWM outputs, only two are connected, and they do not have a 1:1 correspondence with fan headers.

The CHAFAN_SEL1 and CPUFAN_SEL1 jumpers control the voltage that is supplied to the fan headers, with CHAFAN_SEL1 controlling the
six chassis fans (FRNT_FAN1-5 and REAR_FAN1) and CPUFAN_SEL1 controlling the two CPU fans (CPU_FAN1 and CPU_FAN2). If one of these
jumpers shorts the two pins on the side facing towards FRNT_FAN5 (configuring them as 3-pin fans), the fans it controls will
receive a supply voltage modulated by the second PWM channel (pwm2); if the two pins on the opposite side are shorted (configuring 
them as 4-pin fans), they will receive an unmodulated 12V supply instead.

The first PWM channel (pwm1) controls the PWM signal on the fourth pin of all eight fan headers on the board, regardless of the
configuration of CHAFAN_SEL1 and CPUFAN_SEL1. This means that if a 4-pin fan is plugged into a header configured as 3-pin, its
speed will be controlled by both pwm1 and pwm2.

Manual Control (Linux)
----------------------

Ensure the w83795 driver is loaded:

```
# modprobe w83795
```

Identify which hwmon device it is by looking under /sys/class/hwmon/hwmonN/device/name, where N is replaced by a number, and execute
a command such as these:

```
# echo 254 > /sys/class/hwmon/hwmonN/device/pwm1    # Set 4-pin fans to full speed
# echo 254 > /sys/class/hwmon/hwmonN/device/pwm2    # Set 3-pin fans to full speed
```

Experimentation shows that a value of 0 for the 3-pin fans' PWM brings them down to about 60-75% of full speed.

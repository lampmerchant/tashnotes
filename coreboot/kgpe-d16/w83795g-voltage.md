Voltage Monitoring Functions of [Nuvoton W83795G]
=================================================

The D16's voltage sensors are connected to a [Nuvoton W83795G] Hardware Monitor IC at address 0x2F of the southbridge's secondary
SMBus controller.

[Nuvoton W83795G]: http://www.nuvoton.com/resource-files/Nuvoton_W83795G_W83795ADG_Datasheet_V1.43.pdf

| W83795G Pin Name | BIOS Name | w83795 Driver Input | Min Voltage | Max Voltage | Scale Factor | Comments                          |
| ---------------- | --------- | ------------------- | ----------- | ----------- | ------------ | --------------------------------- |
| VCORE1           | VCore1    | in0                 | 0.9         | 1.5         | 1            | CPU 0 Core Voltage                |
| VCORE2           | VCore2    | in1                 | 0.9         | 1.5         | 1            | CPU 1 Core Voltage                |
| VSEN1            | P1DDR3    | in2                 | 1.1         | 1.6         | 1            | CPU 0 DRAM Voltage                |
| VSEN2            | P1DDR3    | in3                 | 1.1         | 1.6         | 1            | CPU 1 DRAM Voltage                |
| VSEN5            | P1_+1.2V  | in4                 | 1.15        | 1.25        | 1            | CPU 0 HyperTransport Link Voltage |
| VSEN6            | P2_+1.2V  | in5                 | 1.15        | 1.25        | 1            | CPU 1 HyperTransport Link Voltage |
| VSEN7            | P1_VDDNB  | in6                 | 1.05        | 1.25        | 1            | SR5690 (Northbridge) Core Voltage |
| VSEN8            | +1.8V     | in7                 | 1.7         | 1.9         | 1            | 1.8V                              |
| VSEN9            | +1.2V     | in8                 | 1.15        | 1.25        | 1            | 1.2V                              |
| VSEN10           | +1.1V     | in9                 | 1.05        | 1.15        | 1            | 1.1V                              |
| VSEN11           | +5VSB     | in10                | 1.50 (4.8)  | 1.625 (5.2) | ~3.2         | 5VSB                              |
| 3VDD             | +3.3V     | in12/+3.3V          | 3.1         | 3.5         | 1            | 3.3V                              |
| 3VSB             | +3.3VSB   | in13/3VSB           | 3.1         | 3.5         | 1            | 3.3VSB                            |
| VBAT             | VBAT      | in14/Vbat           | 2.5         | 3.5         | 1            | CMOS Battery Voltage              |
| VSEN12           | +12V      | in15                | 0.917 (~11) | 1.083 (~13) | ~12          | 12V                               |
| VSEN13           | +5V       | in16                | 1.5 (4.8)   | 1.625 (5.2) | ~3.2         | 5V                                |

[source](https://review.coreboot.org/cgit/coreboot.git/tree/src/mainboard/asus/kgpe-d16/devicetree.cb?id=8d6d3fa109ca6895008639e12b0eb48d700e8665)

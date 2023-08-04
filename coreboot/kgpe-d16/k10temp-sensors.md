CPU Temperature Sensors Readable by Linux [k10temp] Driver
==========================================================

[k10temp]: https://github.com/torvalds/linux/blob/master/drivers/hwmon/k10temp.c

| Input            | CPU | Comments                    |
| ---------------- | --- | --------------------------- |
| k10temp-pci-00c3 | 0   | Passed to W83795G by SB-TSI |
| k10temp-pci-00cb | 0   |                             |
| k10temp-pci-00d3 | 1   | Passed to W83795G by SB-TSI |
| k10temp-pci-00db | 1   |                             |

TODO what are the differences between the two different inputs on the same CPU?

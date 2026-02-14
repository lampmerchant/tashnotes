# Macintosh Floppy Drive Connector

This document details the connectors used by Macintosh computers to connect internal and external floppy drives.

The information here is provided on a best-effort basis and is not guaranteed to be accurate.  Use at your own risk.


## Connector Diagrams

### 2x10 Pin Header

```
Male:
.-------------------------------.  
|  2  4  6  8 10 12 14 16 18 20 |  
|  1  3  5  7  9 11 13 15 17 19 |  
'-------------'   '-------------'  
```


### 19-Pin D-Subminiature Connector

```
Male:
.----------------------------------------.
\  1   2   3   4   5   6   7   8   9  10 /
 \  11  12  13  14  15  16  17  18  19  / 
  '------------------------------------'
```

The non-standard 19-pin D-subminiature connector used for external drive connectors is frequently referred to as a "DB-19", but this is incorrect as "DB" refers to a larger D-subminiature shell size which holds 25, 44, or 52 (typically 25) pins.


## Pin Assignments (Common)

| Pin Name/Function     | Source       | Direction | D-Subminiature Pin | 2x10 Header Pin |
| --------------------- | ------------ | --------- | ------------------ | --------------- |
| Ground                | Power Supply | —         | 1, 2, 3, 4*        | 1, 3, 5, 7      |
| △ (see below)         |              |           | 5                  | 9               |
| +5V                   | Power Supply | →         | 6                  | 11              |
| +12V                  | Power Supply | →         | 7, 8               | 13, 15, 17, 19  |
| ◇ (see below)         |              |           | 10                 | 20              |
| CA0 / Phase0          | IWM/SWIM     | →         | 11                 | 2               |
| CA1 / Phase1          | IWM/SWIM     | →         | 12                 | 4               |
| CA2 / Phase2          | IWM/SWIM     | →         | 13                 | 6               |
| CA3 / Phase3 / LSTRB  | IWM/SWIM     | →         | 14                 | 8               |
| !WRREQ / Write Enable | IWM/SWIM     | →         | 15                 | 10              |
| SEL / Head Select     | VIA          | →         | 16                 | 12              |
| !ENBL / Drive Enable  | IWM/SWIM     | →         | 17                 | 14              |
| RD / Read             | IWM/SWIM     | ←         | 18                 | 16              |
| WR / Write            | IWM/SWIM     | →         | 19                 | 18              |

Direction is Macintosh relative to drive.

\* The last ground (D-subminiature pin 4) is left floating by drives compatible with Apple II computers.


## Pin Assignments (Per Drive)

### △ Pin

| Drive                              | Pin Name/Function | Direction |
| ---------------------------------- | ----------------- | --------- |
| 400K Disk Drive                    | -12V              | →         |
| 800K Disk Drive with Black Label   | Not Connected     | —         |
| 800K Disk Drive with Red Label     | !EJECT            | →         |
| Automatic-Inject SuperDrive (FDHD) | !EJECT            | →         |

Direction is Macintosh relative to drive.


### ◇ Pin

| Drive                              | Pin Name/Function | Direction |
| ---------------------------------- | ----------------- | --------- |
| 400K Disk Drive                    | PWM               | →         |
| 800K Disk Drive with Black Label   | Not Connected     | —         |
| 800K Disk Drive with Red Label     | !CSTOUT           | ←         |
| Automatic-Inject SuperDrive (FDHD) | !CSTOUT           | ←         |

Direction is Macintosh relative to drive.


## Pin Assignments (Per Macintosh Model)

### △ Pin

| Macintosh Model         | Pin Name/Function | Source       | Direction |
| ----------------------- | ----------------- | ------------ | --------- |
| 128K, 512K, 512Ke, Plus | -12V              | Power Supply | →         |
| SE*, IIcx*, IIci*       | -12V              | Power Supply | →         |
| Other†                  | Not Connected     | —            | —         |

Direction is Macintosh relative to drive.

\* D-subminiature connector only; on 2x10 pin header, this pin is not connected.

† Macintosh models with manual-inject floppy drives may connect this pin to ground, which may cause automatic-inject floppy drives to eject constantly.


### ◇ Pin

| Macintosh Model         | Pin Name/Function           | Source       | Direction |
| ----------------------- | --------------------------- | ------------ | --------- |
| 128K, 512K, 512Ke, Plus | PWM                         | ASG PAL      | →         |
| SE*                     | PWM                         | BBU          | →         |
| IIcx*, IIci*            | PWMPU (+5V pullup resistor) | Power Supply | →         |
| SE FDHD*, SE/30*        | +5V                         | Power Supply | →         |
| Other                   | Not Connected               | —            | —         |

Direction is Macintosh relative to drive.

\* D-subminiature connector only; on 2x10 pin header, this pin is not connected.

# Modified Frequency Modulation (MFM)

## Encoding

### Data

Like GCR, MFM encoding represents data as flux transitions (or their absence) in the centers of two-microsecond bit cells, where a transition indicates a '1' data bit and no transition indicates a '0' data bit.  However, at the boundary of two bit cells containing '0' data bits, a flux transition is placed, referred to as a *clock*.

For example, the value `0xC5` is represented like this:

```
             Bit Cells: ..   1   |   1   |   0   |   0   |   0   |   1   |   0   |   1   ..

                        ..___   _____   _________   _____   _________   _____________   _..
      Flux Transitions:      |_|     |_|         |_|     |_|         |_|             |_|

Spacing (microseconds):      |<--2-->|<----3---->|<--2-->|<----3---->|<------4------>|
```


### Mark Bytes

GCR uses special reserved bytes to act as marks.  MFM, however, encodes all 256 data byte values directly and thus requires a different mechanism to perform the same function: missing clocks.  At the point between two '0' bit cells where a clock would ordinarily be placed, the clock is skipped.

Two mark bytes are defined: `0xC2` for index marks and `0xA1` for address and data marks.  Both of these bytes contain a `0b100001` bit sequence where the middle clock is removed:

```
             Bit Cells: ..   1   |   0   |   0   |   0   |   0   |   1   ..

(normal data)
                        ..___   _________   _____   _____   _________   _..
      Flux Transitions:      |_|         |_|     |_|     |_|         |_|

Spacing (microseconds):      |<----3---->|<--2-->|<--2-->|<----3---->|


(clock removed)
                        ..___   _________   _____________   _________   _..
      Flux Transitions:      |_|         |_|             |_|         |_|

Spacing (microseconds):      |<----3---->|<------4------>|<----3---->|
```


### State Machine

The distance between flux transitions can be translated into bits using the following state machine:

```
Start     2 -> 0b1                    2 -> 0b0
  |       .-------.                   .-------.
  |    ___|___    |                   |    ___|___
  |   |       |<--'     3 -> 0b0      '-->|       |
  '-->| Even  |-------------------------->|  Odd  |
      | State |<--------------------------| State |
      |_______|<--.     3 -> 0b01     .-->|_______|
          |       |                   |       |
          '-------'                   '-------'
          4 -> 0b01                   4 -> 0b00 (with missing clock)
```


## Track Layout

### Track

An MFM track is laid out as follows:

| Field Description   | Data (1440 kB Disk)         | Data (720 kB Disk)          |
| ------------------- | --------------------------- | --------------------------- |
| Gap to Index Mark   | 80 x `0x4E`                 | 80 x `0x4E`                 |
| Sync Field          | 12 x `0x00`                 | 12 x `0x00`                 |
| Index Mark          | `0xC2` `0xC2` `0xC2` `0xFC` | `0xC2` `0xC2` `0xC2` `0xFC` |
| Gap to First Sector | 50 x `0x4E`                 | 50 x `0x4E`                 |
| Sector 1            | see below                   | see below                   |
| Sector 2            | see below                   | see below                   |
| ...                 | ...                         | ...                         |
| Sector *n*          | see below                   | see below                   |
| Gap to End of Track | 204 x `0x4E`                | 182 x `0x4E`                |

If the drive has an index pulse signal (Macintosh Superdrives do, but it is only used when formatting disks), it is pulsed at the start of the gap to the index mark.

Note that the `0xC2` bytes described above have a missing clock in order to differentiate them from ordinary data bytes.


### Sector

An MFM sector is laid out as follows:

| Field Description   | Data (1440 kB Disk)         | Data (720 kB Disk)          |
| ------------------- | --------------------------- | --------------------------- |
| Sync Field          | 12 x `0x00`                 | 12 x `0x00`                 |
| Address Mark        | `0xA1` `0xA1` `0xA1` `0xFE` | `0xA1` `0xA1` `0xA1` `0xFE` |
| Address Header      | see below                   | see below                   |
| Address CRC         | see below                   | see below                   |
| Intra-Sector Gap    | 22 x `0x4E`                 | 22 x `0x4E`                 |
| Sync Field          | 12 x `0x00`                 | 12 x `0x00`                 |
| Data Mark           | `0xA1` `0xA1` `0xA1` `0xFB` | `0xA1` `0xA1` `0xA1` `0xFB` |
| Data Sector         | 512 data bytes              | 512 data bytes              |
| Data CRC            | see below                   | see below                   |
| Inter-Sector Gap    | 84 x `0x4E`                 | 101 x `0x4E`                |

Note that the `0xA1` bytes described above have a missing clock in order to differentiate them from ordinary data bytes.


### Address Header

An MFM address header consists of 4 bytes:

| Index | Description                     |
| ----- | ------------------------------- |
| 0     | Cylinder Number                 |
| 1     | Head Number                     |
| 2     | Sector Number                   |
| 3     | Size of Sector (128 * 2ⁿ bytes) |

Note that sectors are numbered starting at 1 instead of 0.

Sector size should be 2 to indicate 512-byte sectors.


### CRC

The CRC algorithm used is [CRC-16/IBM-3740](https://reveng.sourceforge.io/crc-catalogue/16.htm#crc.cat.crc-16-ibm-3740) (sometimes incorrectly identified as CRC-CCITT).  The data fed into the algorithm includes the address/data mark bytes.


## Synchronization

With MFM encoding, it is necessary to synchronize the reader so that it can tell the difference between data bits and clocks, and for the writer to provide a mechanism for synchronization to the reader.  Without synchronization, it is possible for the reader to be out of phase (interpreting data bits as clocks and vice-versa) and the data read to be incoherent.

Synchronization is accomplished by means of the sync field and the mark bytes that follow it.  Before writing an index, address, or data mark, the writer writes a sync field of twelve `0x00` bytes.  This appears to the unsynchronized reader as a sequence of 96 flux transitions, each separated by two microseconds.  The reader can look for this sequence immediately followed by three mark bytes and, on finding it, know that it is synchronized.


### Synchronization to Address/Data Mark

An example of correct synchronization using an address/data mark byte (`0xA1` with missing clock) is shown below:

```
                      [ 0x00                        ] [ 0xA1 with missing clock     ] [ 0xA1 with missing clock     ]
       Bit Cells: .. | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 1 ..
                  .._  __  __  __  __  __  __  __  ____  ______  ____  ______  ____  __  ______  ____  ______  ____  ..
Flux Transitions:    ||  ||  ||  ||  ||  ||  ||  ||    ||      ||    ||      ||    ||  ||      ||    ||      ||    ||
```

It is safe to use this sequence for synchronization without context as it can never occur in normal data.  For it to do so would require the appearance of an `0x0A` byte with a missing clock, which would be invalid:

```
                    [ 0xFF                        ] [ 0x0A with missing clock     ] [ 0x0A with missing clock     ]
       Bit Cells: .. 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | ..
                  .._  __  __  __  __  __  __  __  ____  ______  ____  ______  ____  __  ______  ____  ______  ____  ..
Flux Transitions:    ||  ||  ||  ||  ||  ||  ||  ||    ||      ||    ||      ||    ||  ||      ||    ||      ||    ||
```


### Synchronization to Index Mark

An example of correct synchronization using an index mark byte (`0xC2` with missing clock) is shown below:

```
                      [ 0x00                        ] [ 0xC2 with missing clock     ] [ 0xC2 with missing clock     ]
       Bit Cells: .. | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 1 | 0 ..
                  .._  __  __  __  __  __  __  __  ____  __  ____  ______  ____  ______  __  ____  ______  ____  ____..
Flux Transitions:    ||  ||  ||  ||  ||  ||  ||  ||    ||  ||    ||      ||    ||      ||  ||    ||      ||    ||
```

Unlike synchronization to an address/data mark, it is *not* safe to use this sequence for synchronization without context as it *can* occur in normal data:

```
                    [ 0xFF                        ] [ 0x14                        ] [ 0x14                        ]
       Bit Cells: .. 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | 0 | ..
                  .._  __  __  __  __  __  __  __  ____  __  ____  ______  ____  ______  __  ____  ______  ____  ____..
Flux Transitions:    ||  ||  ||  ||  ||  ||  ||  ||    ||  ||    ||      ||    ||      ||  ||    ||      ||    ||
```

To synchronize to an index mark, the reader must first look for an index pulse from the drive in order to be sure that it is not in the middle of a data sector that contains the sequence of ordinary bytes shown above.

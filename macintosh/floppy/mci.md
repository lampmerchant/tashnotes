# Microfloppy Control Interface

The **Microfloppy Control Interface** (MCI) is the means by which floppy disk drives are controlled by a Macintosh.

## Signal Multiplexing

The MCI uses the CA2-0 lines and the SEL line to select one of a set of one-bit signals from the drive, the value of which is reflected on the RD line.

### 400 KB GCR Drive

| CA2  | CA1  | CA0  | SEL  | RD Output  |
| ---- | ---- | ---- | ---- | ---------- |
| Low  | Low  | Low  | Low  | !DIRTN     |
| Low  | Low  | Low  | High | !CSTIN     |
| Low  | Low  | High | Low  | !STEP      |
| Low  | Low  | High | High | !WRPROT    |
| Low  | High | Low  | Low  | !MOTORON   |
| Low  | High | Low  | High | !TK0       |
| Low  | High | High | Low  | Always Low |
| Low  | High | High | High | !TACH      |
| High | Low  | Low  | Low  | RDDATA0    |
| High | Low  | Low  | High | †          |
| High | Low  | High | Low  | SUPERDRIVE |
| High | Low  | High | High | Always Low |
| High | High | Low  | Low  | SIDES      |
| High | High | Low  | High | Always Low |
| High | High | High | Low  | !DRVIN     |
| High | High | High | High | REVISED    |

† TODO

### 800 KB GCR Drive

| CA2  | CA1  | CA0  | SEL  | RD Output  |
| ---- | ---- | ---- | ---- | ---------- |
| Low  | Low  | Low  | Low  | !DIRTN     |
| Low  | Low  | Low  | High | !CSTIN     |
| Low  | Low  | High | Low  | !STEP      |
| Low  | Low  | High | High | !WRPROT    |
| Low  | High | Low  | Low  | !MOTORON   |
| Low  | High | Low  | High | !TK0       |
| Low  | High | High | Low  | SWITCHED   |
| Low  | High | High | High | !TACH      |
| High | Low  | Low  | Low  | RDDATA0    |
| High | Low  | Low  | High | RDDATA1    |
| High | Low  | High | Low  | SUPERDRIVE |
| High | Low  | High | High | †          |
| High | High | Low  | Low  | SIDES      |
| High | High | Low  | High | !READY     |
| High | High | High | Low  | !DRVIN     |
| High | High | High | High | REVISED    |

† TODO

### Superdrive

| CA2  | CA1  | CA0  | SEL  | MFMMODE | !WRREQ | RD Output   |
| ---- | ---- | ---- | ---- | ------- | ------ | ----------- |
| Low  | Low  | Low  | Low  | *       | *      | !DIRTN      |
| Low  | Low  | Low  | High | *       | *      | !CSTIN      |
| Low  | Low  | High | Low  | *       | *      | !STEP       |
| Low  | Low  | High | High | *       | *      | !WRPROT     |
| Low  | High | Low  | Low  | *       | *      | !MOTORON    |
| Low  | High | Low  | High | *       | *      | !TK0        |
| Low  | High | High | Low  | *       | *      | SWITCHED    |
| Low  | High | High | High | Low     | *      | !TACH       |
| Low  | High | High | High | High    | *      | INDEX       |
| High | Low  | Low  | Low  | Low     | *      | RDDATA0     |
| High | Low  | Low  | Low  | High    | Low    | INDEX       |
| High | Low  | Low  | Low  | High    | High   | RDDATA0     |
| High | Low  | Low  | High | Low     | *      | RDDATA1     |
| High | Low  | Low  | High | High    | Low    | INDEX       |
| High | Low  | Low  | High | High    | High   | RDDATA1     |
| High | Low  | High | Low  | *       | *      | SUPERDRIVE  |
| High | Low  | High | High | *       | *      | MFMMODE     |
| High | High | Low  | Low  | *       | *      | SIDES       |
| High | High | Low  | High | *       | *      | !READY      |
| High | High | High | Low  | *       | *      | !DRVIN      |
| High | High | High | High | *       | *      | PRESENT/!HD |

\* Don't Care

### Hard Disk 20/DCD

| CA2  | CA1  | CA0  | RD Output   |
| ---- | ---- | ---- | ----------- |
| Low  | Low  | Low  | DCDDATA     |
| Low  | Low  | High | DCDDATA     |
| Low  | High | Low  | !HSHK       |
| Low  | High | High | !HSHK       |
| High | Low  | Low  | †           |
| High | Low  | High | Always Low  |
| High | High | Low  | Always High |
| High | High | High | Always High |

† TODO

### Composite

| CA2  | CA1  | CA0  | SEL  | 400 KB GCR | 800 KB GCR | Superdrive    | HD20/DCD    |
| ---- | ---- | ---- | ---- | ---------- | ---------- | ------------- | ----------- |
| Low  | Low  | Low  | Low  | !DIRTN     | !DIRTN     | !DIRTN        | DCDDATA     |
| Low  | Low  | Low  | High | !CSTIN     | !CSTIN     | !CSTIN        | DCDDATA     |
| Low  | Low  | High | Low  | !STEP      | !STEP      | !STEP         | DCDDATA     |
| Low  | Low  | High | High | !WRPROT    | !WRPROT    | !WRPROT       | DCDDATA     |
| Low  | High | Low  | Low  | !MOTORON   | !MOTORON   | !MOTORON      | !HSHK       |
| Low  | High | Low  | High | !TK0       | !TK0       | !TK0          | !HSHK       |
| Low  | High | High | Low  | Always Low | SWITCHED   | SWITCHED      | !HSHK       |
| Low  | High | High | High | !TACH      | !TACH      | !TACH/INDEX   | !HSHK       |
| High | Low  | Low  | Low  | RDDATA0    | RDDATA0    | RDDATA0/INDEX | †           |
| High | Low  | Low  | High | †          | RDDATA1    | RDDATA1/INDEX | †           |
| High | Low  | High | Low  | SUPERDRIVE | SUPERDRIVE | SUPERDRIVE    | Always Low  |
| High | Low  | High | High | Always Low | †          | MFMMODE       | Always Low  |
| High | High | Low  | Low  | SIDES      | SIDES      | SIDES         | Always High |
| High | High | Low  | High | Always Low | !READY     | !READY        | Always High |
| High | High | High | Low  | !DRVIN     | !DRVIN     | !DRVIN        | Always High |
| High | High | High | High | REVISED    | REVISED    | PRESENT/!HD   | Always High |

† TODO

## Signal Descriptions

| Signal Name | Description                                                                             |
| ----------- | --------------------------------------------------------------------------------------- |
| !DIRTN      | Step direction; low=toward center (+), high=toward rim (-)                              |
| !CSTIN      | Low when disk is present                                                                |
| !STEP       | Low when track step has been requested                                                  |
| !WRPROT     | Low when disk is write protected or not inserted                                        |
| !MOTORON    | Low when drive motor is on                                                              |
| !TK0        | Low when head is over track 0 (outermost track)                                         |
| SWITCHED    | High when disk has been changed since signal was last cleared                           |
| !TACH       | Tachometer; frequency reflects drive speed in RPM                                       |
| INDEX       | Pulses high for ~2 ms once per rotation                                                 |
| RDDATA0     | Signal from bottom head; falling edge indicates flux transition                         |
| RDDATA1     | Signal from top head; falling edge indicates flux transition                            |
| SUPERDRIVE  | High when a Superdrive (FDHD) is present                                                |
| MFMMODE     | High when drive is in MFM mode                                                          |
| SIDES       | High when drive has a top head in addition to a bottom head                             |
| !READY      | Low when motor is at proper speed and head is ready to step                             |
| !DRVIN      | Low when drive is installed                                                             |
| REVISED     | High for double-sided double-density drives, low for single-sided double-density drives |
| PRESENT/!HD | High when a double-density (not high-density) disk is present on a high-density drive   |
| DCDDATA     | Communication channel from DCD device to Macintosh                                      |
| !HSHK       | Low when DCD device is ready to receive or wishes to send                               |

## Commands

The MCI uses the CA2-0 lines and the SEL line to select one of a set of commands which is then initiated by a positive pulse on the CA3 line.

| CA2  | CA1  | CA0  | SEL  | Description                                                    |
| ---- | ---- | ---- | ---- | -------------------------------------------------------------- |
| Low  | Low  | Low  | Low  | Set !DIRTN low (toward center; track number increases on step) |
| Low  | Low  | Low  | High |                                                                |
| Low  | Low  | High | Low  | Step drive heads                                               |
| Low  | Low  | High | High | Select MFM mode                                                |
| Low  | High | Low  | Low  | Turn motor on                                                  |
| Low  | High | Low  | High |                                                                |
| Low  | High | High | Low  |                                                                |
| Low  | High | High | High |                                                                |
| High | Low  | Low  | Low  | Set !DIRTN high (toward rim; track number decreases on step)   |
| High | Low  | Low  | High | Reset SWITCHED to low                                          |
| High | Low  | High | Low  |                                                                |
| High | Low  | High | High | Select GCR mode                                                |
| High | High | Low  | Low  | Turn motor off                                                 |
| High | High | Low  | High |                                                                |
| High | High | High | Low  | Eject disk (CA3 must be held high for ~0.5 seconds)            |
| High | High | High | High |                                                                |

## Drive Identification

| Drive Type      | SUPERDRIVE | SIDES | !DRVIN | REVISED / PRESENT/!HD                         |
| --------------- | ---------- | ----- | ------ | --------------------------------------------- |
| None            | High       | High  | High   | High                                          |
| 400 KB GCR      | Low        | Low   | Low    | Low                                           |
| 800 KB GCR      | Low        | High  | Low    | High                                          |
| Superdrive      | High       | High  | Low    | High if double-density disk present, else low |
| Hard Disk 20    | Low        | High  | High   | High                                          |

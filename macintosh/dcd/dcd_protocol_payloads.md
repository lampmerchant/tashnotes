# Directly Connected Disks Protocol Payloads

'Payload' here refers to all bytes in all of the 7-to-8-encoded groups transferred as part of a DCD data transfer.  It does not include the length bytes transferred from the Macintosh to the DCD device or the 0xAA sync bytes transferred in either direction.

## Common Elements

### Identifier

The first byte in all DCD payloads is an identifier.

The MSB (bit 7) is clear if the payload is a command from the Macintosh and set if it's a response from the DCD device.  Bit 6 is set if the payload is a continuation of a previous command from the Macintosh.

The special value 0x7F is a negative acknowledgment and is given in response to receiving a payload with a bad checksum.

### Checksum

The last byte in all DCD payloads is a checksum.  This byte is chosen such that all bytes in the payload sum to 0 (modulo 256).

### Status

The third through sixth bytes in a DCD payload from the DCD device to the Macintosh make up a status code or bit field.  Its exact meaning is not known except that 0 values in all positions indicates no error.

## Basic Commands

Commands listed here are documented in the May 1985 DCD specification, though the definitions below are corrected with observed differences from the document.

### Read Sectors

Macintosh:

| Offset | Value                      |
| ------ | -------------------------- |
| 0      | 0x00                       |
| 1      | Number of sectors to read  |
| 2-4    | Sector offset (big-endian) |
| 5      | 0x00                       |
| 6      | Checksum                   |

DCD Device:

| Offset | Value                                                                                |
| ------ | ------------------------------------------------------------------------------------ |
| 0      | 0x80                                                                                 |
| 1      | Number of sectors remaining to be read (including sector contained in this response) |
| 2-5    | Status                                                                               |
| 6-25   | Tags of sector being read (20 bytes)                                                 |
| 26-537 | Data of sector being read (512 bytes)                                                |
| 538    | Checksum                                                                             |

The DCD device then repeats this response for each sector the Macintosh has requested to be read with the value at offset 1 in the response counting down, beginning at the value at offset 1 in the command and ending at 0x01.

### Write Sectors

Macintosh:

| Offset | Value                                                                                   |
| ------ | --------------------------------------------------------------------------------------- |
| 0      | 0x01                                                                                    |
| 1      | Number of sectors remaining to be written (including sector contained in this response) |
| 2-4    | Sector offset (big-endian)                                                              |
| 5      | 0x00                                                                                    |
| 6-25   | Tags of sector to be written (20 bytes)                                                 |
| 26-537 | Data of sector to be written (512 bytes)                                                |
| 538    | Checksum                                                                                |

DCD Device:

| Offset | Value                                                                                  |
| ------ | -------------------------------------------------------------------------------------- |
| 0      | 0x81                                                                                   |
| 1      | Number of sectors remaining to be written (including sector contained in last command) |
| 2-5    | Status                                                                                 |
| 6      | Checksum                                                                               |

Macintosh (if more than one sector is to be written):

| Offset | Value                                                                                   |
| ------ | --------------------------------------------------------------------------------------- |
| 0      | 0x41                                                                                    |
| 1      | Number of sectors remaining to be written (including sector contained in this response) |
| 2-5    | 0x00 0x00 0x00 0x00                                                                     |
| 6-25   | Tags of sector to be written (20 bytes)                                                 |
| 26-537 | Data of sector to be written (512 bytes)                                                |
| 538    | Checksum                                                                                |

The Macintosh then repeats this command (and the DCD device repeats its response above) for each sector the Macintosh has requested to be written with the value at offset 1 in the command counting down, ending at 0x01.

### Write and Verify Sectors

This command is identical to Write Sectors, except that the first byte of the initial command from the Macintosh is 0x02, the first byte of the DCD's response is 0x82, and the first byte of subsequent continuations of the command from the Macintosh is 0x42.

### Controller Status

Macintosh:

| Offset | Value                                       |
| ------ | ------------------------------------------- |
| 0      | 0x03                                        |
| 1      | 0x00 or 0x01, usually 0x00; meaning unknown |
| 2-5    | 0x00 0x00 0x00 0x00                         |
| 6      | Checksum                                    |

DCD Device:

| Offset  | Value                                        | Sample Value from HD20 |
| ------- | -------------------------------------------- | ---------------------- |
| 0       | 0x83                                         |                        |
| 1       | 0x00                                         |                        |
| 2-5     | Status                                       |                        |
| 6-7     | Device type                                  | 0x0001                 |
| 8-9     | Device manufacturer                          | 0x0001                 |
| 10      | Device characteristics bit field (see below) | 0xE6                   |
| 11-13   | Number of blocks                             | 0x009835               |
| 14-15   | Number of spare blocks                       | 0x0045                 |
| 16-17   | Number of bad blocks                         | 0x0001                 |
| 18-69   | Manufacturer reserved                        |                        |
| 70-197  | Icon (see below)                             |                        |
| 198-325 | Icon mask (see below)                        |                        |
| 326     | Device location string length                |                        |
| 327-341 | Device location string                       |                        |
| 342     | Checksum                                     |                        |

All multibyte integers are big-endian.

The icon is defined as a bit-packed 32x32 monochrome image, with 1 bits representing black pixels and 0 bits representing white pixels.  The icon mask is defined as a bit-packed 32x32 monochrome image, with 1 bits representing opaque pixels and 0 bits representing transparent pixels.

The device characteristics bit field is defined as follows:

| Value | Meaning                   |
| ----- | ------------------------- |
| 0x80  | Mountable                 |
| 0x40  | Readable                  |
| 0x20  | Writable                  |
| 0x10  | Ejectable (see below)     |
| 0x08  | Write protected           |
| 0x04  | Icon included             |
| 0x02  | Disk in place (see below) |

The "ejectable" and "disk in place" bits are ostensibly intended to support removable media, however no mechanism for ejecting disks is known to exist in the protocol.

## Diagnostic Commands

Commands listed here are documented in the December 1984 Nisha firmware specification.

### Read ID

Macintosh:

| Offset | Value                    |
| ------ | ------------------------ |
| 0      | 0x04                     |
| 1-5    | 0x00 0x00 0x00 0x00 0x00 |
| 6      | Checksum                 |

DCD Device:

| Offset | Value                           | Sample Value from HD20 |
| ------ | ------------------------------- | ---------------------- |
| 0      | 0x84                            |                        |
| 1      | 0x00                            |                        |
| 2-5    | Status                          |                        |
| 6-18   | Name string                     | "Rene-1 RM MH "        |
| 19-21  | Device type                     | 0x000210               |
| 22-23  | Firmware revision               | 0x3372                 |
| 24-26  | Capacity (blocks)               | 0x009835               |
| 27-28  | Bytes per block                 | 0x0214                 |
| 29-30  | Number of cylinders             | 0x0131                 |
| 31     | Number of heads                 | 0x04                   |
| 32     | Number of sectors               | 0x20                   |
| 33-35  | Number of possible spare blocks | 0x00004C               |
| 36-38  | Number of spare blocks (?)      | 0x110100 (?)           |
| 39-41  | Number of bad blocks (?)        | 0x000000 (?)           |
| 42-47  | 0x00 0x00 0x00 0x00 0x00 0x00   |                        |
| 48     | Checksum                        |                        |

## Other Commands

Commands listed here are neither documented in the May 1985 DCD specification nor the December 1984 Nisha firmware specification.

### Format Disk

The meaning of this command is guessed from its use in the Erase Disk command in operation.

Macintosh:

| Offset | Value               |
| ------ | ------------------- |
| 0      | 0x19                |
| 1      | 0x01                |
| 2-5    | 0x00 0x00 0x00 0x00 |
| 6      | Checksum            |

The meaning of the bytes from offsets 1-5 are not known, this is observed from the Erase Disk command in operation.

DCD Device:

| Offset | Value    |
| ------ | -------- |
| 0      | 0x99     |
| 1      | 0x00     |
| 2-5    | Status   |
| 6      | Checksum |

The meaning of the byte in offset 1 is not known, this is observed from the Erase Disk command in operation.

### Verify Format

The meaning of this command is guessed from its use in the Erase Disk command in operation.

Macintosh:

| Offset | Value                    |
| ------ | ------------------------ |
| 0      | 0x1A                     |
| 1-5    | 0x00 0x00 0x00 0x00 0x00 |
| 6      | Checksum                 |

The meaning of the bytes from offsets 1-5 are not known, this is observed from the Erase Disk command in operation.

DCD Device:

| Offset | Value    |
| ------ | -------- |
| 0      | 0x9A     |
| 1      | 0x00     |
| 2-5    | Status   |
| 6      | Checksum |

The meaning of the byte in offset 1 is not known, this is observed from the Erase Disk command in operation.

The observed status from this command on an actual HD20 was 0x0000008A, however, returning a status of 0x00000000 does not appear to interrupt the Erase Disk operation.

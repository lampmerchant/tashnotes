# IBM PC 5150 Cassette BASIC Filesystem

## Directory Entries

Every file on the cassette is preceded by a directory entry, a section containing a single block in the following format:

| Offset | Type                               | Content                                       |
| ------ | ---------------------------------- | --------------------------------------------- |
| 0x00   | 8-bit unsigned int                 | 0xA5                                          |
| 0x01   | 8-byte string                      | File name                                     |
| 0x09   | 8-bit unsigned int                 | File type                                     |
| 0x0A   | 16-bit unsigned int, little-endian | Length                                        |
| 0x0C   | 16-bit unsigned int, little-endian | Segment                                       |
| 0x0E   | 16-bit unsigned int, little-endian | Offset                                        |
| 0x10   | 240-byte string                    | Padding, bytes equal to second byte of offset |

File names are padded with spaces and case-sensitive.

## File Types

| Value | Extension | Meaning                           |
| ----- | --------- | --------------------------------- |
| 0x00  | .D        | Data file                         |
| 0x01  | .M        | Memory image created by BSAVE     |
| 0x40  | .A        | ASCII BASIC listing               |
| 0x80  | .B        | Tokenized BASIC program           |
| 0xA0  | .P        | Protected tokenized BASIC program |

### Data Files

Data files are written and read using BASIC's OPEN statement.  Only one file can be open at a time; trying to open a second will result in a "file already open" error.

Data files are stored as a series of sections following the directory entry, each containing a single block in the following format:

| Offset          | Type               | Content                                   |
| --------------- | ------------------ | ----------------------------------------- |
| 0x00            | 8-bit unsigned int | Length, including this byte               |
| 0x01            | String             | Data                                      |
|                 | String             | Padding, bytes equal to last byte written |

Length will be equal to 0x00 for blocks containing the full 255 bytes of data.

Lines of text are delimited by CRs only.  When closing an open file, a final sector is always written; if there is no buffered data to write, length is equal to 0x01 and data is empty.

A file of any type except a BSAVE memory image may be opened by the OPEN command, but it will be interpreted as described above.  Extensions are ignored by the OPEN command.

### BSAVE Memory Images

Memory images are written and read using BASIC's BSAVE and BLOAD statements.

Memory images are stored as a single section following the directory entry, containing the data in the region of memory specified by the BSAVE command.  The segment, offset, and length fields in the memory image's directory entry contain the parameters specified by the DEF SEG and BSAVE commands.

BLOAD uses the length value from the directory entry but ignores the segment and offset values.  BLOAD will only open memory images and ignores other file types.

### BASIC Programs

BASIC programs are written with the segment last defined by the DEF SEG statement (0x0060 by default).

BASIC programs are loaded by the LOAD statement, which ignores extensions if provided and will load any type of file.

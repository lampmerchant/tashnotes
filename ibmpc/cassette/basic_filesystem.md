# IBM PC 5150 Cassette BASIC Filesystem

## Directory Entries

Every file on the cassette begins with a directory entry, a section containing a single block in the following format:

| Offset | Type                               | Size (bytes) | Content                                       |
| ------ | ---------------------------------- | ------------ | --------------------------------------------- |
| 0x00   | 8-bit unsigned int                 | 1            | 0xA5                                          |
| 0x01   | String                             | 8            | File name                                     |
| 0x09   | 8-bit unsigned int                 | 1            | File type                                     |
| 0x0A   | 16-bit unsigned int, little-endian | 2            | Length                                        |
| 0x0C   | 16-bit unsigned int, little-endian | 2            | Segment                                       |
| 0x0E   | 16-bit unsigned int, little-endian | 2            | Offset                                        |
| 0x10   | String                             | 240          | Padding, bytes equal to second byte of offset |

File names are padded with spaces (0x20) and case-sensitive.

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

Data files are stored as a series of sections following the directory entry, each containing a single block.

The block in non-terminal sections is in the following format:

| Offset | Type               | Size (bytes) | Content |
| ------ | ------------------ | ------------ | ------- |
| 0x00   | 8-bit unsigned int | 1            | 0x00    |
| 0x01   | String             | 255          | Data    |

The block in terminal sections (which represent the end of the file) is in the following format:

| Offset   | Type               | Size (bytes)   | Content                                   |
| -------- | ------------------ | -------------- | ----------------------------------------- |
| 0x00     | 8-bit unsigned int | 1              | Length, including this byte               |
| 0x01     | String             | (length) - 1   | Data                                      |
| (length) | String             | 256 - (length) | Padding, bytes equal to last byte written |

Data is buffered in memory until there are 255 bytes to write, at which point a non-terminal section is written.  When closing an open file, a terminal section is always written; if there is no buffered data to write, length is equal to 0x01 and data is empty.

Data files' directory entries are written with the length, segment, and offset last used when writing a directory entry to the cassette; these values are ignored when reading.  Curiously, if a BASIC ASCII listing was most recently saved before opening a data file for writing, the data file's type will be 0x40 in its directory entry, representing a BASIC ASCII listing rather than a data file.

Lines of text are delimited by CRs only.

A file of any type except a BSAVE memory image may be opened by the OPEN command, but it will be interpreted as described above.  Extensions are ignored by the OPEN command.

### BSAVE Memory Images

Memory images are written and read using BASIC's BSAVE and BLOAD statements.

Memory images are stored as a single section following the directory entry, containing the data in the region of memory specified by the BSAVE command.  The segment, offset, and length fields in the memory image's directory entry contain the parameters specified by the DEF SEG and BSAVE commands.

BLOAD uses the length value from the directory entry but ignores the segment and offset values.  BLOAD will only open memory images and ignores other file types.

### BASIC Programs

Tokenized BASIC programs' directory entries are written with the segment last defined by the DEF SEG statement (0x0060 by default) and the offset 0x081E, but these are ignored when reading.  The length is the actual length of the tokenized BASIC program and this is used when reading.

ASCII BASIC listings are written in the same manner as data files, described above.

BASIC programs are loaded by the LOAD statement, which ignores extensions if provided and will attempt to load any type of file.

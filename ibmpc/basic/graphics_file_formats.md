# Graphics File Formats Native to BASIC


## Conventions Used In This Document

"BASIC" is here used to refer to Microsoft BASIC for the IBM PC and all its various forms, including BASICA, GWBASIC, QBASIC, and QuickBASIC.  Not all screen modes are supported by all versions of BASIC.

Graphics adapters, when referred to, are the minimum graphics adapter compatible with a given screen mode.  EGA and MCGA are backward compatible with CGA; VGA is backward compatible with CGA, EGA, and MCGA.

All multi-byte numbers are little-endian; structs are never aligned.


## BSV

All graphics file formats that might be considered "native" to BASIC are based around the BSV file format.  "BSV" is an unofficial file extension which is short for BSAVE, the name of a BASIC command that quickly dumps an area of memory to a file.  The BSAVE command and its counterpart BLOAD exist in several dialects of Microsoft BASIC across multiple different platforms, but BASIC on the IBM PC uses the following format:

| Name      | Type    | Description                                                       |
| --------- | ------- | ----------------------------------------------------------------- |
| `magic`   | uint8   | Always 0xFD                                                       |
| `segment` | uint16  | The segment component of the address from which memory was dumped |
| `offset`  | uint16  | The offset within the segment from which memory was dumped        |
| `length`  | uint16  | The length, in bytes, of the memory dump                          |
| `data`    | uint8[] | The memory dump itself, `length` bytes in length                  |
| `trailer` | uint8   | Some versions of BASIC omit this byte; always 0x1A if present     |


## GET and PUT

In the absence of true pointers, BASIC provides GET and PUT to transfer graphics data to and from numeric arrays (the contents of which can be stored in BSV files).  The arrays are most commonly BASIC short (16-bit, signed) integer arrays, but they can be of any type; BASIC simply overwrites the array's memory area with data in the following format:

| Name        | Type    | Description                                                                  |
| ----------- | ------- | ---------------------------------------------------------------------------- |
| `bpspp`     | uint16  | Number of bits per scanline per plane (width times bits per pixel per plane) |
| `scanlines` | uint16  | Number of scanlines                                                          |
| `data`      | uint8[] | Graphics data, length is ceil(`bpspp` / 8) * `planes` * `scanlines`          | 

The value of `planes` can be derived if the size of `data` is known (for example, from the `length` field in the BSV header).

Scanlines are stored in order from top to bottom, with each scanline consisting of bit-packed graphics data where the most significant bit corresponds to the left side of the screen.  If `planes` is greater than 1, the data for a single scanline consists of that scanline's data for each plane stored separately in sequence from the least significant plane to the most significant plane.

The number of bits per pixel varies by screen mode in BASIC:

| Screen Mode | Adapter | Colors | Bits/Pixel/Plane | Planes |
| ----------- | ------- | ------ | ---------------- | ------ |
| 1           | CGA     | 4      | 2                | 1      |
| 2           | CGA     | 2      | 1                | 1      |
| 7           | EGA     | 16     | 1                | 4      |
| 8           | EGA     | 16     | 1                | 4      |
| 9*          | EGA     | 4      | 1                | 2      |
| 9           | EGA     | 16     | 1                | 4      |
| 10          | EGA     | 4      | 1                | 2      |
| 11          | MCGA    | 2      | 1                | 1      |
| 12          | VGA     | 16     | 1                | 4      |
| 13          | MCGA    | 256    | 8                | 1      |

\* if EGA has only 64K of RAM

If `planes` is 2 or 4, the data is guaranteed to represent a planar EGA/VGA image of 4 or 16 colors with 1 bit per pixel per plane.  However, if `planes` is 1, the number of bits per pixel cannot be derived from the data stored by GET.


## Whole-Screen Dumps

BSV files can also contain dumps of an entire screen image, transferred directly to/from video memory instead of using an array with GET and PUT.  This is most commonly done with images in non-planar screen modes.  It is possible to save and load screen images in planar modes as well, but besides requiring some knowledge of the video adapter's control registers, a single planar screen mode image requires the creation of a separate BSV file for each plane, so it is rarely done.

The BSV's `segment` and `length` can be used to determine the screen mode:

| Screen Mode | Adapter | Resolution | Interlaced? | Colors | Segment | Length      |
| ----------- | ------- | ---------- | ----------- | ------ | ------- | ----------- |
| 0           | CGA     | 80x25 Text | No          | 16     | 0xB800  | 4000        |
| 1           | CGA     | 320x200    | Yes         | 4      | 0xB800  | 16192-16384 |
| 2           | CGA     | 640x200    | Yes         | 2      | 0xB800  | 16192-16384 |
| 11          | MCGA    | 640x480    | No          | 2      | 0xA000  | 38400       |
| 13          | MCGA    | 320x200    | No          | 256    | 0xA000  | 64000       |


## Palette

Because palettes are not memory-mapped, they cannot be directly BSAVEd.  QBASIC and QuickBASIC do, however, have a PALETTE USING command which downloads an array filled with palette settings into the graphics adapter, and a BSV of such an array may be considered BASIC's native palette format.

For MCGA/VGA modes 11, 12, and 13, the array used by PALETTE USING must be an array of BASIC long (32-bit, signed) integers, with the RGB values packed into them as follows:

| Name    | Type  | Description            |
| ------- | ----- | ---------------------- |
| `r`     | uint8 | Red intensity (0-63)   |
| `g`     | uint8 | Green intensity (0-63) |
| `b`     | uint8 | Blue intensity (0-63)  |
| `dummy` | uint8 | Always 0 (almost)      |

If the value in a given array position is -1 (i.e. 0xFFFFFFFF), the existing palette value remains untouched.

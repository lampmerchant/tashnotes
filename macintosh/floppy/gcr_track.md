# GCR Track Layout

As read by the IWM or SWIM chip, a GCR track consists of a sequence of 8-bit bytes where the most significant bit is always '1' and there are never more than two consecutive '0' bits.  Such bytes will henceforth be referred to as "IWM bytes".

In order to represent data with an arbitrary number of consecutive zeroes, a correspondence between six-bit "nibbles" and IWM bytes is defined.

TODO table

A GCR track consists of the following logical units, in order, as follows.


## Autosync Groups

Autosync groups consist of sequences of eight '1' bits separated by two '0' bits.  A sufficient number of autosync groups is guaranteed to synchronize the reader so that IWM bytes following the autosync groups have the MSb in the correct place, regardless of where the reader starts reading.

```
| = read starting point

|
111111110011111111001111111100111111110011111111
[0xFF  ]  [0xFF  ]  [0xFF  ]  [0xFF  ]  [0xFF  ]

 |
111111110011111111001111111100111111110011111111
 [0xFE  ] [0xFF  ]  [0xFF  ]  [0xFF  ]  [0xFF  ]

  |
111111110011111111001111111100111111110011111111
  [0xFC  ][0xFF  ]  [0xFF  ]  [0xFF  ]  [0xFF  ]

   |
111111110011111111001111111100111111110011111111
   [0xF9  ][0xFE  ] [0xFF  ]  [0xFF  ]  [0xFF  ]

    |
111111110011111111001111111100111111110011111111
    [0xF3  ][0xFC  ][0xFF  ]  [0xFF  ]  [0xFF  ]

     |
111111110011111111001111111100111111110011111111
     [0xE7  ][0xF9  ][0xFE  ] [0xFF  ]  [0xFF  ]

      |
111111110011111111001111111100111111110011111111
      [0xCF  ][0xF3  ][0xFC  ][0xFF  ]  [0xFF  ]

       |
111111110011111111001111111100111111110011111111
       [0x9F  ][0xE7  ][0xF9  ][0xFE  ] [0xFF  ]

        |
111111110011111111001111111100111111110011111111
          [0xFF  ]  [0xFF  ]  [0xFF  ]  [0xFF  ]

         |
111111110011111111001111111100111111110011111111
          [0xFF  ]  [0xFF  ]  [0xFF  ]  [0xFF  ]

          |
111111110011111111001111111100111111110011111111
          [0xFF  ]  [0xFF  ]  [0xFF  ]  [0xFF  ]
```

TODO How many before address mark?


## Address Mark

An address mark consists of the three IWM bytes `0xD5 0xAA 0x96`.


## Address Header

An address header consists of five values encoded from six-bit nibbles into IWM bytes:


### Cylinder Number Low

This nibble contains the lower six bits of the cylinder number.

| Bit | Description              |
| --- | ------------------------ |
| 5   | Bit 5 of cylinder number |
| 4   | Bit 4 of cylinder number |
| 3   | Bit 3 of cylinder number |
| 2   | Bit 2 of cylinder number |
| 1   | Bit 1 of cylinder number |
| 0   | Bit 0 of cylinder number |


### Sector Number

This nibble contains the sector number.

| Bit | Description            |
| --- | ---------------------- |
| 5   | Always 0               |
| 4   | Bit 4 of sector number |
| 3   | Bit 3 of sector number |
| 2   | Bit 2 of sector number |
| 1   | Bit 1 of sector number |
| 0   | Bit 0 of sector number |


### Head and Cylinder Number High

This nibble contains the head number and the upper bits of the cylinder number:

| Bit | Description                       |
| --- | --------------------------------- |
| 5   | Head number (0 = bottom, 1 = top) |
| 4   | Always 0                          |
| 3   | Always 0                          |
| 2   | Always 0                          |
| 1   | Always 0                          |
| 0   | Bit 6 of cylinder number          |


### Format

This nibble contains attributes of the format of the disk.

| Bit | Description                        |
| --- | ---------------------------------- |
| 5   | 0 = single-sided, 1 = double-sided |
| 4   | 0 = Macintosh/Apple II, 1 = Lisa   |
| 3   | Bit 3 of sector interleave ratio   |
| 2   | Bit 2 of sector interleave ratio   |
| 1   | Bit 1 of sector interleave ratio   |
| 0   | Bit 0 of sector interleave ratio   |

Known values:

| Value | Description                                     |
| ----- | ----------------------------------------------- |
| 0x02  | Macintosh single-sided disk with 2:1 interleave |
| 0x12  | Lisa single-sided disk with 2:1 interleave      |
| 0x22  | Macintosh double-sided disk with 2:1 interleave |
| 0x24  | ProDOS double-sided disk with 4:1 interleave    |


### Checksum

This nibble contains the value of the preceding four nibbles XORed together.


## Bit Slip Sequence

The bit slip sequence consists of the two bytes `0xDE 0xAA`.


## Autosync Groups

See above.

TODO How many before data mark?


## Data Mark

A data mark consists of the three IWM bytes `0xD5 0xAA 0xAD`.


## Data Sector Number

The sector number is encoded as a single nibble.


## Sector Data

The sector data (12 bytes of tags preceding 512 bytes of data) is first "mangled" using the GCR checksum algorithm, then groups of three bytes are transformed into groups of four nibbles as follows:

| Nibble | Nibble Bit | Byte | Byte Bit |
| ------ | ---------- | ---- | -------- |
| 1st    | 5          | 1st  | 7        |
| 1st    | 4          | 1st  | 6        |
| 1st    | 3          | 2nd  | 7        |
| 1st    | 2          | 2nd  | 6        |
| 1st    | 1          | 3rd  | 7        |
| 1st    | 0          | 3rd  | 6        |
| 2nd    | 5          | 1st  | 5        |
| 2nd    | 4          | 1st  | 4        |
| 2nd    | 3          | 1st  | 3        |
| 2nd    | 2          | 1st  | 2        |
| 2nd    | 1          | 1st  | 1        |
| 2nd    | 0          | 1st  | 0        |
| 3rd    | 5          | 2nd  | 5        |
| 3rd    | 4          | 2nd  | 4        |
| 3rd    | 3          | 2nd  | 3        |
| 3rd    | 2          | 2nd  | 2        |
| 3rd    | 1          | 2nd  | 1        |
| 3rd    | 0          | 2nd  | 0        |
| 4th    | 5          | 3rd  | 5        |
| 4th    | 4          | 3rd  | 4        |
| 4th    | 3          | 3rd  | 3        |
| 4th    | 2          | 3rd  | 2        |
| 4th    | 1          | 3rd  | 1        |
| 4th    | 0          | 3rd  | 0        |

There are 174 such groups, which transform 522 bytes into 696 nibbles.  The final 2 bytes of data are transformed into 3 nibbles as follows:

| Nibble | Nibble Bit | Byte | Byte Bit |
| ------ | ---------- | ---- | -------- |
| 1st    | 5          | 1st  | 7        |
| 1st    | 4          | 1st  | 6        |
| 1st    | 3          | 2nd  | 7        |
| 1st    | 2          | 2nd  | 6        |
| 1st    | 1          | -    | -        |
| 1st    | 0          | -    | -        |
| 2nd    | 5          | 1st  | 5        |
| 2nd    | 4          | 1st  | 4        |
| 2nd    | 3          | 1st  | 3        |
| 2nd    | 2          | 1st  | 2        |
| 2nd    | 1          | 1st  | 1        |
| 2nd    | 0          | 1st  | 0        |
| 3rd    | 5          | 2nd  | 5        |
| 3rd    | 4          | 2nd  | 4        |
| 3rd    | 3          | 2nd  | 3        |
| 3rd    | 2          | 2nd  | 2        |
| 3rd    | 1          | 2nd  | 1        |
| 3rd    | 0          | 2nd  | 0        |

Nibble bits marked `-` are left as zeroes.


## Sector Data Checksum

The three bytes of the sector data checksum are transformed into four nibbles in the same way as the 174 groups of data, described above.


## Bit Slip Sequence

The bit slip sequence consists of the two bytes `0xDE 0xAA`.

# GCR Checksum Algorithm

The GCR checksum algorithm is applied to the 524 bytes of sector data on GCR-encoded floppy disks.  While it calculates a three-byte value which is appended to the sector, it is more than simply a checksum as it causes the bytes of the sector to be mangled as it is calculated, a process which must be reversed to recover the original data.

It is a cursed algorithm which is unclearly or erroneously detailed in several documents.  This document shall attempt to set the record straight.

## Mangling

Mangling is the process by which the data bytes of the sector are encoded before being recorded onto a GCR disk.

### Pseudocode

  * Set *checksum_A* to 0.
  * Set *checksum_B* to 0.
  * Set *checksum_C* to 0.
  * Set *carry* to 0.
  * Loop:
    * Set *byte* to the next byte in the input data or exit loop now if there are no more bytes.
    * Set *checksum_A* to the sum of itself, *byte*, and *carry*.
    * If *checksum_A* is greater than 255:
      * Subtract 256 from *checksum_A*.
      * Set *carry* to 1.
    * Else:
      * Set *carry* to 0.
    * Set *byte* to the bitwise exclusive-OR (XOR) of itself and *checksum_C*.
    * Write *byte* to output.
    * Set *byte* to the next byte in the input data or exit loop now if there are no more bytes.
    * Set *checksum_B* to the sum of itself, *byte*, and *carry*.
    * If *checksum_B* is greater than 255:
      * Subtract 256 from *checksum_B*.
      * Set *carry* to 1.
    * Else:
      * Set *carry* to 0.
    * Set *byte* to the bitwise exclusive-OR (XOR) of itself and *checksum_A*.
    * Write *byte* to output.
    * Set *byte* to the next byte in the input data or exit loop now if there are no more bytes.
    * Set *checksum_C* to the sum of itself, *byte*, and *carry*.
    * If *checksum_C* is greater than 255:
      * Subtract 256 from *checksum_C*.
      * Set *carry* to 1.
    * Else:
      * Set *carry* to 0.
    * Set *byte* to the bitwise exclusive-OR (XOR) of itself and *checksum_B*.
    * Write *byte* to output.
    * If *checksum_C* is greater than 127:
      * Subtract 128 from *checksum_C*.
      * Set *carry* to 1.
      * Multiply *checksum_C* by 2.
      * Add 1 to *checksum_C*.
    * Else:
      * Set *carry* to 0.
      * Multiply *checksum_C* by 2.

### Python 3.x Code

```
def mangle(data, checksum_a=0, checksum_b=0, checksum_c=0):
  def _mangle_gen(data):
    nonlocal checksum_a, checksum_b, checksum_c
    data = iter(data)
    carry = 0
    while True:
      try:
        byte = next(data)
      except StopIteration:
        break
      carry, checksum_a = divmod(checksum_a + byte + carry, 256)
      byte ^= checksum_c
      yield byte
      try:
        byte = next(data)
      except StopIteration:
        break
      carry, checksum_b = divmod(checksum_b + byte + carry, 256)
      byte ^= checksum_a
      yield byte
      try:
        byte = next(data)
      except StopIteration:
        break
      carry, checksum_c = divmod(checksum_c + byte + carry, 256)
      byte ^= checksum_b
      yield byte
      carry = 1 if (checksum_c & 0x80) else 0
      checksum_c = ((checksum_c << 1) & 0xFF) | carry
  return bytes(_mangle_gen(data)), bytes((checksum_a, checksum_b, checksum_c))
```

## Unmangling

Unmangling is the process by which the data bytes of the sector are decoded after being read from a GCR disk.

### Pseudocode

  * Set *checksum_A* to 0.
  * Set *checksum_B* to 0.
  * Set *checksum_C* to 0.
  * Loop:
    * If *checksum_C* is greater than 127:
      * Subtract 128 from *checksum_C*.
      * Set *carry* to 1.
      * Multiply *checksum_C* by 2.
      * Add 1 to *checksum_C*.
    * Else:
      * Set *carry* to 0.
      * Multiply *checksum_C* by 2.
    * Set *byte* to the next byte in the input data or exit loop now if there are no more bytes.
    * Set *byte* to the bitwise exclusive-OR (XOR) of itself and *checksum_C*.
    * Write *byte* to output.
    * Set *checksum_A* to the sum of itself, *byte*, and *carry*.
    * If *checksum_A* is greater than 255:
      * Subtract 256 from *checksum_A*.
      * Set *carry* to 1.
    * Else:
      * Set *carry* to 0.
    * Set *byte* to the next byte in the input data or exit loop now if there are no more bytes.
    * Set *byte* to the bitwise exclusive-OR (XOR) of itself and *checksum_A*.
    * Write *byte* to output.
    * Set *checksum_B* to the sum of itself, *byte*, and *carry*.
    * If *checksum_B* is greater than 255:
      * Subtract 256 from *checksum_B*.
      * Set *carry* to 1.
    * Else:
      * Set *carry* to 0.
    * Set *byte* to the next byte in the input data or exit loop now if there are no more bytes.
    * Set *byte* to the bitwise exclusive-OR (XOR) of itself and *checksum_B*.
    * Write *byte* to output.
    * Set *checksum_C* to the sum of itself, *byte*, and *carry*.
    * If *checksum_C* is greater than 255:
      * Subtract 256 from *checksum_C*.
      * Set *carry* to 1.
    * Else:
      * Set *carry* to 0.

### Python 3.x Code

```
def unmangle(data):
  checksum_a = checksum_b = checksum_c = 0
  def _unmangle_gen(data):
    nonlocal checksum_a, checksum_b, checksum_c
    data = iter(data)
    while True:
      carry = 1 if (checksum_c & 0x80) else 0
      checksum_c = ((checksum_c << 1) & 0xFF) | carry
      try:
        byte = next(data)
      except StopIteration:
        break
      byte ^= checksum_c
      yield byte
      carry, checksum_a = divmod(checksum_a + byte + carry, 256)
      try:
        byte = next(data)
      except StopIteration:
        break
      byte ^= checksum_a
      yield byte
      carry, checksum_b = divmod(checksum_b + byte + carry, 256)
      try:
        byte = next(data)
      except StopIteration:
        break
      byte ^= checksum_b
      yield byte
      carry, checksum_c = divmod(checksum_c + byte + carry, 256)
  return bytes(_demangle_gen(data)), bytes((checksum_a, checksum_b, checksum_c))
```

## Example

### Unmangled Data

```
000  54 41 47 53 54 41 47 53 54 41 47 53 44 41 54 41  TAGSTAGSTAGSDATA
010  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
020  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
030  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
040  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
050  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
060  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
070  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
080  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
090  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
0A0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
0B0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
0C0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
0D0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
0E0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
0F0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
100  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
110  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
120  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
130  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
140  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
150  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
160  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
170  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
180  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
190  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
1A0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
1B0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
1C0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
1D0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
1E0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
1F0  44 41 54 41 44 41 54 41 44 41 54 41 44 41 54 41  DATADATADATADATA
200  44 41 54 41 44 41 54 41 44 41 54 41              DATADATADATA    
```

### Mangled Data

```
000  54 15 06 DD F3 D4 D8 BC BC A6 76 63 32 34 25 D4  T.........vc24%.
010  F3 F4 F9 4D B3 A2 1A 0A 0E D3 D8 7C 90 91 A8 69  ...M.......|...i
020  56 C3 3D 27 C3 EF F3 F6 B4 AA A5 04 69 2D D2 C0  V.='........i-..
030  1F 8A 96 1E 49 46 17 23 0C 4E F0 DD E4 A9 89 A8  ....IF.#.N......
040  6E 63 02 3F 27 74 94 F6 A8 45 AD C1 01 7C D1 DB  nc.?'t...E...|..
050  3A D2 88 93 ED 60 40 BC 27 19 28 F6 DD C0 AD AC  :....`@.'.(.....
060  C1 7F 7B F4 C4 32 B9 94 F0 23 42 48 0D 1A 1E 7A  ..{..2...#BH...z
070  D9 CF AC B0 8B 39 66 65 31 36 31 D2 FD E8 FD 4F  .....9fe161....O
080  AF 9A 04 7E 7E D5 D4 5C 92 85 E8 6B 42 43 3F 1B  ...~~..\...kBC?.
090  C2 EE CF F4 B5 9E B9 07 65 25 D3 34 0F 8D E2 7E  ........e%.4...~
0A0  4A BA 55 22 78 CD F0 D1 DF A8 85 9E 6F 57 16 3E  J.U"x.......oW.>
0B0  13 4C 95 EA D8 44 B9 A0 03 70 12 DA 36 54 89 87  .L...D...p..6T..
0C0  E8 60 5C 43 26 0D D2 F7 C9 D4 AC A0 E9 7C 77 45  .`\C&........|wE
0D0  3A 26 DE 93 EC DC 40 BC F9 18 6A 65 DE C3 9E AF  :&....@...je....
0E0  87 5D 7B 59 F8 32 0D 61 F0 DC 96 49 9B 4D 1D 72  .]{Y.2.a...I.M.r
0F0  D8 CC 20 A0 8B F9 10 62 BE 50 31 0F E2 E8 DB B4  .. ....b.P1.....
100  AF 92 38 7E 51 22 D5 28 0D 87 FE 7A 4C AE AC 1D  ..8~Q".(...zL...
110  74 3B C9 C5 33 A3 91 D6 60 4B E7 38 0F AE EF DE  t;..3...`K.8....
120  24 BE B4 28 05 64 03 D7 22 76 8C FB B4 5D A8 C9  $..(.d.."v...]..
130  19 01 C1 F3 C5 F2 A0 94 AD 78 43 3D 3E 1A 2F 8E  .........xC=>./.
140  D8 3E 45 B0 C5 17 66 EC DC 37 89 AC F3 03 7A 4D  .>E...f..7....zM
150  4D 32 19 FB F0 D0 A3 48 97 37 1E 66 35 CE 3C CA  M2.....H.7.f5.<.
160  95 ED CD 67 AA FA 3C 03 BF EC D7 DF AA 86 EB 02  ...g..<.........
170  4D 49 D0 1C D6 89 CA 8C 4E A2 48 1F 60 83 CB 39  MI......N.H.`..9
180  42 A2 ED F5 60 BF BF 38 7B 21 EE D2 07 BE 80 6E  B...`..8{!.....n
190  05 58 B6 D4 1E 0D 82 EF 5F 5A A4 9F 18 75 14 F1  .X......_Z...u..
1A0  31 48 A6 88 D0 77 5F 92 3C 0E 7C 8D D4 99 59 A4  1H...w_.<.|...Y.
1B0  76 10 72 4E D7 2B F4 A6 EF 88 7D 41 52 2C 15 95  v.rN.+....}AR,..
1C0  EA C4 7E 43 83 9D 17 5A 78 C6 08 60 8D E1 90 5C  ..~C...Zx..`...\
1D0  A6 51 0A 77 E0 E3 23 B0 A0 FA 30 79 B9 10 2E 10  .Q.w..#...0y....
1E0  63 FE C6 B7 44 96 3A 15 5C 26 C2 2D 15 9A F9 0A  c...D.:.\&.-....
1F0  59 B3 4D 33 77 FD E7 C6 BF B9 9C DF 7D 4C 95 CF  Y.M3w.......}L..
200  0A 4B 9B E3 D2 52 B0 94 11 69 7A E8              .K...R...iz.    
```

| Checksum Byte | Value in Hexadecimal |
| ------------- | -------------------- |
| *checksum_A*  | A9                   |
| *checksum_B*  | 69                   |
| *checksum_C*  | 2E                   |

## References

  * [[http://bitsavers.org/pdf/apple/disk/sony/uPD72070_Specification_1991.pdf|uPD72070 Floppy Disk Controller Specification]], Page 6
  * [[http://bitsavers.org/pdf/apple/disk/sony/SWIM_Chip_Users_Ref_198801.pdf|SWIM Chip User's Reference, Revision 1.5 (11-Jan-1988)]], Page 6

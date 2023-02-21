# Details Missing or Inaccurate in the [May 1985 DCD Document](http://bitsavers.trailing-edge.com/pdf/apple/disk/hd20/Directly_Connected_Disks_Specification_1.2a_May85.pdf)

* The sync byte, in either direction, is always 0xAA, 0x96 is not used.
* When Mac is transmitting a command, the sync byte is followed by two more raw IWM bytes before the 7-to-8 groups begin. DCD transmits only a sync byte and does not transmit these extra bytes.
   * The first is 0x80 plus the number of 7-to-8 groups in the command being transmitted by Mac.
   * The second is 0x80 plus the number of 7-to-8 groups that Mac expects to receive in response.
* The holdoff protocol is completely different than specified.
   * In either direction, a holdoff is initiated by Mac transitioning from state 1 to state 0.
   * If Mac is transmitting, it will finish the 7-to-8 group that it has begun transmitting. This data is valid.
   * If DCD is transmitting, it must finish the 7-to-8 group that it has begun transmitting. This data will be treated by Mac as valid.
   * A holdoff is ended by Mac transitioning from state 0 to state 1. There is no negotiation.
   * If Mac is transmitting and WR is high at the end of the last byte transmitted before holdoff, it will transition WR from high to low right before transitioning back to state 1.
   * Mac will resume transmission with an 0xAA byte, followed by the bytes in the next group after the group where the holdoff began.
   * DCD must resume transmission with an 0xAA byte, followed by the bytes in the next group after the group where the holdoff began.
* The Controller Status (command 0x03) block is slightly different than specified.
   * The total size of the Controller Status block is 336 bytes, not 532 bytes.
      * 336 bytes of data, 6 byte header, checksum byte == 343 bytes == 49 7-to-8 groups
   * The Icon field contains a 32x32 icon as a 1-bit bitmap, followed by its 32x32 mask, also as a 1-bit bitmap, for a total of 256 bytes.
      * The format of the bitmaps is identical to that of ICON resources.
   * The Filler field is replaced by a 16-byte Pascal string (first byte is length) that determines what appears in the "Where:" field of the Get Info dialog box.
* The checksum byte is chosen such that all data bytes in all 7-to-8 groups (not including the sync byte or the command/response length IWM bytes) sum to 0 modulo 256.

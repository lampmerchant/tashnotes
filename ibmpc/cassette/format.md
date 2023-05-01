# IBM PC 5150 Cassette Format

## Bit Format

Bits are encoded on the cassette as a single period of a wave with a 50% duty cycle.  The period is 500µs for a zero bit and 1000µs for a one bit.  

The effective bitrate is between 1 and 2 kbps, depending on content, with an average of 1.5 kbps (187.5 bytes per second), and one side of a 90-minute cassette can be expected to hold approximately 500 kilobytes of data when filled.

## Byte Format

8-bit bytes are written and read on the cassette MSB to LSB.

## Block Format

Blocks of 256 bytes of data are written and read on the cassette followed by a 16-bit CRC.

### CRC Algorithm

The CRC at the end of each block uses the CCITT polynomial (0x1021).  The register is initialized to 0xFFFF and is complemented before being written to the cassette in big-endian order.  As such, the register should equal 0x1D0F when a block followed by its correct CRC are fed into it.

## Section Format

A section is composed of, in order:
 * Leader of one bits
    * INT 15h routine uses a leader of 2048 one bits (256 0xFF bytes) when writing
    * INT 15h routine expects a leader of at least 512 one bits (64 0xFF bytes) when reading
 * Synchronization zero bit
 * Synchronization byte (0x16, ASCII SYN)
 * One or more 256-byte blocks of data, each followed by its 2-byte CRC
 * Trailer of 32 one bits (4 0xFF bytes)

### Limitations

Calls to INT 15h to read from the cassette expect to read an entire section and stop at the beginning of the leader of the next section, but it has no way of knowing the number of blocks in a section and relies only on the byte count given to it by the caller.

If the number of bytes requested to be read by INT 15h is less than the number of bytes contained in the section on the cassette, the read will stop after the number of blocks needed to read the requested number of bytes, leaving the tape head in the middle of a section.  This will likely cause future read calls to fail to find a leader, and in rare cases could result in data being incorrectly interpreted as a leader by the next read call before the next actual leader is found.

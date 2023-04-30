# IBM PC 5150 Cassette Format

## Bit Format

Bits are encoded on the cassette as a single period of a wave with a 50% duty cycle.  The period is 500µs for a zero bit and 1000µs for a one bit.

## Byte Format

8-bit bytes are written and read on the cassette MSB to LSB.

## Block Format

Blocks of 256 bytes of data are written and read on the cassette followed by a 16-bit CRC.

### CRC Algorithm

The CRC at the end of each block uses the CCITT polynomial (0x1021).  The register is initialized to 0xFFFF and is complemented before being written to the cassette in big-endian order.  As such, the register should equal 0x1D0F when a block followed by its correct CRC are fed into it.

## Section Format

A section is composed of, in order:
 * Leader of at least 2048 one bits (256 0xFF bytes)
 * Zero bit
 * Synchronization byte (0x16, ASCII SYN)
 * One or more 256-byte blocks of data, each followed by its 2-byte CRC
 * Trailer of 32 one bits (4 0xFF bytes)
 * Period of silence

A call to INT 15h will write or read only at the section granularity.  If the number of bytes requested to be read is less than the number of bytes contained in the section, the next read will begin at the next section, not where the previous read left off.

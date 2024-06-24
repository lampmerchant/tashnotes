# PowerKey

This document contains experimentally-observed behavior of the ADB protocol of the Sophisticated Circuits PowerKey Classic (PK-1). It contains enough information to satisfactorally emulate the device, however, a few unknowns exist.

The PowerKey's default address is 0x7 and its default handler ID is 0x22.


## Register 0

Register 0 is a 16-bit register.  Its upper byte can be written by the host using a Listen 0 command, however its lower byte is read-only and its value in the Listen 0 command will be ignored.  Register 0 can only be read when bit 8 of it is set, otherwise Talk 0 produces no reply.

| Bit  | Description                                                                                          |
| ---- | ---------------------------------------------------------------------------------------------------- |
| 15:9 | Undetermined, appears to always be set to 0                                                          |
| 8    | If set, Talk 0 returns the contents of register 0; if clear, it returns nothing                      |
| 7    | If set, relay is closed and outlets are powered; if clear, relay is open and outlets are not powered |
| 6    | If set, relay was last closed as a result of register 1 overflowing                                  |
| 5    | If set, relay was last closed as a result of the power key being pressed on the keyboard             |
| 4    | Undetermined, appears to always be set to 0                                                          |
| 3:0  | Firmware version (see below)                                                                         |

| Bits 3:0 | Firmware Version |
| -------- | ---------------- |
| 0b0000   | 1.1              |
| 0b0001   | 1.2              |
| 0b0010   | 1.3              |
| 0b0011   | 1.4              |
| 0b0100   | 1.5              |
| 0b0101   | 1.6              |
| 0b0110   | 1.7              |
| 0b0111   | 1.8              |
| 0b1000   | 1.9              |
| 0b1001   | 2.0              |
| 0b1010   | 2.1              |
| 0b1011   | 2.2              |
| 0b1100   | 2.3              |
| 0b1101   | 2.4              |
| 0b1110   | 2.5              |
| 0b1111   | 2.6              |


## Registers 1 and 2

Registers 1 and 2 are both readable (with Talk) and writable (with Listen) 32-bit timers which increment when their high byte is nonzero.  When register 1 overflows from 0xFFFFFFFF to 0, if the relay was open, it closes and sets bits 7 and 6 of register 0.  When register 2 overflows from 0xFFFFFFFF to 0, if the relay was closed, it opens and clears bits 7, 6, and 5 of register 0.

The timers associated with registers 1 and 2 were observed to increment at a rate of 60 Hz.  It is speculated, though not proven, that the timers increment once per cycle of the mains AC waveform and therefore would increment at a rate of 50 Hz if connected to a 50 Hz electric supply (instead of the 60 Hz supply on which they were tested).  On startup, version 3.4 of the driver software sets register 1 then reads its value after approximately half a second, which supports this hypothesis.


## Register 3

The Exceptional Event bit is always set.  The Service Request Enable bit is always clear.


## Device Information

The software always reports the PowerKey Classic as having a serial number of 0.  No register on the device appears to contain this information.


### Observed Values from PK-1 Units

| Serial Number | Firmware Version |
| ------------- | ---------------- |
| 09977         | 1.2              |
| 66276         | 1.4              |

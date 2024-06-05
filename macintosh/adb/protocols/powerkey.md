# PowerKey

This document contains experimentally-observed behavior of the ADB protocol of the Sophisticated Circuits PowerKey Classic (PK-1). It contains enough information to satisfactorally emulate the device, however, a few unknowns exist.

The PowerKey's default address is 0x7 and its default handler ID is 0x22.

## Register 0

Register 0 is a 16-bit register.  Its upper byte can be written by the host using a Listen 0 command, however its lower byte is read-only and its value in the Listen 0 command will be ignored.  Register 0 can only be read when its bit 8 is set, otherwise Talk 0 produces no reply.

| Bit  | Description                                                                                          |
| ---- | ---------------------------------------------------------------------------------------------------- |
| 15:9 | Undetermined, appears to always be set to 0                                                          |
| 8    | If set, Talk 0 returns the contents of register 0; if clear, it returns nothing                      |
| 7    | If set, relay is closed and outlets are powered; if clear, relay is open and outlets are not powered |
| 6    | If set, relay was last closed as a result of register 1 overflowing                                  |
| 5    | If set, relay was last closed as a result of the power key being pressed on the keyboard             |
| 4:2  | Undetermined, appears to always be set to 0                                                          |
| 1:0  | Undetermined, appears to always be set to 1                                                          |

## Registers 1 and 2

Registers 1 and 2 are both readable (with Talk) and writable (with Listen) 32-bit timers which increment 60 times per second when their high byte is nonzero.  When register 1 overflows from 0xFFFFFFFF to 0, if the relay was open, it closes and sets bits 7 and 6 of register 0.  When register 2 overflows from 0xFFFFFFFF to 0, if the relay was closed, it opens and clears bits 7, 6, and 5 of register 0.

## Register 3

The Exceptional Event bit is always set.  The Service Request Enable bit is always clear.

## Device Information

The software always appears to report the PowerKey Classic as having a serial number of 0 and a firmware revision of 1.4.  No register on the device appears to contain this information.

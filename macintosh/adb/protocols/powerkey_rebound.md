# PowerKey Rebound!

This document contains experimentally-observed behavior of the ADB protocol of the Sophisticated Circuits PowerKey Rebound! (PKRB-M). It contains enough information to satisfactorally emulate the device, however, a few unknowns exist.

The Rebound!'s default address is 0x7 and its default handler ID is 0x87.

## ADB Registers

The Rebound! has eight registers that are accessed using ADB registers 1 and 2.  ADB register 1 controls which Rebound! register is reflected in ADB register 2 - Listen 2 will set the contents of the register, Talk 2 will read the contents of the register.

## Rebound! Registers

### Register 0

16 bits in length, purpose unknown, default value 0x0102.  Version 1.1 of the software does not read it.

### Register 1

16 bits in length, purpose unknown, default value 0x1000.  Version 1.1 of the software reads it when the extension is loaded.

### Register 2

32 bits in length, contains the serial number of the Rebound! dongle.  Version 1.1 of the software reads it when the extension is loaded.

### Registers 3-5

Register 3 is 16 bits in length and contains a counter which decrements by 1 once per second.  When it reaches 0, if register 5 is nonzero, register 5 is decremented, register 3 is set to the value in register 4, and the Mac is restarted by a Cmd-Ctrl-Reset keystroke emulated on the keyboard.  If register 5 is zero when register 3 decrements to zero, registers 3 and 5 remain at zero and the Mac is not restarted.

Version 1.1 of the software, by default, sets registers 3 and 4 to 0x11E (286) and register 5 to 1, then sets register 3 to 0x11E once per minute.

### Register 6

16 bits in length, purpose unknown, default value 0x0000.  Version 1.1 of the software does not read it.

### Register 7

16 bits in length, purpose unknown, default value 0x0000.  Version 1.1 of the software does not read it.

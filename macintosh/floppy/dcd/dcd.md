# Directly Connected Disks 

In order to be able to connect hard drives to Macintosh computers before adopting SCSI with the Macintosh Plus, Apple created the **Directly Connected Disks** (DCD) specification.

DCD uses the floppy disk drive controller (IWM/SWIM) as a high-speed serial interface communicating a high-level protocol at 47/96 (~0.490) MHz using a 7-to-8 encoding which reduces the effective speed to 329/768 (~0.428) MHz.  This is almost double the maximum speed (230.4 kHz) that the SCC can achieve when internally clocked, but considerably less than the speed achievable using SCSI.

Multiple DCD devices can be connected to a single Macintosh in a daisy chain, optionally with a single floppy disk drive at the end of the chain.  The protocol places no limit on the number of chained DCD devices, but in practice, it is limited by ROM support to 2 or 4.  On Macs that only support 2 chained DCD devices, connecting a chain of more than 2 will cause the Mac to only recognize the first.

All signals from the IWM/SWIM are common to all floppy disk drive connectors in a Macintosh except for the enable (!ENBL) signal; the IWM/SWIM has two of these (!ENBL1 and !ENBL2) with the intention of being able to control two floppy disk drives.  All known Macintosh ROMs that have support for DCD will only recognize DCD devices when !ENBL2 is used as the !ENBL signal.  When a Macintosh has an external 19-pin D-sub[^1] floppy disk drive connector, !ENBL2 is always routed to it, but !ENBL2 can still be used to connect devices even on a Macintosh that does not have such a connector.


## Macintosh Compatibility Matrices

Cells marked with ? are speculated to be true but untested.


### Support in Native ROM

| Model         | !ENBL2 Location               | Max Devices | Tested By         |
| ------------- | ----------------------------- | ----------- | ----------------- |
| 512Ke         | External 19-Pin D-Sub         | 4           | Tashtari          |
| Plus          | External 19-Pin D-Sub         | 4           |                   |
| SE            | External 19-Pin D-Sub         | 2           |                   |
| IIci          | External 19-Pin D-Sub         | 2?          |                   |
| Portable      | External 19-Pin D-Sub         | 2?          | WillJac@68kMLA    |
| Classic       | External 19-Pin D-Sub         | 2           | Tashtari          |
| LC            | Second Internal Floppy Header | 2?          |                   |
| IIsi          | External 19-Pin D-Sub         | 2?          |                   |
| Classic II    | External 19-Pin D-Sub         | 2           | Tashtari          |
| PowerBook 100 | External HDI-20 Connector     | 2?          | David Cook@68kMLA |
| LC II         | SWIM PLCC Pin 19              | 2?          | Tashtari          |
| IIvi          | SWIM PLCC Pin 19              | 2?          |                   |
| IIvx          | SWIM PLCC Pin 19              | 2?          | Fizzbinn@68kMLA   |


### Support with Hard Disk 20 Patch

Official patch distributed with System 2.1.

| Model | !ENBL2 Location       | Max Devices | Tested By |
| ----- | --------------------- | ----------- | --------- |
| 512K  | External 19-Pin D-Sub | 4?          |           |


### Support with MacIIHD20 Patch

Unofficial patch distributed on "Developer CD Series 1990: Night of the Living Disc", available [here](https://discmaster.textfiles.com/browse/31867/bincue/Night%20of%20the%20Living%20Disc.bin/Storage%20&%20Communications/Hard%20Disks/HD%2020%20Drivers%201.1/).

| Model | !ENBL2 Location               | Max Devices | Tested By         |
| ----- | ----------------------------- | ----------- | ----------------- |
| II    | Second Internal Floppy Header | 2?          |                   |
| IIx   | Second Internal Floppy Header | 2?          | David Cook@68kMLA |
| SE/30 | External 19-Pin D-Sub         | 2?          | David Cook@68kMLA |
| IIcx  | External 19-Pin D-Sub         | 2?          | David Cook@68kMLA |


### Support with BMOW ROMinator

Unofficial ROM SIMM for various 68030-based Macs, available [here](https://www.bigmessowires.com/mac-rom-inator-ii/).

| Model | !ENBL2 Location               | Max Devices | Tested By    |
| ----- | ----------------------------- | ----------- | ------------ |
| IIx   | Second Internal Floppy Header | 2?          |              |
| SE/30 | External 19-Pin D-Sub         | 2?          |              |
| IIcx  | External 19-Pin D-Sub         | 2           | demik@68kMLA |


## Macintosh System Software Compatibility Matrix

| Version | Compatible? |
| ------- | ----------- |
| 6.0.8   | Yes         |
| 7.1     | Yes         |
| 7.5     | No          |


## Implementations

### Hard Disk 20

The Hard Disk 20 was the only contemporaneous product released by Apple or any other company following the DCD specification.

More Info:
  * [Hard Disk 20](https://en.wikipedia.org/wiki/Hard_Disk_20)


### Floppy Emu

In 2014, the Floppy Emu by Big Mess O' Wires added support for DCD.

More Info:
  * [Floppy Emu Homepage](https://www.bigmessowires.com/floppy-emu/)
  * [Blog Post, February 2014](https://www.bigmessowires.com/2014/02/06/emulating-the-apple-hd20/)
  * [Blog Post, November 2014](https://www.bigmessowires.com/2014/11/22/reverse-engineering-the-hd20/)


### TashTwenty

TashTwenty is an open source firmware implementation of DCD written by Tashtari for the Microchip PIC16F1704.

More Info:
  * [GitHub Repository](https://github.com/lampmerchant/tashtwenty/)
  * [68kMLA Forum Thread](https://68kmla.org/bb/index.php?threads/tashtwenty-single-chip-dcd-hard-disk-20-interface.39357/)


## References

  * [Directly Connected Disks Specification 1.2a](http://bitsavers.trailing-edge.com/pdf/apple/disk/hd20/Directly_Connected_Disks_Specification_1.2a_May85.pdf)
    * Note that this document has several differences from the protocol as implemented.

[^1]: Often referred to as "DB-19", but this is incorrect as "DB" in the de facto [D-subminiature](https://en.wikipedia.org/wiki/D-subminiature) standard refers to the shell size which accommodates 25 pins and is used for PC parallel ports.

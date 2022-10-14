# Directly Connected Disks Protocol

## Daisy-Chaining Devices

The DCD protocol allows for daisy-chaining multiple DCD devices to one floppy disk port, optionally with one floppy disk drive at the end of the chain.  This is accomplished using the CA3 line.

When !ENBL is asserted by the Macintosh, the first DCD device in the chain is selected and the !ENBL line on the first DCD device's daisy chain port is unasserted.  A pulse on the CA3 line will cause the first DCD device to assert the !ENBL line on its daisy chain port and ignore further activity from the Macintosh until !ENBL from the Macintosh is deasserted and then reasserted.  Subsequent devices in the daisy chain will behave the same way, allowing for a chain of devices limited only by the electrical characteristics of the cabling used, though in practice the Macintosh ROM supports only 2 or 4 daisy-chained DCD devices.

A floppy disk drive on the end of the daisy chain will be selected in the same way a DCD device would be, except that it has no way of selecting a subsequent device and therefore ends the daisy chain.

## Phase Lines and Multiplexing

Similar to the MCI (Microfloppy Control Interface), the "Phase" CA0, CA1, and CA2 lines are used to multiplex signals between the device and the Macintosh.

| Signal Name | Direction | Meaning   | Asserted...                                                       |
| ----------- | --------- | --------- | ----------------------------------------------------------------- |
| !HSHK       | <-        | Handshake | To indicate readiness to transfer data                            |
| HOST        | ->        | Host      | When a data transfer is starting/while a data transfer is ongoing |
| HOFF        | ->        | Holdoff   | To temporarily suspend a data transfer                            |
| RESET       | ->        | Reset     | To cause the DCD to perform the equivalent of a power-on reset    |

| State | CA2  | CA1  | CA0  | HOST | HOFF | RESET | RD Function |
| ----- | ---- | ---- | ---- | ---- | ---- | ----- | ----------- |
| 0     | Low  | Low  | Low  | High | High | Low   | Data        |
| 1     | Low  | Low  | High | High | Low  | Low   | Data        |
| 2     | Low  | High | Low  | Low  | Low  | Low   | !HSHK       |
| 3     | Low  | High | High | High | Low  | Low   | !HSHK       |
| 4     | High | Low  | Low  | Low  | Low  | High  | --          |
| 5     | High | Low  | High | Low  | Low  | Low   | Drive Low   |
| 6     | High | High | Low  | Low  | Low  | Low   | Drive High  |
| 7     | High | High | High | Low  | Low  | Low   | Drive High  |

During startup, the Macintosh will use states 5, 6, and 7 to detect that a DCD device is connected.

Note that the Macintosh can only change one of the phase lines at a time, so it cannot, for example, transition directly from state 3 to state 0.

## Data Encoding

When transferring data, the IWM (and the SWIM in IWM mode) communicate in 8-bit bytes, transmitted from most to least significant bit, of which the most significant bit is always 1.  The WR line from the Macintosh to the DCD device uses NRZI encoding, in which an inversion of the signal denotes a 1 bit and no inversion denotes a 0 bit.  The RD line from the DCD device to the Macintosh uses a variation on NRZI encoding where only falling edges are significant, in which a falling edge on the signal denotes a 1 bit and no falling edge denotes a 0 bit.

Because the MSB of every byte transmitted must be set, a 7-to-8 encoding is used in order to transmit 8-bit bytes.  Each of a group of seven 8-bit bytes is shifted right one bit and has its MSB set, with the LSBs of the group (in order from last to first) forming an eighth byte, also with its MSB set.

```
Unencoded:  Rotate 1 in:   Rotate LSB out:
00110001    1 -> 00110001  10011000 -> 1 -------
00110010    1 -> 00110010  10011001 -> 0 ------ |
00110011    1 -> 00110011  10011001 -> 1 ----- ||
00110100    1 -> 00110100  10011010 -> 0 ---- |||
00110101    1 -> 00110101  10011010 -> 1 --- ||||
00110110    1 -> 00110110  10011011 -> 0 -- |||||
00110111    1 -> 00110111  10011011 -> 1 - ||||||
                                          |||||||
                               LSB Byte: 11010101
```

When data is sent from the Macintosh to the DCD device, the LSB byte precedes the other seven; when data is sent from the DCD device to the Macintosh, the LSB byte follows the other seven.

## Handshaking and Data Transfer

The Macintosh initiates all data transfers; the DCD device transfers data only in response to the Macintosh.

### Macintosh to DCD Device

To initiate a data transfer, the Macintosh asserts HOST by transitioning from state 2 (the idle state) to state 3 and uses the RD line to sense the !HSHK signal from the DCD device.  When the DCD device indicates its readiness to transfer data by asserting !HSHK, the Macintosh transitions from state 3 to state 1 and begins to transfer data.

The Macintosh's data transfer begins with three bytes which are not encoded using the 7-to-8 encoding described above.  The first is an 0xAA "sync" byte.  The second indicates the number of 7-to-8-encoded groups contained in the transfer to follow, plus 0x80 (because the MSB must be set).  The third indicates the number of 7-to-8-encoded groups that the Macintosh expects to receive in response, plus 0x80.  These three bytes are followed by 7-to-8-encoded groups, the number of which was indicated by the second byte.

Sometimes, the Macintosh will need to suspend the data transfer midway in order to service an interrupt from a device such as the serial port controller or the quadrature mouse.  If the Macintosh needs to suspend the data transfer, it will assert HOFF by transitioning from state 1 to state 0.  It will continue to send data until the current 7-to-8-encoded group is finished.

When the Macintosh is ready to resume the data transfer, it will first transition the WR line to low if it was high at the end of the last bit transferred, then it will deassert HOFF by transitioning from state 0 to state 1.  The data transfer will resume first with an 0xAA "sync" byte (the bytes indicating the length of the transfer and response will not be repeated) and then the next 7-to-8-encoded group after the last one transferred before the suspension began, continuing with subsequent 7-to-8-encoded groups until the data transfer is complete or it needs to suspend the data transfer again.

When the data transfer is complete, the Macintosh will transition from state 1 to state 3 to sense the !HSHK signal from the DCD device again.  When the DCD device deasserts the !HSHK signal, the Macintosh will deassert HOST by transitioning from state 3 to state 2 and await the DCD device's response.

### DCD Device to Macintosh

To respond to a data transfer from the Macintosh, the DCD device will wait until HOST is deasserted, then assert !HSHK.  The Macintosh will respond by asserting HOST by transitioning to state 3 and then immediately to state 1 to read data from the DCD device.

The DCD device begins its data transfer with an 0xAA "sync" byte, following with the number of 7-to-8-encoded groups indicated by the third byte in the data transfer from the Macintosh to which it is responding.

If the Macintosh needs to suspend the data transfer, it will assert HOFF by transitioning from state 1 to state 0.  The DCD device must continue sending the current 7-to-8-encoded group until it is finished, then wait for HOFF to be deasserted.

When the Macintosh is ready to resume the data transfer, it will deassert HOFF by transitioning from state 0 to state 1.  The DCD device will resume by first sending an 0xAA "sync" byte followed by the next 7-to-8-encoded group after the last one transferred before the suspension began, continuing with subsequent 7-to-8-encoded groups until the data transfer is complete or the Macintosh needs to suspend the data transfer again.

When the number of 7-to-8-encoded groups indicated by the third byte in the Macintosh's data transfer has been received, the Macintosh will deassert HOST by transitioning to state 3, then immediately to state 2.

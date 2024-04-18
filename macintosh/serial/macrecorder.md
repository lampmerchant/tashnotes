# Farallon MacRecorder

## TxD± (from Macintosh)

The MacRecorder is presumed to use the TxD± pins for power, as it has no external power source and starts operating when the Macintosh's 26LS30 drivers are turned on.

## HSKi (to Macintosh)

The MacRecorder generates a clock signal over HSKi with a frequency of approximately 358 kHz, which is one tenth the NTSC color burst frequency of 315/88 (~3.58) MHz.

## RxD± (to Macintosh)

RxD idles high (RxD+ high, RxD- low).  When powered, approximately 200 ns after every sixteenth rising edge of the clock on HSKi, the MacRecorder begins to emit an 8-bit unsigned sample, resulting in a sample rate of approximately 22372 Hz.  The protocol uses the SCC's "isochronous" mode, with every change of RxD happening approximately 200 ns after the rising edge of the clock on HSKi.  Each sample begins with a start (low) bit, followed by eight bits of data beginning with the MSB and ending with the LSB (note that this is the opposite of the order usually associated with RS-422), followed by seven stop (high) bits.

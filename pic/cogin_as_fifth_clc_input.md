# COGIN as Fifth CLC Input

On the PIC16F1704 and 1708 (at least), it is possible to configure the Complementary Output Generator (COG)'s COGIN input pin to act as a fifth input pin to the Configurable Logic Cells (CLCs).

Once the COG is configured as shown below, the COG1A input to the CLCs can be used to represent the COGIN pin.  Note, however, that unlike the CLCINx pins, the COGIN pin will act as a *synchronized* input and the value of COG1A will not update until the next tick of the clock source.


## Setup

Set the following registers:
* `COG1RIS` ← `0x01`
* `COG1FIS` ← `0x01`
* `COG1STR` ← `0x01`

Set `COG1CON0` to one of the following values, depending on the desired clock source:
* `0x88` to use Fosc
   * This option will result in the fastest updates if the Fosc clock source is faster than the 16 MHz HFINTOSC.
   * During sleep, an internal Fosc source will shut down, meaning that the COG1A value will not be updated from COGIN.
* `0x90` to use HFINTOSC
   * This option will result in the COG1A value being updated from COGIN at 16 MHz, even during sleep.
   * If this option is selected, HFINTOSC will continue to run during sleep, which will affect current consumption.

All other COG1 registers remain at their power-on reset defaults:
* `COG1CON1` = `0x00`
* `COG1RSIM` = `0x00`
* `COG1FSIM` = `0x00`
* `COG1ASD0` = `0x00`
* `COG1ASD1` = `0x00`


## Explanation

The value of `COG1CON0` enables the COG in steered PWM mode.  This mode detects rising and falling edges on the configured inputs using the configured clock source and feeds them into an SR latch.  The rising and falling edge inputs are configured by the values of `COG1RIS` and `COG1FIS` to come from the COGIN pin.

The values of `COG1STR` and `COG1CON1` configure the COG1A output to be the SR latch's output with positive polarity, which can then be used by the CLCs.  The COG1B, COG1C, or COG1D outputs could be used in the same way, but as there is only one input pin, there is no point in configuring more than one output.

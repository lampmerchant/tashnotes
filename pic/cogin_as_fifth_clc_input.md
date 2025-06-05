# COGIN as Fifth CLC Input

On the PIC16F1704 and 1708 (at least), it is possible to configure the Complementary Output Generator (COG)'s COGIN input pin to act as a fifth input pin to the Configurable Logic Cells (CLCs).


## Setup

* `COG1RIS` ← 0x01
* `COG1FIS` ← 0x01
* `COG1STR` ← 0x01
* `COG1CON0` ← 0x88
* All other COG registers remain at their power-on reset defaults

Once the COG is configured this way, the COG1A input to the CLCs is updated once per Fosc tick to the value present at COGIN.


## Explanation

The value of `COG1CON0` enables the COG in steered PWM mode with Fosc as its clock source.  This mode detects rising and falling edges on the specified inputs using the specified clock and feeds them into an SR latch.  The rising and falling edge inputs are configured by the values of `COG1RIS` and `COG1FIS` to come from the COGIN pin.

The values of `COG1STR` and `COG1CON1` (with its power-on reset value of 0x00) configure the COG1A output to be the COG1A waveform with positive polarity, which can then be used by the CLCs.  The B, C, or D waveforms could be used in the same way, but only one input can be configured, so there's no point in configuring more than one.

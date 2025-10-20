# PIC UART Silicon Errata

## TXREG Write Ignored

When TXREG is written by two successive instructions, the first write is ignored.

```asm
	movlb	3		;UART registers in bank 3
	movlw	0x55		;Test pattern
	;fall through

Loop
	btfss	TXSTA,TRMT	;Spin until UART transmitter is idle
	bra	$-1		; "
	clrf	TXREG		;Send 0x00
	movwf	TXREG		;Send 0x55
	bra	Loop		;Loop
```

The preceding code should repeatedly transmit an 0x00 byte followed by an 0x55 byte, but instead results in the UART transmitting only 0x55.

| PIC Model         | Observed? |
| ----------------- | --------- |
| PIC12F1840        | No        |
| PIC16F1454        | Yes       |
| PIC16F1704 rev A4 | Yes       |
| PIC16F1704 rev A6 | Yes       |

This issue can be worked around by inserting a `nop` between the writes.


## Inconsistent Length of Start Bit

When a byte is written to an idle UART, the start bit may be anywhere from 1 to 2 bit times in width, causing serious issues with reception.

| PIC Model         | Observed? |
| ----------------- | --------- |
| PIC16F1704 rev A4 | Yes       |
| PIC16F1704 rev A6 | No        |

This issue can be worked around by clearing and then setting the TXEN bit before transmitting from idle.

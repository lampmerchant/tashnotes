# Precise Delay Subprogram

## Enhanced Mid-Range Core PICs

This version of the routine depends on the availability of the `brw` instruction.

```assembly
;Delay, including the subprogram call and return, exactly 10 + W cycles.
;Clobbers: W (0), C, DC, Z
Delay10PlusW
	addlw	-4		;Start by delaying 3 + ((W // 4) * 4) cycles,
	btfsc	STATUS,C	; leaving W at 252, 253, 254, or 255 depending
	bra	Delay10PlusW	; on if W % 4 was 0, 1, 2, or 3
	sublw	255		;This gives us the value of 3 - (W % 4)
	brw			;Delay 2 + (W % 4) cycles, which means we've
	nop			; delayed 3 + 1 + 2 + W cycles including this;
	nop			; add the call and return to get 10 + W
	nop			; "
	retlw	0		;Set W to 0 (Z is not affected) and return
```


## PIC10F320/322

Because this version of the routine depends on an 8-bit calculation of the program counter, if used on the PIC10F322, PCLATH must be set to the 256-byte block of memory in which the second half of the routine resides.

```assembly
;Delay, including the subprogram call and return, exactly 10 + W cycles.
;Clobbers: W (0), C, DC, Z
Delay10PlusW
	addlw	-4		;Start by delaying 3 + ((W // 4) * 4) cycles,
	btfsc	STATUS,C	; leaving W at 252, 253, 254, or 255 depending
	goto	Delay10PlusW	; on if W % 4 was 0, 1, 2, or 3
	sublw	$+1		;Delay 3 + (W % 4) cycles, which means we've
	movwf	PCL		; delayed 3 + 3 + W cycles including this; add
	nop			; the call and return to get 10 + W
	nop			; "
	nop			; "
	retlw	0		;Set W to 0 (Z is not affected) and return
```

# Double Dabble Algorithm

## Binary to BCD

```
;Convert the binary number in X1:0 into a packed BCD number in X4:X2.
	clrf	X4		;Clear the BCD space
	clrf	X3		; "
	clrf	X2		; "
	bsf	STATUS,C	;Use this 1 as an end sentinel
Loop	rlf	X0,F		;Shift
	rlf	X1,F		; "
	rlf	X2,F		; "
	rlf	X3,F		; "
	rlf	X4,F		; "
	rlf	X1,W		;Check if the sentinel bit reached the MSb of X1
	iorwf	X0,W		; and finish if so
	btfsc	STATUS,Z	; "
	return			; "
	movlw	0x33		;Set MSb of each nibble if they're >= 5 (we
	addwf	X2,F		; don't need to do this for the highest nibble
	addwf	X3,F		; since it'll never be more than 3)
	movlw	0x03		;If the MSb of either byte's low nibble is
	btfss	X2,3		; clear, we shouldn't have added 3 to it, so
	subwf	X2,F		; subtract
	btfss	X3,3		; "
	subwf	X3,F		; "
	movlw	0x30		;If the MSb of either byte's high nibble is
	btfss	X2,7		; clear, we shouldn't have added 3 to it, so
	subwf	X2,F		; subtract
	btfss	X3,7		; "
	subwf	X3,F		; "
	bcf	STATUS,C	;Clear carry so we don't rotate another 1 in
	bra	Loop		;Loop
```


## BCD to Binary

```
;Convert the packed BCD number in X3:X2 into a binary number in X1:X0.
	clrf	X1		;Clear the binary space
	clrf	X0		; "
	bsf	X1,7		;Use this 1 as an end sentinel
Loop	lsrf	X3,F		;Shift
	rrf	X2,F		; "
	rrf	X1,F		; "
	rrf	X0,F		; "
	btfsc	STATUS,C	;Check if the sentinel bit rolled off the end of
	return			; the binary space and finish if it did
	movlw	0x03		;If the MSb of either byte's low nibble is set,
	btfsc	X2,3		; subtract 3 from it
	subwf	X2,F		; "
	btfsc	X3,3		; "
	subwf	X3,F		; "
	movlw	0x30		;If the low byte's high nibble's MSb is set,
	btfsc	X2,7		; subtract 3 from it (that of the high byte
	subwf	X2,F		; cannot be set because we lsrf'd it)
	bra	Loop		;Loop
```

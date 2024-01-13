# Macintosh Serial Port Pinouts

```
DE-9 Female (128K,                        DE-9 Male (PC
512K, 512Ke):        Mini-DIN-8 Female:   RS-232 adapter):
  -----------          .---_---.           -----------
 \ 5 4 3 2 1 /        / 8  7  6 \         \ 1 2 3 4 5 /
  \ 9 8 7 6 /        ( 5 9* 4  3 )         \ 6 7 8 9 /
    -------           \  2   1  /            -------
                       '-------'
```

| Signal   | Dir | DE-9F | Mini-DIN-8F | DE-9M |
| -------- | --- | ----- | ----------- | ----- |
| Ground   | —   | 1, 3  | 4           | 5     |
| TxD+     | →   | 4     | 6           | —     |
| TxD-     | →   | 5     | 3           | 3     |
| RxD+     | ←   | 8     | 8           | 5     |
| RxD-     | ←   | 9     | 5           | 2     |
| HSKi/Clk | ←   | 7     | 2           | 8     |
| HSKo     | →   | —     | 1           | 4, 7  |
| GPi      | ←   | —     | 7**         | —     |
| +5V      | →   | 2     | 9*          | —     |
| +12V     | →   | 6     | —           | —     |

Direction is DTE (Macintosh) relative to DCE.

\* GeoPort only

\** Modem port only

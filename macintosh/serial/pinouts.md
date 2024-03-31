# Macintosh Serial Port Pinouts

## Connector Diagrams

```
DE-9 Female:     MiniDIN-8 Female:   DE-9 Male:      MiniDIN-8 Male:
  -----------      .---_---.          -----------      .---_---.  
 \ 5 4 3 2 1 /    / 8  7  6 \        \ 1 2 3 4 5 /    / 6  7  8 \ 
  \ 9 8 7 6 /    ( 5 9* 4  3 )        \ 6 7 8 9 /    ( 3  4 9* 5 )
    -------       \_ 2   1 _/           -------       \_ 1   2 _/
                    '-----'                             '-----'
```


## Signals

| Signal   | Dir | DE-9F (Mac 128k/512k/512ke) | MiniDIN-8F |
| -------- | --- | --------------------------- | ---------- |
| Ground   | —   | 1, 3                        | 4          |
| TxD+     | →   | 4                           | 6          |
| TxD-     | →   | 5                           | 3          |
| RxD+     | ←   | 8                           | 8          |
| RxD-     | ←   | 9                           | 5          |
| HSKi/Clk | ←   | 7                           | 2          |
| HSKo     | →   | —                           | 1          |
| GPi      | ←   | —                           | 7**        |
| +5V      | →   | 2                           | 9*         |
| +12V     | →   | 6                           | —          |

Direction is DTE (Macintosh) relative to DCE.

\* GeoPort only

\** Modem port only


## Cables

### From DE-9F (Mac 128k/512k/512ke)

| DE-9M | Dir | DE-9M (to Mac) | MiniDIN-8M | DE-9M (to DCE) | DE-9F (to PC/DTE) |
| ----- | --- | -------------- | ---------- | -------------- | ----------------- |
| 1, 3  | —   | 1, 3           | 4          | 5              | 5                 |
| 4     | →   | 8              | 8          | —              | —                 |
| 5     | →   | 9              | 5          | 3              | 2                 |
| 7     | ←   | —              | 1          | 8              | 7                 |
| 8     | ←   | 4              | 6          | 5              | 5                 |
| 9     | ←   | 5              | 3          | 2              | 3                 |


### From MiniDIN-8F

| MiniDIN-8M | Dir | DE-9M (to Mac) | MiniDIN-8M (Crossover) | DE-9M (to DCE) | DE-9F (to PC/DTE) |
| ---------- | --- | -------------- | ---------------------- | -------------- | ----------------- |
| 1          | →   | 7              | 2                      | 7              | 8                 |
| 2          | ←   | —              | 1                      | 8              | 7                 |
| 3          | →   | 9              | 5                      | 3              | 2                 |
| 4          | —   | 1, 3           | 4                      | 5              | 5                 |
| 5          | ←   | 5              | 3                      | 2              | 3                 |
| 6          | →   | 8              | 8                      | —              | —                 |
| 7          | ↔*  | —              | 7                      | —              | —                 |
| 8          | ←   | 4              | 6                      | 5              | 5                 |

\* GPi signal is input on Macintosh but output on device


### From DE-9F (DCE)

| DE-9M | Dir | DE-9M (to Mac) | MiniDIN-8M | DE-9F (to PC/DTE) |
| ----- | --- | -------------- | ---------- | ----------------- |
| 2     | →   | 9              | 5          | 3                 |
| 3     | ←   | 5              | 3          | 2                 |
| 4     | ←   | —              | —          | 6                 |
| 5     | —   | 1, 3, 8        | 4, 8       | 5                 |
| 6     | →   | —              | —          | 4                 |
| 7     | ←   | —              | 1          | 8                 |
| 8     | →   | 7              | 2          | 7                 |


### From DE-9M (PC/DTE)

| DE-9F | Dir | DE-9M (to Mac) | MiniDIN-8M | DE-9M (to DCE) |
| ----- | --- | -------------- | ---------- | -------------- |
| 2     | ←   | 5              | 3          | 3              |
| 3     | →   | 9              | 5          | 2              |
| 4     | →   | —              | —          | 6              |
| 5     | —   | 1, 3, 8        | 4, 8       | 5              |
| 6     | ←   | —              | —          | 4              |
| 7     | →   | 7              | 2          | 8              |
| 8     | ←   | —              | 1          | 7              |

All headers are mapped such that the board is face-up with the ATX panel in the upper left.


KGPE-D16 Headers - Not Stuffed
==============================


B2-E2
-----

(from left to right)


### P0_J3

|                         |
| ----------------------- |
| GND                     |
| GND                     |
| ACE_P0_DIMM_VREF_SUS_DQ |
| ACE_P0_DIMM_VREF_SUS_DQ |


### P0_J2

|                   |
| ----------------- |
| GND               |
| GND               |
| ACE_P0_M_VREF_SUS |
| ACE_P0_M_VREF_SUS |


### P0_J1

|                         |
| ----------------------- |
| GND                     |
| GND                     |
| ACE_P0_DIMM_VREF_SUS_CA |
| ACE_P0_DIMM_VREF_SUS_CA |


### P1_J1

|                         |
| ----------------------- |
| ACE_P1_DIMM_VREF_SUS_CA |
| ACE_P1_DIMM_VREF_SUS_CA |
| GND                     |
| GND                     |


### P1_J2

|                   |
| ----------------- |
| ACE_P1_M_VREF_SUS |
| ACE_P1_M_VREF_SUS |
| GND               |
| GND               |


### P1_J3

|                         |
| ----------------------- |
| ACE_P1_DIMM_VREF_SUS_DQ |
| ACE_P1_DIMM_VREF_SUS_DQ |
| GND                     |
| GND                     |


A4
--

### AST_JTAG1

|          |           |         |         |         |          |         |           |     |     |
| -------- | --------- | ------- | ------- | ------- | -------- | ------- | --------- | --- | --- |
| +3V3_AUX | GND       | GND     | GND     | GND     | GND      | GND     | GND       | GND | GND |
| +3V3_AUX | AST_NTRST | AST_TDI | AST_TMS | AST_TCK | AST_RTCK | AST_TDO | AST_SRST# | nc  | nc  |


B4
--

### AST_UART

|       |            |            |     |
| ----- | ---------- | ---------- | --- |
| +5VSB | AST_TXD2_R | AST_RXD2_R | GND |


### ACE_CLOCK_HEADER1

|     |        |     |
| --- | ------ | --- |
| GND |        | GND |
|     | CLX_X1 |     |
| GND |        | GND |

SMA connector - CLX_X1 connects to pin 24 on 932S890CKLF


C4
--

X


D4
--

### NB_DEBUG_HEADER1

|                 |                 |                 |                 |     |     |
| --------------- | --------------- | --------------- | --------------- | --- | --- |
| RD890_DFT_GPIO2 | RD890_DFT_GPIO4 | RD890_DBG_GPIO1 | RD890_DBG_GPIO3 | GND | nc  |
| RD890_DFT_GPIO1 | RD890_DFT_GPIO3 | RD890_DBG_GPIO0 | RD890_DBG_GPIO2 | GND | key |


### TEST_CON1

|           |           |           |           |
| --------- | --------- | --------- | --------- |
| P0_TEST36 | GND       | GND       | P0_TEST11 |
| P0_TEST14 | P0_TEST15 | P0_TEST16 | P0_TEST17 |


### NB_JTAG_HEADER1

|            |                    |            |                     |                 |                 |     |
| ---------- | ------------------ | ---------- | ------------------- | --------------- | --------------- | --- |
| NB_I2C7SCL | PCIE_RST_GPIO3_TMS | NB_I2C7SDA | RD890_PWM_GPIO6_TDO | RD890_TESTMODE* | RD890_TESTMODE* | nc  |
| GND        | GND                | GND        | GND                 | +1V8            | pullup to 1V8?  | key |

* connected to RD890_TESTMODE net through two different resistors


### TEST_CON2

|           |           |           |           |
| --------- | --------- | --------- | --------- |
| P1_TEST36 | GND       | GND       | P1_TEST11 |
| P1_TEST14 | P1_TEST15 | P1_TEST16 | P1_TEST17 |


E4
--

### HDT_CON1

|          |            |
| -------- | ---------- |
| P0_VDDIO | PX_TCK     |
| GND      | PX_TMS     |
| GND      | P0_TDI     |
| GND      | P1_TDO     |
| PX_TRST# | HDT_PWROK  |
| pulldown | HDT_RESET# |
| pulldown | P0_DBRDY   |
| P1_DBRDY | HDR_DBREQ# |
| GND      | HDR_TEST19 |
| P0_VDDIO | HDR_TEST18 |


A5
--

X


B5
--

### IBTN_SEL

|             |          |           |
| ----------- | -------- | --------- |
| PIKE_IBTNIN | IBTN_SEL | SB_ITBNIN |


C5
--

X


D5
--

### HDLED1

|    |           |           |    |
| -- | --------- | --------- | -- |
| nc | HDLED_SD5 | HDLED_SD5 | nc |


E5
--

### HSTAT1

|     |              |              |              |              |     |     |              |              |
| --- | ------------ | ------------ | ------------ | ------------ | --- | --- | ------------ | ------------ |
| GND | SB_STAT_HDD4 | SB_STAT_HDD3 | SB_STAT_HDD2 | SB_STAT_HDD1 | key | GND | SB_STAT_HDD6 | SB_STAT_HDD5 |


### SJ8

|                  |              |                  |               |              |          |     |
| ---------------- | ------------ | ---------------- | ------------- | ------------ | -------- | --- |
| USB_OC_2_3_L_TCK | SB_TEST1_TMS | USB_OC_0_1_L_TDI | SB_IR_RX1_TDO | SB_JTAG_RST# | SB_TEST2 | nc  |
| GND              | GND          | GND              | GND           | +3V3_AUX     | pulldown | key |

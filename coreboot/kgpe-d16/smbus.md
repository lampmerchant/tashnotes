SMBus Connections on KGPE-D16
=============================

* SP5100 [southbridge] pin W18 (SDA) / AA18 (SCL) - SDA0/SCL0, southbridge's primary SMBus controller
  * 'EPU' chip near first CPU pin 31 (SDA) / 30 (SCL)
  * 'EPU' chip near second CPU pin 31 (SDA) / 30 (SCL)
* SP5100 [southbridge] pin K2 (SDA) / K1 (SCL) - SDA1/SCL1, southbridge's secondary SMBus controller
  * W83795G [system monitor] pin 34 (SDA) / 33 (SCL)
  * AST2050 [BMC/video] C14 (SDA) / D14 (SCL) - SDA2/SCL2
  * TLV3125 pin 3 (SDA) / 6 (SCL) ... which connect to 4 (SDA) / 7 (SCL) if net I2CMUX_ENABLE# is asserted
    * 932S890CKLF [clock generator] pin 23 (SDA) / 22 (SCL)
    * SR5690 [northbridge] pin C20 (SDA) / B20 (SCL)
    * NB_JTAG_HEADER1 pin 6 (SDA) / 2 (SCL)
    * 74HC4052 [dual 4:1 mux] pin 3 (SDA) / 13 (SCL)
      * ... which connect to pin 1 (SDA) / 12 (SCL) if S1:S0 (pin 10:pin 9) == 0
        * AUX_PANEL1 pin 10 (SDA) / 4 (SCL)
        * TLV3125 pin 14 (SDA) / 11 (SCL) ... which connect to 13 (SDA) / 10 (SCL) if net I2CMUX_ENABLE# is asserted
          * TPM pin 14 (SDA) / 13 (SCL)
          * PCI slot pin A41 (SDA) / A40 (SCL)
          * PIKE1 and all PCIe slots pin B6 (SDA) / B5 (SCL)
      * ... which connect to pin 5 (SDA) / 14 (SCL) if S1:S0 (pin 10:pin 9) == 1
        * unconnected
      * ... which connect to pin 2 (SDA) / 15 (SCL) if S1:S0 (pin 10:pin 9) == 2
        * SDRAM slots for first CPU pin 238 (SDA) / 118 (SCL)
          * SPD flash chips on DIMMs
      * ... which connect to pin 4 (SDA) / 11 (SCL) if S1:S0 (pin 10:pin 9) == 3
        * SDRAM slots for second CPU pin 238 (SDA) / 118 (SCL)
          * SPD flash chips on DIMMs
* SP5100 [southbridge] pin F19 (SDA) / D21 (SCL) - SDA2/SCL2, southbridge's primary SMBus controller
  * AST2050 [BMC/video] pin A14 (SDA) / B14 (SCL) - SDA3/SCL3
* SP5100 [southbridge] pin E21 (SDA) / E20 (SCL) - SDA3/SCL3, southbridge's primary SMBus controller
  * unconnected
* AST2050 [BMC/video] pin A15 (SDA) / B15 (SCL) - SDA1/SCL1
  * PSUSMB1 pin 2 (SDA) / 1 (SCL)
* AST2050 [BMC/video] pin C13 (SDA) / D13 (SCL) - SDA4/SCL4
  * W83795G pin 30 (SDA) / 29 (SCL) - SB-TSI interface
  * first CPU pin BU5 (SDA) / BU4 (SCL) - SB-TSI interface
  * second CPU pin BU5 (SDA) / BU4 (SCL) - SB-TSI interface
* AST2050 [BMC/video] pin A13 (SDA) / B13 (SCL) - SDA5/SCL5
  * AT24C08C [8Kb EEPROM] pin 5 (SDA) / 6 (SCL)
  * upper W83601G [I/O expander] pin 2 (SDA) / 1 (SCL)
    * DIMM error LEDs (TODO other functions?)
  * lower W83601G [I/O expander] pin 2 (SDA) / 1 (SCL)
    * DIMM error LEDs (TODO other functions?)
* AST2050 [BMC/video] pin C12 (SDA) / D12 (SCL) - SDA6/SCL6
  * pins appear not to be used for SMBus
* AST2050 [BMC/video] pin A12 (SDA) / B12 (SCL) - SDA7/SCL7
  * pins appear not to be used for SMBus

74HC4052 [mux] S1:S0 (pin 10:pin 9) is controlled by GPIO60:GPIO59 on SP5100 [southbridge] or GPIOF5:GPIOF4 on AST2050 [BMC/video].
The AST2050 controls the mux if the ASMB4-iKVM is present (and thus pin 7 [BMC_PRESENT#] of the BMC_FW1 header is pulled low),
otherwise the SP5100 does.

TODO net I2CMUX_ENABLE# is driven by pin 12 on LVC14A [hex inverting schmitt trigger] ... which is driven by the input on pin 13 ...
which is net SYS_PWRGD, but what drives it?

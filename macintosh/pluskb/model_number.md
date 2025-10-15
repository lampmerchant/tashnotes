# Model Number Command Responses

This document contains known responses to the Model Number command for Macintosh Plus/512k/128k keyboards.


## Responses

| Keyboard                                                 | Response | Tested By                   |
| -------------------------------------------------------- | -------- | --------------------------- |
| M0110 (US) Keyboard, No Keypad                           | 0x03     | Tashtari                    |
| M0110 (US) Keyboard, M0120 Keypad                        | 0x13     | Tashtari                    |
| M0110 (US) Keyboard, Dayna MacCharlie Keyboard Extension | 0x13     | KennyPowers@TinkerDifferent |
| M0110A (US) Keyboard                                     | 0x0B     | KennyPowers@TinkerDifferent |


## Response Format

| Bit 7                            | Bits 6-4           | Bits 3-1              | Bit 0    |
| -------------------------------- | ------------------ | --------------------- | -------- |
| 1 if additional device connected | Next device number | Keyboard model number | Always 1 |

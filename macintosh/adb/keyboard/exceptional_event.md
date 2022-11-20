# Exceptional Event Handling in ADB Keyboards

## Documentation

Little is documented of the "Exceptional Event" bit (bit 14) in ADB except that it is specified to be set to 1 if not used[^1] and that it is set to 1 if the reset/power key has been pressed on the ADB keyboard and 0 if no exceptional event has occurred[^2].

[^1]: Apple Guide to the Macintosh Family Hardware, Second Edition, page 322
[^2]: Apple IIgs Hardware Reference, page 144

## Observations

On some models of ADB keyboard, the Exceptional Event bit is set to 1 by default and only set to 0 _while the reset/power key is held down_; it does not latch the event, so if a Talk 3 command is not issued while the key is being held, the Macintosh will not see the exceptional event.

| Model Name                 | Model Number | Clears Exceptional Event Bit  |
| -------------------------- | ------------ | ----------------------------- |
| Apple Keyboard             | M0116        | While reset/power key is held |
| Apple Extended Keyboard    | M0115        | While reset/power key is held |
| Apple Extended Keyboard II | M3501        | While reset/power key is held |
| Apple Keyboard II          | M0487        | While reset/power key is held |
| AppleDesign Keyboard       | M2980        | Never                         |

"Thermal Cruise" Mode of W83795G
================================

The Nuvoton chip advertises a "Thermal Cruise" mode which promises to enable the chip to handle thermal management autonomously,
however, it requires a 1:1 correspondence between a tachometer input and a PWM output, which the D16 doesn't provide.  The stock
firmware likely uses this mode, but exactly how it sets it up is not established.

TODO dig into this more

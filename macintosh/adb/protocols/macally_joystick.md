# MacAlly JoyStick

The JoyStick presents itself on the ADB at address 0x5 with handler 0x03.  The handler cannot be changed.

## Talk 0

### Mouse Mode

In mouse mode, the JoyStick imitates the standard Apple mouse protocol, attempting to correspond the center of the joystick to the center of the screen.  Both joystick buttons actuate the main mouse button.  This mode does not work as intended because the JoyStick's initial device address is 0x5 rather than 0x3.

Why the JoyStick uses address 0x5 instead of 0x3 is unknown.  It is possible, though only speculation, that Apple began allocating handler IDs on address 0x5 after all handler IDs on address 0x3 were used and that, at the time of the handler ID on address 0x5 being assigned, firmware had already been written with the assumption that a handler ID on address 0x3 would be assigned.

### Joystick Mode

In joystick mode, Talk 0 gives joystick position information and button state as a five-byte string:

| Byte | Description           |
| ---- | --------------------- |
| 1st  | Y position, low byte  |
| 2nd  | X position, low byte  |
| 3rd  | Y position, high byte |
| 4th  | X position, high byte |
| 5th  | Button state          |

X and Y position are given as absolute numbers between 0x000 (up and left) and 0x3FF (down and right).

Button state is a bit field defined as follows, with 0 meaning down and 1 meaning up:

| Bit | Description          |
| --- | -------------------- |
| 7-2 | Always 1             |
| 1   | Top trigger button   |
| 0   | Front trigger button |

## Talk/Listen 1

Register 1 indicates and sets the mode for Talk 0.

At startup/after reset, register 1 reads as `0x55 0x55`, which indicates mouse mode.  In mouse mode, writing `0x55 0x55` to register 1 changes its contents to `0xAA 0xAA`, which indicates joystick mode.  In joystick mode, writing `0xAA 0xAA` to register 1 changes its contents back to `0x55 0x55`.

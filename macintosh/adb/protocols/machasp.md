# MacHASP

The MacHASP is an ADB license dongle.  It presents itself on the ADB at address 0x7 with handler ID 0x67.


## Communication Overview

The MacHASP dongle communicates with the host in encrypted 8-byte buffers.  Communication is always initiated by the host, which encrypts a command buffer and sends it with a Listen 1 command, then retreives the response with a Talk 1 command and decrypts it.  See [Algorithms](#algorithms) below for details of the algorithm.


## Commands and Responses

The second byte of the command buffer is common to all commands:

| Bit(s) | Description       |
| ------ | ----------------- |
| 7-6    | Transaction ID    |
| 5-4    | Ignored by dongle |
| 3-0    | Always 5          |

Likewise, the second byte of the response buffer is common to all commands:

| Bit(s) | Description                                      |
| ------ | ------------------------------------------------ |
| 7-6    | Transaction ID (same value as in command buffer) |
| 5-4    | Ignored by host                                  |
| 3-0    | Always 5                                         |


### Get Code

Command:

| Byte(s) | Description                                   |
| ------- | --------------------------------------------- |
| 0       | MSb must be set; other bits ignored by dongle |
| 1       | Common, see above                             |
| 2-4     | Seed 1                                        |
| 5-7     | Seed 2                                        |

Response:

| Byte(s) | Description       |
| ------- | ----------------- |
| 0       | Always 0          |
| 1       | Common, see above |
| 2-4     | Result 1          |
| 5-7     | Result 2          |

Result 1 is the result of feeding seed 1 through an encryption algorithm using a 24-bit key that is unique to each developer.  Result 2 is the result of feeding seed 2 through an encryption algorithm using a 24-bit key that is unique to each dongle.  The algorithm used is different to the communication buffer algorithm (see [Algorithms](#algorithms) below).


### Other Commands

Command buffers for commands other than Get Code share a common format:

| Byte(s) | Description                                                                   |
| ------- | ----------------------------------------------------------------------------- |
| 0       | MSb must be clear; other bits ignored by dongle                               |
| 1       | Common, see above                                                             |
| 2       | Bits 7-6 are command-specific parameters; bits 5-0 contain the command number |
| 3-7     | Command-specific parameters                                                   |

Likewise, response buffers share a common format:

| Byte(s) | Description                    |
| ------- | ------------------------------ |
| 0       | Status code                    |
| 1       | Common, see above              |
| 2-3     | Command-specific return values |
| 4-7     | Ignored by host                |

Known returned status codes:

| Code | Meaning                     |
| ---- | --------------------------- |
| 0    | Success or wrong password   |
| 2    | Invalid service number      |
| 5    | Invalid EEPROM byte address |

Parameters not specified below are ignored by dongle.  Return values not specified below are ignored by host.


#### Is Hasp Command

Command Number: 1

Test communications with the MacHASP dongle.  Always returns 0 status code.


#### Get Type Command

Command Number: 3

Return information about the MacHASP dongle.  Always returns 0 status code.

Byte 2 of response buffer contains the following information:

| Bit(s) | Description                                         |
| ------ | --------------------------------------------------- |
| 5-3    | Number of net stations (see below)                  |
| 2-1    | Size of EEPROM (memo) memory if present (see below) |
| 0      | Set if dongle has EEPROM (memo) memory              |

Net stations values:

| Value | Meaning   |
| ----- | --------- |
| 0     | None      |
| 1     | 5         |
| 2     | 10        |
| 3     | 20        |
| 4     | 50        |
| 5     | 100       |
| 6     | 256       |
| 7     | Unlimited |

EEPROM size values:

| Value | Meaning    |
| ----- | ---------- |
| 0     | 128 bytes  |
| 1     | 256 bytes  |
| 2     | 512 bytes  |
| 3     | 1024 bytes |


#### Read Memo Command

Command Number: 4

Read a byte of EEPROM (memo) memory.

Parameters:

| Byte(s)      | Description                                                              |
| ------------ | ------------------------------------------------------------------------ |
| 2 (bits 7-6) | Bits 9-8 of EEPROM byte address                                          |
| 3-5          | Hash of read password (see [Passwords](#passwords) below), little-endian |
| 6            | Bits 7-0 of EEPROM byte address                                          |

Byte value at the given address is returned in byte 2 of response.

Known returned status code values are 0 and 5.


#### Write Memo Command

Command Number: 5

Write a byte of EEPROM (memo) memory.

Parameters:

| Byte(s)      | Description                                                                             |
| ------------ | --------------------------------------------------------------------------------------- |
| 2 (bits 7-6) | Bits 9-8 of EEPROM byte address                                                         |
| 3-5          | Hash of write or privileged password (see [Passwords](#passwords) below), little-endian |
| 6            | Bits 7-0 of EEPROM byte address                                                         |

The privileged password hash must be given to write privileged memory; the write password hash must be given to write user memory (see [Memory](#memory) below).  The privileged password hash cannot be used to write user memory and the write password hash cannot be used to write privileged memory.

Known returned status code values are 0 and 5.


#### Read RAM Word 0 Command

Command Number: 9

Read word 0 of RAM memory.  Always returns 0 status code, even if password hash is wrong.

Parameters:

| Byte(s) | Description                                                              |
| ------- | ------------------------------------------------------------------------ |
| 3-5     | Hash of read password (see [Passwords](#passwords) below), little-endian |

Return values:

| Byte(s) | Description                            |
| ------- | -------------------------------------- |
| 2-3     | Value read from RAM memory, big-endian |


#### Read RAM Word 1 Command

Command Number: 10

Read word 1 of RAM memory.  Always returns 0 status code, even if password hash is wrong.

Parameters:

| Byte(s) | Description                                                              |
| ------- | ------------------------------------------------------------------------ |
| 3-5     | Hash of read password (see [Passwords](#passwords) below), little-endian |

Return values:

| Byte(s) | Description                            |
| ------- | -------------------------------------- |
| 2-3     | Value read from RAM memory, big-endian |


#### Write RAM Word 0 and 1 Command

Command Number: 11

Write words 0 and 1 of RAM memory.  Always returns 0 status code, even if password hash is wrong.

Parameters:

| Byte(s) | Description                                                              |
| ------- | ------------------------------------------------------------------------ |
| 3-5     | Hash of read password (see [Passwords](#passwords) below), little-endian |
| 6-7     | Value to write to RAM memory, big-endian                                 |


#### Write RAM Word 1 Command

Command Number: 12

Write word 1 of RAM memory.  Always returns 0 status code, even if password hash is wrong.

Parameters:

| Byte(s) | Description                                                              |
| ------- | ------------------------------------------------------------------------ |
| 3-5     | Hash of read password (see [Passwords](#passwords) below), little-endian |
| 6-7     | Value to write to RAM memory, big-endian                                 |


## Memory

All MacHASP dongles have two words (32 bits) of RAM memory that can be read and written using the read password as described in [Commands and Responses](#commands-and-responses) above.

Some MacHASP dongles also have EEPROM (memo) memory:

| Dongle Type | Has EEPROM | EEPROM Size |
| ----------- | ---------- | ----------- |
| MacHASP-S   | No         | —           |
| MacHASP-M   | Yes        | 128 bytes   |
| MacHASP-M8  | Yes        | 1024 bytes  |

MacHASP dongles that have EEPROM (memo) memory organize it as such:

| Byte Addresses | Description       |
| -------------- | ----------------- |
| 0-1            | Net max stations  |
| 28-37          | Privileged memory |
| 38 and above   | User memory       |


## Passwords

All MacHASP dongles have three passwords (unique to a developer) that are given in the form of two 32-bit words.  This form is only used by the code provided to developers, however - they are always communicated to the dongle in the form of 24-bit "hashes" (see [Algorithms](#algorithms) below for details of the "hash" algorithm).

| Password   | Usage                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------- |
| Read       | Used to read/write RAM memory and read EEPROM (memo) memory.  Typically included in software. |
| Write      | Used to write user EEPROM (memo) memory.  Sometimes included in software.                     |
| Privileged | Used to write privileged EEPROM (memo) memory.  Never included in software.                   |


## Algorithms

In the algorithms described below, the `swap_nibbles` function swaps the nibbles of a byte (i.e. `11110000` becomes `00001111`).


### Host Communication Buffer Algorithm

This algorithm is run by the host on buffers before sending them to the dongle and on buffers after receiving them from the dongle.

```python
for i in range(10):
  buf[7] = (swap_nibbles(buf[7] ^ 0x00) - buf[6]) & 0xFF
  buf[6] = (swap_nibbles(buf[6] ^ 0x91) - buf[3]) & 0xFF
  buf[5] = (swap_nibbles(buf[5] ^ 0x19) - buf[4]) & 0xFF
  buf[4] = (swap_nibbles(buf[4] ^ 0x04) - buf[1]) & 0xFF
  buf[3] = (swap_nibbles(buf[3] ^ 0x16) - buf[2]) & 0xFF
  buf[2] = (swap_nibbles(buf[2] ^ 0x35) - buf[0]) & 0xFF
  buf[1] = (swap_nibbles(buf[1] ^ 0x22) - buf[7]) & 0xFF
  buf[0] = (swap_nibbles(buf[0] ^ 0xFF) - buf[5]) & 0xFF
```


### Dongle Communication Buffer Algorithm

This algorithm is run by the dongle on buffers before sending them to the host and on buffers after receiving them from the host.  That is, it is the reverse of the algorithm above.

```python
for i in range(10):
  buf[0] = swap_nibbles((buf[0] + buf[5]) & 0xFF) ^ 0xFF
  buf[1] = swap_nibbles((buf[1] + buf[7]) & 0xFF) ^ 0x22
  buf[2] = swap_nibbles((buf[2] + buf[0]) & 0xFF) ^ 0x35
  buf[3] = swap_nibbles((buf[3] + buf[2]) & 0xFF) ^ 0x16
  buf[4] = swap_nibbles((buf[4] + buf[1]) & 0xFF) ^ 0x04
  buf[5] = swap_nibbles((buf[5] + buf[4]) & 0xFF) ^ 0x19
  buf[6] = swap_nibbles((buf[6] + buf[3]) & 0xFF) ^ 0x91
  buf[7] = swap_nibbles((buf[7] + buf[6]) & 0xFF) ^ 0x00
```


### Password "Hash" Algorithm

This algorithm is used to convert a password (consisting of two 32-bit words) into a 24-bit "hash".  The word "hash" is quoted because the algorithm is trivially reversible.

```python
for i in range(10):
  buf[7] = (swap_nibbles(buf[7] ^ 0x6E) - buf[0]) & 0xFF
  buf[6] = (swap_nibbles(buf[6] ^ 0x69) - buf[7]) & 0xFF
  buf[5] = (swap_nibbles(buf[5] ^ 0x64) - buf[2]) & 0xFF
  buf[4] = (swap_nibbles(buf[4] ^ 0x61) - buf[5]) & 0xFF
  buf[3] = (swap_nibbles(buf[3] ^ 0x6C) - buf[1]) & 0xFF
  buf[2] = (swap_nibbles(buf[2] ^ 0x6C) - buf[6]) & 0xFF
  buf[1] = (swap_nibbles(buf[1] ^ 0x41) - buf[4]) & 0xFF
  buf[0] = (swap_nibbles(buf[0] ^ 0x02) - buf[3]) & 0xFF
```

As a precondition, `buf` should contain the following:

| Byte(s) | Contents                            |
| ------- | ----------------------------------- |
| 0-3     | First password word, little-endian  |
| 4-7     | Second password word, little-endian |

After running the algorithm, the password "hash" is contained in bytes 5-7, little-endian.


### Password "Unhash" Algorithm

This algorithm is used to convert a 24-bit password "hash" into a password (consisting of two 32-bit words).  That is, it is the reverse of the algorithm above.

```python
for i in range(10):
  buf[0] = swap_nibbles((buf[0] + buf[3]) & 0xFF) ^ 0x02
  buf[1] = swap_nibbles((buf[1] + buf[4]) & 0xFF) ^ 0x41
  buf[2] = swap_nibbles((buf[2] + buf[6]) & 0xFF) ^ 0x6C
  buf[3] = swap_nibbles((buf[3] + buf[1]) & 0xFF) ^ 0x6C
  buf[4] = swap_nibbles((buf[4] + buf[5]) & 0xFF) ^ 0x61
  buf[5] = swap_nibbles((buf[5] + buf[2]) & 0xFF) ^ 0x64
  buf[6] = swap_nibbles((buf[6] + buf[7]) & 0xFF) ^ 0x69
  buf[7] = swap_nibbles((buf[7] + buf[0]) & 0xFF) ^ 0x6E
```

As a precondition, `buf` should contain the following:

| Byte(s) | Contents                       |
| ------- | ------------------------------ |
| 0-4     | Always 0x59*                   |
| 5-7     | Password "hash", little-endian |

\* These bytes can be anything and result in a password that can be passed through the "hash" algorithm to produce the same value, but known passwords always have them set to 0x59.

After running the algorithm, `buf` will contain the following:

| Byte(s) | Contents                            |
| ------- | ----------------------------------- |
| 0-3     | First password word, little-endian  |
| 4-7     | Second password word, little-endian |


### Get Key Algorithm

(withheld by request)


## Credits

This writeup is made possible almost in its entirety by extensive reverse engineering by Windoze.  Thank you!

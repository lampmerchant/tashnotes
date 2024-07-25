# Timing of PS/2 Host to Device Communications

```
   ___            _   _   _   _   _   _   _   _   _   _   ___   __
      |          | | | | | | | | | | | | | | | | | | | | |   | | 
Clock |          | | | | | | | | | | | | | | | | | | | | |   | | 
      |__________| |_| |_| |_| |_| |_| |_| |_| |_| |_| |_|   |_| 
   ____________      ___ ___ ___ ___ ___ ___ ___ ___ ___ ___     _
               |    |   |   |   |   |   |   |   |   |   |   |   | 
 Data          |Strt| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |Pty|Stp|Ack| 
               |____|___|___|___|___|___|___|___|___|___|   |___| 
      ^        ^ ^ ^^^ ^^^ ^^^ ^^^ ^^^ ^^^ ^^^ ^^^ ^^^ ^^^  ^^ ^^
      A        B C DEF GHI GHI GHI GHI GHI GHI JKL MNO PQR  ST UV
```

- **A** - Host pulls clock low and waits 100+ us
- **B** - Host pulls data low, serving as request to send and start bit (0)
- **C** - Host releases clock
   - Note: **B** and **C** may happen at the same time
- **D** - Device pulls clock low, signalling host to put LSb on data
- **E** - Host puts LSb of byte on data
- **F** - Device releases clock, clocking in LSb
- **G** - Device pulls clock low, signalling host to put next bit on data
- **H** - Host puts next bit on data
- **I** - Device releases clock, clocking in next bit
- **J** - Device pulls clock low, signalling host to put MSb on data
- **K** - Host puts MSb on data
- **L** - Device releases clock, clocking in MSb
- **M** - Device pulls clock low, signalling host to put odd parity bit on data
- **N** - Host puts odd parity bit on data
- **O** - Device releases clock, clocking in odd parity bit
- **P** - Device pulls clock low, signalling host to put stop bit (1) on data
- **Q** - Host puts stop bit on data
- **R** - Device releases clock, clocking in stop bit
- **S** - Device puts acknowledge bit (0) on data
- **T** - Device pulls clock low
   - Note: **S** and **T** may happen at the same time
- **U** - Device releases clock, signalling host to clock in acknowledge bit
- **V** - Device releases data, ending transaction

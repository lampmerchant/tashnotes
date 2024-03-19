# Inside AppleTalk (Second Edition) Errata

## GetMyZone (page 8-15)

Figure 8-3 indicates that the start index of a GetMyZone datagram should be 0.  However, it has been observed (on an SE/30 running System 7.5.5) that the Mac sends a GetMyZone request with a start index of 1.  This is consistent with page 8-8 ("zone names in the router are assumed to be numbered starting with 1").

# APRS Telemetry

The APRS telemetry packet looks like:
```
CALLSIGN>TNCID:T#nnn,nnn,nnn,nnn,nnn,nnn,nnnnnnnn
```

Where the first nnn is the three digit sequence number
The second through sixth fields are 000-255 zero padded unsigned integers
The last field is eight booleans

JavAPRSSrvr is very strict that packets follow this format, including
staying in the range 0-255 and being zero-padded, to match the filter t/t.
aprsc is much more forgiving and thus includes telemetry packets which
don't zero pad their fields or use values outside the 0-255 range.

The aprs.fi telemetry parser is described as 
"treats it as unsigned, but stores 2 decimals, 32 bits total or thereabouts."

Noted violators of the original APRS101 format include:

* Aprx - Uses floating point
* DIXPRS - Doesn't zero pad
* Direwolf - Large floating point values
* APMIxx  SQ3PLX - Large values
* APRSISCE win32 - large values (saw 500)
* APTT4 - large 3 digit values seen

# Proposed Telemetry format

```
T#nnn,n,n,n,n,n,bbbbbbbbCOMMENT
```

* nnn - three digit sequence number wraps 999 --> 000
* n - Analog telemetry values; decimal representation of a 
signed 32 bit integer, which should not but may be 
zero padded up to ten digits plus minus sign.
Positive numbers are indicated by the lack of a minus sign.
* bbbbbbbb - Eight boolean values represented by 0/1
* COMMENT - A textual comment no longer than 140 bytes

Lengths
* `T#nnn,` - 6
* `-0000000001,` - 12×5
* `bbbbbbbb` - 8
* `COMMENT` - 140 bytes


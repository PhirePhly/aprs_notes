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
* n - Analog telemetry value
* bbbbbbbb - Eight boolean values represented by 0/1
* COMMENT - Additional textual information which need not be parsed or stored

Analog telemetry values are single precision floating point values
which are always represented as base ten decimal encoding.
Values may be represented with leading zeros, and must still be parsed
as base ten values (and not base 8 as some parsers will handle numbers
with leading zeros).
Negative numbers must have a leading negative sign. Positive numbers
may not use a positive sign.

Lengths
* `T#nnn,` - 6
* `n,` - 5×required length for numberic value
* `bbbbbbbb` - 8
* `COMMENT` - Undefined
* Total length - 214 bytes

## Parameter labels

To aid in the presentation of telemetry values, APRS message packets
matching a specific format and addressed to the source of the telemetry
packets may be beaconed to give other stations information
on how each telemetry value should be processed and labeled.

### Telemetry Parameter Name Data
```
:MYCALL   :PARM.A1,A2,A3,A4,A5,B1,B2,B3,B4,B5,B6,B7,B8
```
Parameter name labels begin with "PARM." followed by 13 fields to name
the five analog and eight binary fields. There is no length restriction
on individual field lengths except that the total message contents
(PARM., fields, and commas) may not exceed 197.

These fields would typically be used for the title on telemetry plots.

### Telemetry Unit/Label Data
```
:MYCALL   :UNIT.A1,A2,A3,A4,A5,B1,B2,B3,B4,B5,B6,B7,B8
```
Unit/label data begins with "UNIT." followed by 13 fields to name
the units for the five analog and eight binary fields.
There is no length restriction
on individual field lengths except that the total message contents
(UNIT., fields, and commas) may not exceed 197.

These fields would typically be used for the Y axis on telemetry plots.

### Equation Coefficients Data

```
:MYCALL   :EQNS.a,b,c,a,b,c,a,b,c,a,b,c,a,b,c
```

Polynomial coefficients for the five analog fields.
The displayed or plotted value should be calculated from the raw
telemetry packet using the following formula:

```
[DISPLAY VALUE FOR A1] = a×A1 ^ 2 + b×A1 + c
```

Utilization of this packet by telemetry sources is optional,
but all telemetry receivers should support these coefficients
if the telemetry source chooses not to do processing before sending
the telemetry.

### Bit Sense / Project Title

```
:MYCALL   :BITS.XXXXXXXXProject Title
```

X indicates the active polarity of the corresponding b value in the
telemetry packets. Thus, when `b = X`, the value is considered true.

The project title may be displayed above any telemetry plot.
The project title is limited to 184 bytes.

## Links

Interesting paper on printing floating point: https://cseweb.ucsd.edu/~lerner/papers/fp-printing-popl16.pdf

Compressed telemetry: http://he.fi/doc/aprs-base91-comment-telemetry.txt

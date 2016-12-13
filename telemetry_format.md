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
T#nnn,n,n,n,n,n,bbbbbbbb
```

* nnn - three digit sequence number wraps 999 --> 000
* n - Analog telemetry value
* bbbbbbbb - Eight boolean values B1 - B8 represented by 0/1

Analog telemetry values are integer or floating point values
which are always represented as base ten decimal encoding.
Values may be represented with leading zeros, but must always be parsed
as base ten values (and not base 8 as some parsers will handle numbers
with leading zeros).
Negative numbers must have a leading negative sign. Positive numbers
should not use a positive sign.

Stations are not required to use all five analog and the binary field.
Stations may optionally only beacon the number of fields used, so a
telemetry packet may be formatted as any of the following:

```
T#nnn,n,n,n,n,n,bbbbbbbb
T#nnn,n,n,n,n,n
T#nnn,n,n,n,n
T#nnn,n,n,n
T#nnn,n,n
T#nnn,n
```

Fields which were not included should be assumed to be 0. Some implementations
include a comment field after the boolean field, which may be ignored.

Lengths
* `T#nnn,` - 6
* `n,` - 5×required length for numberic value
* `bbbbbbbb` - 8
* Maximum length - 214 bytes

## Telemetry Parameter Formatters

To aid in the presentation of telemetry values, APRS message packets
matching a specific format and addressed to the source of the telemetry
packets may be beaconed to give other stations information
on how each telemetry value should be processed and labeled.

Each subsequent comma separated field in a telemetry formatter packet is
optional. Excluding labels or units or setting the field to an empty string 
for a telemetry field indicates that that field is not being used, and
may be excluded by receiving stations from telemetry display.

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

Lacking an EQNS formatter packet, the coefficients should be assumed to
be {a,b,c} = {0,1,0}.

### Bit Sense / Project Title

```
:MYCALL   :BITS.XXXXXXXX,Project Title
```

X indicates the active polarity of the corresponding b value in the
telemetry packets. Thus, when `b = X`, the value is considered true.

The project title may be displayed above any telemetry plot.
The project title is limited to 183 bytes, with a recommended presentation
length of 23 characters.

## Links

Interesting paper on printing floating point: https://cseweb.ucsd.edu/~lerner/papers/fp-printing-popl16.pdf

Compressed telemetry: http://he.fi/doc/aprs-base91-comment-telemetry.txt

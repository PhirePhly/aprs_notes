# Maximum Transmission Units for APRS Networks


## Definitions

`[HDLC FRAME [AX.25 FRAME [APRS Packet]]]`

  * HDLC Frame: Everything from flag to flag
  * AX.25 Frame: Everything between the flag and Frame Checksum
  * APRS Packet: Everything inside the AX.25 Information Field

`[KISS FRAME [AX.25 FRAME [APRS Packet]]]`

  * KISS Frame: Everything from FEND to FEND
  * AX.25 Frame: Everything between the Command Code and FEND
  * APRS Packet: Everything inside the AX.25 Information Field

`[APRS-IS Packet [APRS Packet]]`

  * APRS-IS packet: Everything from Source Callsign to Carriage return-new line on a line not starting with #
  * APRS Packet: Everything after the first colon


## AX.25 UI Frame (excluding HDLC frame)

See: APRS101.PDF p.12

    [TNCID][SRCCALL][VIA PATH][CONTROL FIELD][PROTOCOL ID][INFORMATION FIELD]

7+7+56+1+1+256 = 328 Maximum

## Minimum AX.25 UI Frame (excluding HDLC, specifically for APRS)

    [TNCID][SRCCALL][CONTROL FIELD][PROTOCOL ID][INFORMATION FIELD]

7+7+1+1+1 = 17 minimum

Minimum Information Field size of 1 is because empty APRS packets are invalid. 
All APRS packets must have data type identifiers.

## HDLC Frame

    [FLAG][AX.25 Frame][FRAMECHECKSUM][FLAG]

1+328+2+1 = 332 (+ bit stuffing of 328 + 2) maximum

## KISS Frame

    [FEND][COMMANDCODE][AX.25 Frame][FEND]

1+1+328+1 = 331 maximum


## Third party encapsulation

This is inside 256 byte UI Info Field:
From http://www.aprs-is.net/IGateDetails.aspx

    IGATECALL>APRS,GATEPATH:}FROMCALL>TOCALL,TCPIP,IGATECALL*:original packet data

Inside Information Field:

    }[SRCCALL]>[TNCID],[NETWORKID],[IGATECALL]*:[PACKET PAYLOAD]

1+9+1+9+1+9+1+9+2 = 42 overhead

NETWORKID to indicate a packet is from APRS-IS is "TCPIP"

The original APRS101.PDF spec advocates for RF-gating the entire used
path, which can add up to 80 additional bytes. Is this deprecated?
It still doesn't explain the limitation on message length.

[AX.25 Info Field MTU] - [Third Party encap Overhead] = [APRS MTU]

256 - 42 = 214 byte APRS L3 MTU

Maximum length of any APRS packet to allow third party encapsulation is 214 bytes.

## APRS Messaging

Inside Information Field:

    :[DESTCALL+PADDING]:[MESSAGE]{[MESSAGEID]

1+9+1+67+1+5 = 84 maximum

Why is the maximum length for APRS Message packets 84 and not 214?!
Why can't the maximum message length be 197 instead of 67?


## APRS-IS packets

MTU defined as 512 - http://aprs-is.net/downloads/javAPRSSrvr/javAPRSSrvrUsersGuide.pdf

```
[SRCCALL]>[TNCID][,VIA PATH]*:[APRS PACKET][CARRIAGE RETURN][NEW LINE]
```

9+1+9+10×8+2+214+1+1 = 317

All this implies is that 512 is a valid MTU for APRS-IS TNC2 transport. No change required.

## Other APRS Maximum Lengths

Digging into the APRS101.PDF doc for the maximum length for other APRS data types

 * Location packets - 1+8+1+9+1+43 = 63
 * Time stamped location - 1+7+8+1+9+1+7+36 = 70
 * NMEA position - 1+209 = 210
 * DF Report - 1+8+1+9+1+7+8+28 = 63
 * DF Report w/ time - 1+7+8+1+9+1+7+8+28 = 70
 * Compressed location - 1+1+4+4+1+2+1+40 = 54
 * Compressed location w/ time - 1+7+1+4+4+1+2+1+40 = 61
 * Object - 1+9+1+7+8+1+9+1+43 = 80
 * Compressed object - 1+9+1+7+13+43 = 74
 * PARM. - 1+9+1+5+7+7+6+6+5+6+5+4+4+4+3+3+3 = 79 (THIS IS LONGER THAN 67!)
 * BITS. - 1+9+1+5+8+23 = 47
 * Status report - 1+7+62 = 70
 * Third party with used path = 9+1+9+80+1+9+1+9+1+1+n = 121+n
 

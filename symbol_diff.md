# Differences between symbol indexes

There are three symbol index files found on aprs.org:

http://www.aprs.org/symbols/symbols.txt
http://www.aprs.org/symbols/symbolsX.txt
http://www.aprs.org/symbols/symbols-new.txt

The actual de-facto standard is the WA8LMF icon set:

http://wa8lmf.net/aprs/APRS_symbols.htm

Useful archive links for historical diffs:
https://web.archive.org/web/20040301000000*/http://web.usna.navy.mil/~bruninga/aprs/symbolsX.txt

There are of course differences between them since not one of them is a
single source of truth on the matter of APRS symbols, and symbols-new
is often modified with little discussion.

## symbols.txt to symbolsX.txt

* `/(` Changed from CLOUDY to Mobile Satellite Station
* `/)` added as Wheelchair (handicapped)
* `//` changed from dot to red dot
* `/0` - `/9` are listed as obsolete but all still supported as circled numbers
* `/F` added as Farm Vehicle (tractor)
* `/[` generalized from jogger to human
* `/]` changed from PBBS to MAIL/post office
* `/c` added as Incident Command Post
* `/r` changed from antenna to repeater
* `\#` changed from numbered star to overlay digi
* `\%` added as power plant
* `\&` changed from numbered diamond to I-gate
* `\'` changed from crash site to incident site
* `\)` added as Firenet MEO, MODIS Earth Obs.
* `\*` SNOW removed
* `\/` added as Waypoint Destination
* `\8` added as 802.11 network node
* `\:` Hail removed
* `\A` changed from NUMBERED BOX to BOX DTMF & RFID & XO
* `\B` Blowing snow removed
* `\D` changed from Drizzle to DEPOTS
* `\F` Freezing rain removed
* `\G` Snow shower removed
* `\H` changed from Haze to Haze + hazards
* `\J` Lightening removed
* `\K` changed from Kenwood to Kenwood HT
* `\M` added as MARS
* `\O` added as Overlay Balloon
* `\Y` added as Radios and devices
* `\[` changed from Wall Cloud to Wall cloud + humans with overlay
* `\\` changed added as GPS symbol (and changed XYZ code)
* `\b` Blowing dust removed
* `\h` changed from ham store to store
* `\i` changed from BOXn digipeater to BOX or point of Interest
* `\k` added as SUV/ATV/etc
* `\p` Partly Cloudy removed
* `\x` added as Wreck or Obstruction
* `\y` added as Skywarn
* `\z` added as OVERLAYED Shelter
* `\{` Fog removed

Issues with both:

* `/z` is listed as TBD, but widely used as a shelter. It was originally listed as a shelter on 2003-10-29, and removed on 2004-05-06
* `\_` is noted in both as (green digi) which is incorrect

The `\I` and \backtick symbols collected a lot of the other weather events
as overlays.

## symbolsX.txt to symbols-new.txt

At this point we move from symbolsX.txt which seems to be the current
primary and secondary tables (with some overlays scattered in) to
the symbols-new.txt file, which only deals with overlays.

* `\O` changed from Balloon to ROCKET
* `\h` changed from store back to ham store. Hh listed as Home Depot instead of ham store
* `H-` changed from House w/ HF to Hydro-powered House
* `\K` and `KY` now both Kenwood

* `/z` still not defined as shelter, but often shown as one in icon sets despite only being defined for <1 year in 2003/2004.


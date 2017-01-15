# Power Source Data Extension

There has been a proliferation of APRS symbol overlays trying to encode
the power source for stations, which suffers from a few issues:
 1. Several duplicated sets of symbol overlays for power sources per station type (i.e. solar powered plane, solar powered car, and solar powered house)
 2. Prevents the symbol overlay from being used to convey more important information than the station's power source
 3. Is unable to represent more than one power source at a time
 4. Only gives the ability to indicate power source to a limited list of station types which have symbol power overlays defined

To correct these issues, I am proposing the definition of a new station
capability field to be included in the `<` capability packet and
included as a data extension in location or status comment fields.

`PWR=[Unordered list of power source codes]`

* B - Battery
* C - Coal, natural gas, wood, etc
* F - H2 fuel cells
* G - Internal combustion generator
* H - Hydroelectric
* N - Nuclear
* S - Solar
* T - GeoThermal
* U - Utility mains
* W - Wind

Multiple sources should be listed as several power source codes immediately
following the `=` with no seperator between power source codes.
The list of power sources is terminated by any character which is not
an upper case letter [A-Z].

The PWR extension is not meant to differentiate between different sources for
utility mains power.
The availability of power codes such as geothermal and nuclear are only
for completeness, and should not be used by stations which are simply
paying their local utility company to operate these sorts of power sources.
This means that a power source like `PWR=N` would be very unusual,
since it indicates that the station is operating a nuclear power source.

Stations should indicate the power sources they typically have available,
which may not currently all be active.
For example, an off-the-grid solar site would beacon `PWR=SB`
to indicate that they are a solar + battery station,
despite the fact that the solar panels aren't necessarily producting power
during the night.

This means that the PWR extension should never be used as a way to remotely
monitor power sources like whether a site has
utility power or not (i.e. it switching between `PWR=UB` and `PWR=B`).
The correct way to monitor if a site has power or not is to use one of the
boolean telemetry fields to encode the power state and beacon a telemetry
formatter such as `:MYCALL   :PARM.,,,,,AC Power`.

The PWR extension may be foregone when the power source for a station is unremarkable.
For example, house stations only running on wall power (`PWR=U`)
or car mobile stations only running on internal combustion generator (`PWR=G`)
may forego the additional packet length taken to indicate the obvious.

This power source capability / data extension field deprecates the following symbol overlays (with possible replacements indicated):
- E^ = Electric aircraft (`PWR=B`)
- S^ = Solar powered airplane (`PWR=S`)
- E> = Electric car (`PWR=B`)
- H> = Hybrid car (`PWR=BG`)
- S> = Solar powered car/vehicle (`PWR=S`)
- V> = GM Volt (`PWR=BG`)
- E# = Emergency powered digipeater (`PWR=?`)
- 5- = 50Hz mains power, house (`PWR=U`)
- 6- = 60Hz mains power, house (`PWR=U`)
- B- = Backup battery power, house (`PWR=UB`)
- E- = Emergency power, house (`PWR=?`)
- G- = Geothermal, house (`PWR=T`)
- H- = Hydro powered, house (`PWR=H`)
- S- = Solar Powered, house (`PWR=S`)
- W- = Wind powered, house (`PWR=W`)

Many of these symbols would likely be replaced with something other than
what's listed; e.g. the overlay for a solar-powered house lacked the
expressiveness to distinguish if it's a grid-tied solar-powered house
(`PWR=SU`) or if it's an off-the-grid house with a battery bank (`PWR=SB`).

The distinction between 60Hz utility power and 50Hz utility power is not
made in this data extension since the possible application of where that
distinction would need to be made is incredibly small.
For the very few applications where mains frequency would be of any
concern to other users, APRS nodes may otherwise include a note on the power
frequency. One possible method would be `PWR=U(50Hz)`;
power capability parsers should terminate at the '(' and need not process the
frequency indication after it.

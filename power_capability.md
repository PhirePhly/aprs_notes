There has been a proliferation of APRS symbol overlays trying to encode
the power source for stations, which suffers from a few issues:
 1. Several duplicated sets of symbol overlays for the same information
 2. Prevents the symbol overlay from being used to convey other info
 3. Is unable to represent more than one power source

To correct these issues, I am proposing the definition of a new station
capability field to be included in the '<' capability packet or
included in location or status comment fields.

PWR=[list of power sources]

* B - Battery
* C - Coal, natural gas, wood, etc
* F - H2 fuel cells
* G - Gas generator
* H - Hydroelectric
* N - Nuclear
* S - Solar
* T - GeoThermal
* U - Utilty mains
* W - Wind

Stations may either indicate their currently used power source,
or provide a list of all the available power sources.
For example, an off-the-grid solar site might either beacon 'PWR=SB'
continuously to indicate that they are a solar + battery station, 
or beacon 'PWR=S' during the day until the battery bank stops charging,
and then revert to 'PWR=B' during the night.

A home station
operating with a battery backup may either beacon 'PWR=UB' to indicate that
they have battery backup for their utilities, or may beacon 'PWR=U' until the
power fails, at which point the node may revert to 'PWR=B'

This capability field deprecates the following symbol overlays:
- E^ = Electric aircraft (PWR=B)
- S^ = Solar powered airplane (PWR=S)
- E> = Electric car (PWR=B)
- H> = Hybrid car (PWR=BG)
- S> = Solar powered car/vehicle (PWR=S)
- V> = GM Volt (PWR=BG)
- E# = Emergency powered digipeater (PWR=?)
- 5- = 50Hz mains power (PWR=u)
- 6- = 60Hz mains power (PWR=U)
- B- = Backup battery power, house (PWR=UB)
- E- = Emergency power, house (PWR=?)
- G- = Geothermal, house (PWR=T)
- H- = Hydro powered, house (PWR=H)
- S- = Solar Powered, house (PWR=S)
- W- = Wind powered, house (PWR=W)

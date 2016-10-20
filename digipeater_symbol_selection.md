# Symbol Selection for Digipeaters and Internet Gateways

*Kenneth Finnegan, W6KWF - 2016*

This guide is meant to assist the operators of APRS digipeaters (digis) and
Internet gateways (I-gates) to select the correct contemporary symbol
and symbol overlay to use for their APRS node.
These symbols give other local operators a first glance overview of
the topology of the local RF network, and should give them a good idea of
what kind of coverage they should expect.
The need for this document comes from the fact that there has been a
large number of different suggested conventions for infrastructure
symbology, without a distilled single source of information on selecting
symbols specifically for digipeaters and I-gates.

Each symbol listed will be bulleted, and start with the two character
symbol code, which consists of the overlay character and then the
symbol character. If the overlay character is either a forward-slash or
back-slash, the symbol is one of the primary or secondary table APRS
symbols, with no character overlay.

Of course, the large number of possible permutations on different
digipeater / I-gate configurations means that not all APRS infrastructure
nodes will cleanly fall into one of these categories. This document
should be considered a set of suggestions and local discretion used to
select the most appropriate symbol.

## Internet Gateways (I-gates)

I-gates are APRS stations which are connected to the Internet and
act as a bridge between the local RF network and the global APRS-IS
(-Internet System) network.
I-gates may be either unidirectional, where they only gateway APRS packets
from RF to the APRS-IS network and are called Rx-only I-gates,
or may be bidirectional, where they can additionally route select relevant
packets from the APRS-IS back to the local RF in addition to gatewaying
packets to the Internet.
This action of gating packets from the APRS-IS back to the local RF net
is often called "RF-gating".

I-gates do not perform any APRS alias-based digipeating. 
If a station provides both local RF-to-RF digipeating services as well
as I-gating packets, it should be considered an I-gate equipped digipeater.

* R& - Rx-only I-gate without the ability to RF-gate packets back to the
local RF network
* I& - Generic VHF I-gate with the ability to RF-gate packets
* /& - HF Gateways are I-gates which are operating on the 30m APRS
network instead of the local VHF net

## Digipeaters

Digipeaters are the core of the RF APRS network which allow other nodes
to be able to reach the entire local RF network while operating
from non-ideal locations using low power or low efficiency antennas.
Digipeaters receive packets, check to see if they are requesting digipeater
service from them, and then selectively repeating (or digipeating) the
packet using the digipeater's higher antenna and power.

* /# - High level digipeater supporting the global WIDEn-N alias

Since high level digipeaters are unable to hear all trackers in all
locations inside their coverage area, there are also what are called
fill-in digipeaters, which are lower level digipeaters which are meant
to only shuttle traffic very local to them up to the higher level
digipeater.
Fill-in digipeaters are not meant to extend the range of a packet
once it has reached a high-level digipeater, and will often only cover
a subset of the same area as the nearest high-level digi but with
a better sensitivity.
Because of this, fill-in digipeaters should not be used after the first
hop (which translates into only ever using WIDE1-1 as the first hop
in a path).

* 1# - Fill-in digipeaters which should only be used for the first hop

In many high population density areas, APRS will not be limited to a single
VHF frequency. For example, in some parts of the US 144.990MHz is defined
as a "low power tracker" frequency, where quieter stations may beacon
and be hear by what are called Alt-input digipeaters, which then
digipeat these quiet stations into the local (and nosier) primary channel.

This classification does not generally include multi-port digipeaters
which are used to route in both directions between either two VHF or
a VHF and a UHF APRS network which are both meant for general purpose
traffic. These should be considered two standard digipeaters which happen
to also serve the roll of bridging these two APRS frequencies.

* A# - Alternate input digipeater

Many high level digipeaters support additional local aliases in addition
to the global WIDEn-N alias. This is called the SSn-N system, since it
was originally developed for serving US states.
For example, every digipeater in Pennsyvania would be configured to
respond to the alias PAn-N in addition to WIDEn-N. This would allow
any stations in Pennsyvania with traffic relevant to others in the state
to use the path PA7-7. This would cause the packet to flood the entire 
state without spilling out and causing undesirable traffic across the
entire Eastern seaboard.

SSn-N digipeaters should note what additional aliases they support
in their beacon comment or status fields.

* S# - High level digipeater supporting local SSn-N aliases

Most high level digipeaters are situated on mountain tops, which
traditionally lacked reliable Internet connections. As this has changed,
a new class of station known as "I-gate digipeaters" has become
increasingly common. These digipeaters not only provide RF digipeater
service, but also I-gate packets to and from the Internet.

* I# - Digipeater providing I-gate services

Digipeaters are generally expected to be long-lived, permanent, fixtures of the
local APRS network. Local APRS stations are encouraged to set their path
to route APRS traffic through specific local digipeaters instead of using
aliases like WIDEn-N when getting digipeated by those single specific
digipeaters provides the desired coverage.

Digipeaters are also expected to behave as prototypical APRS digipeaters.
When a new digipeater is built supporting unusual behavior which users might
find confusing, or is set up temporarily for activities such as coverage
testing from a new site, it is considered an experimental digipeater.

* X# - Experimental digipeater

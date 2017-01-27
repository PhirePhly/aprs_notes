# Event Time Data Extension Specification

This document formalizes the radio net and in-person meeting time spec
which is off-handedly mentioned in the APRS Freq Spec text file
(http://www.aprs.org/info/freqspec.txt).
This data extension is meant to indicate weekly or monthly nets/meetings
which would be of interest to amateur radio operators.
It does not include any information on where or what frequency the event happens,
so that should be clear from the context of the rest of the packet.
(i.e. Meeting events could be included in a "club shack" object beacon,
and net events could be in repeater object beacons or accompanied by a freq spec)

These data extensions may be used similar to all other APRS data extensions,
so may be included anywhere in location packet comment fields,
in status packets, in object comment fields, messages, etc.

The length of this data extension is not limited to 7 bytes
and may be dynamic based on the specificity of the event time.

These data extensions are not meant to replace ISO 8601 time/date stamps,
which should be used for specific non-recurring events such as annual hamfests.

There is no way to express biweekly events in this extension.
Other methods should be used to advertise events on that schedule.

## Extension Format

```
[Event Identifier](Week of Month)[Day of Week][Time of Day](Time zone abbreviation)
```

Items in square brackets are required; those in parenthesis are optional.

The event identifier indicates what kind of event is being advertised:
* `NET` - Radio net
* `MTG` - Club or social in-person meeting

Week of month is optional to indicate that an event only happens
on a certain week of a month.
Value may be one or more of:
* `1st`
* `2nd`
* `3rd`
* `4th`
* `5th`

Day of week is required and indicates the days of the week an event happens.
Values may be one or more of:
* `Mo` (Monday)
* `Tu` (Tuesday)
* `We` (Wednesday)
* `Th` (Thursday)
* `Fr` (Friday)
* `Sa` (Saturday)
* `Su` (Sunday)

The week of the month is calculated per day,
not including partial weeks where the day of interest falls in the previous month.

Time of Day is required and indicates the time of the event in 24h local time.
It is a four digit number less than `2400`.

The time zone abbreviation is an optional indication of which time zone is used,
and should only be used when near the edge of two time zones
and it's unclear which conventional local time to use.
This field should be very rarely used.

## Examples

`NETTu1745` - A net happens weekly on Tuesday at 5:45PM.

`MTG2ndWe1900` - A meeting happens on the second Wednesday of the month at 7PM.

`NETMoTuWeThFr0900` - The 9AM talknet happens every weekday at 9AM.

`MTG1st3rdSu1000` - Brunch is had on the first and third Sundays of the month at 10AM


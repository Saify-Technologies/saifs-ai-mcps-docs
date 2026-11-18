# Indian Railways IRCTC

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/indian-railways-irctc`. It answers Indian Railways questions: which train, which route, what is running now, and what is available on a date you name.

People search for this as an Indian Railways lookup, an IRCTC train search, a live train status, or a station arrival board. The short install page is the [server README](../README.md). This page is what each tool takes, why the date and the station code matter, and what this server will not do.

| | |
|---|---|
| **Registry name** | `ai.saifs/indian-railways-irctc` |
| **Endpoint** | `https://transport-agent-claude.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Six lookup tools.

A 5-digit train number returns the train, its full list of stops, or where it is right now. Two station codes return the trains between them. Add a journey date and the same pair returns classes and availability. A station code returns the arrivals and departures on the board.

The reply is a journey plan, not a dump of every train. One best option comes first, with the class that fits the ask. If no usable direct train runs that day, the plan continues with a connection: fewest changes, a buffer at the change, and the date of each leg. A leg is described as verified when a tool confirmed it, and as practical or partial when a piece is still missing.

This server is information only. It does not book, pay, cancel, refund, or change a ticket. A PNR and a ticket status stay on the official Indian Railways and IRCTC channels.

## Why a model should call the tool

A language model can name a famous train and still invent the departure, the running day, or a seat that is not there. These tools return the train record, the route, the board, and the availability for the date you send. A missing field stays missing. It is not filled in.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks.

A train number is exactly five digits, such as `12961`. A station is a code, such as `RTM` for Ratlam or `MMCT` for Mumbai Central. If a city has more than one station, send the code. The server does not guess `NDLS` from “Delhi.” A journey date is `DD-MM-YYYY`. A relative date is converted to that form before the call. A date more than 60 days ahead is refused. Trains outside India are refused.

For a source and a destination, the journey date is required. If it is missing, the assistant asks once and waits. Without a date, `railway.train.betweenStations` lists the trains that link the two codes. With a date, `railway.train.searchByRouteDate` returns those trains for that day, including class and availability.

```json
{
  "from_station": "RTM",
  "to_station": "MMCT",
  "journey_date": "15-12-2026"
}
```

Availability on that result is read in plain words: available, RAC, a waitlist, or regret. A class with no figure is unavailable, not a guess. Running days matter. A train that exists between the stations but does not run on the asked date is reported that way, and the nearest day it does run can be checked with the same date search.

### One train

`railway.train.getByNumber` returns the train for that number: name, source, and destination. `railway.train.route` returns the stop sequence. `railway.train.liveStatus` returns where that train is now and whether it is on time. An optional date on the live-status call is ignored. The status is the current run.

### A station

`railway.station.liveBoard` takes one station code and returns the trains arriving and departing there.

### When the direct train will not do

If the direct trains are missing, all regret, or only an impractical waitlist, the plan looks for one change, then two. Hubs are chosen from the geography and from trains the tools actually return. Each leg has to leave after the previous leg arrives. A next-day departure is labeled as the next day. An overnight halt is stated with the city and the times. A cross-city change, such as two different stations in the same city, is named.

If the exact pair is waitlisted and one train is still the right one, the same train can be checked from an earlier stop it really serves, or to a later stop it really serves. That booked segment is labeled separately from the station where you board and the station where you get off. It is a suggestion about availability. It is not a ticket, and current railway boarding rules still apply.

### Prompts

- “Which trains go from Ratlam (RTM) to Mumbai Central (MMCT) on 15-12-2026?”
- “Where is train 12961 right now?”
- “What is the full route of train 12961?”
- “What is arriving at MMCT?”
- “I need a sleeper from NDLS to BCT next Friday. One best train.”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `railway.train.getByNumber` | A 5-digit `train_number` | The train record for that number | You want trains between two stations, a date search, or live status |
| `railway.train.searchByRouteDate` | `from_station`, `to_station`, and `journey_date` as `DD-MM-YYYY` | Trains for that pair on that date, with class and availability | The date is missing, more than 60 days ahead, or the trip is outside India. Also skip it for a live position or a full stop list of one known train |
| `railway.train.betweenStations` | `from_station` and `to_station` | The trains that link those codes, and how many | You already have a travel date. Use the date search. Also skip it for a PNR, a live board, or a booking |
| `railway.train.route` | A 5-digit `train_number` | The full route and stops | You want the live position or the station board |
| `railway.station.liveBoard` | A `station_code` | Arrivals and departures at that station | You want one train’s running status, or that train’s whole route |
| `railway.train.liveStatus` | A 5-digit `train_number` | Current running status and position | You want availability, a booking, a PNR, or the full timetable |

## How to read a result

- **One recommendation comes first.** Alternatives follow only when they matter. “Best” is the practical option for the wording you used: fastest, cheapest, or a seat that is actually open.
- **Available, RAC, waitlist, and regret are the availability words.** A blank class is unavailable.
- **A verified leg was confirmed by a tool.** A partial or practical label means a connection is the best structure found, and one piece is still unconfirmed. That route is not described as final.
- **Times come from the tool.** If a route search and the train-number timetable disagree, the train-number timetable is the one to trust.
- **The live board is the station.** The live status is one train. They are different calls.
- **Nothing here is a reservation.** Showing availability does not hold a berth.

## Data and what this will not do

You send a train number, station codes, and a date. Each user signs in with OAuth. The lookups are read-only.

The server will not book, take payment, cancel, refund, or modify a ticket. It will not look up a PNR or a reservation confirmation. It will not search trains outside India. It will not search a journey date more than 60 days ahead. It will not guess a station code from a city name when more than one station fits.

For a PNR, use the official Indian Railways or IRCTC channels.

Privacy policy: https://saifs.ai/privacy_policy

## Support

- Install and client setup: [server README](../README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy

# Faster Trading
Less idle time for station traders in X4 Foundations

- Our GitHub repo: https://github.com/Vectorial1024/v1024_faster_trading
- Our EgoSoft Forums link: (WIP)
- Our Steam Workshop link: https://steamcommunity.com/sharedfiles/filedetails/?id=3495318749
- Our Nexus link: (WIP)

> Effective logistics; that's how simple it is!

---

## Quick Info
TL;DR: no player action needed!

Firstly, trade hubs are defined as trading stations that trade in many types of wares in a small corner of the map.

This mod aims to improve trade hub throughput by reducing the various undocumented trade-related idle time, and also diverting some traders to handle importing wares.
This hopefully reduces the need to use custom trading scripts (e.g. TaterTrade, the Mules, etc.).

If you have used Civilian Fleets before, this should feel similar to when you would be setting up a regional trading fleet to move wares from this side of the map to the other side of the map.

Compatible with my other trade tweak mods e.g. Fully Loaded.

The reasoning is explained below.

## Technical Information

It turns out, vanilla station traders have a lot of idling built into them:
- The traders are coordinated to have only 1 of them look for trades at any time
  - Other traders must wait for their turn; this lasts for max 1.5 to 5.5 minutes
  - Code comments suggest this is to prevent trade stampeding, but they didn't make this a hard requiremment...?
- Traders idle a bit while iterating through nearby sectors to find matching trade offers
  - About 1 to 3 seconds per sector searched
- Traders will always try to export station wares first, and will not import unless there are really no good export deals
  - Traders will directly consider importing only when the station has nothing to export

These behaviors are good when the station is a normal station (i.e. a "factory station") with not much types of wares to buy/sell, and the station coverage area is small.
It also makes sense to prevent trade stampeding from occuring in the first place.

But clearly, these behaviors also create soft limits when we consider trade hubs with dozens of wares to trade across perhaps a dozen sectors at max 5 sector range:
- 12 types of wares: about 12 to 36 seconds each
- 15 sectors in range: about 15 to 45 seconds each
- worst case: 12 wares * 15 sectors = 180 to 1620 seconds for export only -> 3 to 27 minutes just to find exporting deals
- other traders basically has to wait for someone to be done with these!

It also prevents trade hubs from reliably sustaining a stock for itself, because incoming wares are very quickly traded away by their very own subordinates.

This explains why trade hubs usually do not perform as good as expected.

As such, this mod tries to do the following:
- Respect the one-trader lock as much as possible
- Slightly reduce the waiting time during the ware/space iteration
- Divert some traders to directly consider import offers

These changes should improve trade hub throughput, and make them really act like logistics hubs we may intend them to.

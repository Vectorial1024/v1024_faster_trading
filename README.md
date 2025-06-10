# Faster Trading
Less idle time for station traders in X4 Foundations

- Our GitHub repo: https://github.com/Vectorial1024/v1024_faster_trading
- Our EgoSoft Forums link: N/A
- Our Steam Workshop link: N/A
- Our Nexus Mods link: N/A

> Effective logistics; that's how simple it is!

**Notice: Mod is currently temporarily unavailable because it causes unexpected full freeze.**

---

## Quick Info
TL;DR: no player action needed!

This mod aims to increase station trading throughput by doing the following to station traders:
- reduce their various undocumented trade-matching-related idle time; and
- diverting some (20%) station traders to be import-first, instead of the usual export-first
  - this works in conjunction with the new shortage handling feature since 7.6
All stations (usually player stations) are benefitted, but the underlying goal is to improve player trading station throughput, to make them more like "regional logistics hubs".

This mod **does NOT modify** the following:
- the stay-at-dock time of station traders
- the behavior of "free" traders:
  - unassigned traders; or
  - traders working in a Mimic-fleet

Some expected practical benefits:
- late-game trading networks require less trading ships
- vanilla-like trading behavior is good enough, less need to use custom trading scripts (e.g. TaterTrade, the Mules, etc.)

Be warned: _in theory_, this mod may result in more frequent lag spikes, but perhaps this risk is balanced out by actually having less trade ships for the game to process.

If you have used Civilian Fleets before, this should feel similar to when you would be setting up a regional trading fleet to move wares from this side of the map to the other side of the map.

Compatible with my other trade tweak mods e.g. Fully Loaded.

The reasoning is explained below.

## Technical Information

It turns out, vanilla station traders have a lot of idling built into them:
- The traders are coordinated to have only 1 of them look for trades at any time
  - Other traders must wait for their turn; this lasts for max 1.5 to 5.5 minutes
  - Code comments suggest this is to prevent trade stampeding, but they didn't make this a hard requiremment...?
- Traders idle a bit while iterating through nearby sectors to find matching trade offers
  - e.g., if exporting wares, about 1 to 3 seconds per sector searched
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

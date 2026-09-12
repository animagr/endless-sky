# Foundations: making Endless Sky an open sandbox like X4

A design note on how far this checkout can be pushed toward the X4: Foundations
loop - find demand, invest, automate, reinvest, hold territory - and what each
step costs in merge pain.

Written 2026-09-12 against this fork at commit `759c92b84` (0.11.3). Every claim
below was checked against the source, not against the wiki; file and line
references are given so they can be rechecked when they drift.

The framing this note assumes: X4 is a 4X strategy game viewed from a cockpit,
Endless Sky is a piloting RPG with a written campaign. Closing that gap means
adding an *empire* layer, not more ships.

## 1. What the engine already does

Endless Sky is not a static universe. There is a real daily economy tick.

- `GameData::StepEconomy()` ([GameData.cpp:422](source/GameData.cpp#L422)) runs
  once per in-game day, called from [Engine.cpp:1537](source/Engine.cpp#L1537).
- Each system keeps 89% of its stock, exports 10% to linked neighbours, and adds
  `Random::Normal() * 2000` tons of new production
  ([System.cpp:1101](source/System.cpp#L1101), constants at
  [System.cpp:38](source/System.cpp#L38)).
- Price is `base - 100 * erf(supply / 20000)`
  ([System.cpp:1307](source/System.cpp#L1307)).

Also present and useful:

- Events can rewrite `system`, `planet`, `government`, `fleet`, `shipyard`,
  `outfitter`, `galaxy`, `news`, and `link`/`unlink`
  ([UniverseObjects.cpp:143](source/UniverseObjects.cpp#L143)).
- Missions have an `on daily` trigger ([Mission.cpp:379](source/Mission.cpp#L379)).
- Actions can schedule a future event with `event <name> <minDays> <maxDays>`
  ([GameAction.cpp:217](source/GameAction.cpp#L217)) and can `give ship` /
  `take ship` ([GameAction.cpp:172](source/GameAction.cpp#L172)).
- Planets can pay daily `tribute`, readable as the condition `tribute: <planet>`
  ([Planet.h:167](source/Planet.h#L167)).
- Gamerules already ship a `fleet size limitation` with `none`, `ship capacity`,
  `crew capacity` and `administrative capacity` modes, plus
  `default admin cap 25` ([data/gamerules.txt](data/gamerules.txt),
  [Gamerules.cpp:110](source/Gamerules.cpp#L110)).
- The condition system is effectively a scripting language: arithmetic,
  comparisons, functions, `roll:`, `random`, date counters,
  `hyperjumps to system:`, `scheduled event:`, and read access to the whole
  fleet, cargo, planetary storage, reputation and net worth
  ([wiki/Player-Conditions.md](wiki/Player-Conditions.md)).

## 2. The three gaps

**Gap 1: nothing consumes anything.** In `GameData::StepEconomy()` the player's
`purchases` map is the only demand sink in the galaxy. Production is a random
walk, not a supply chain. Nothing is built from anything else. X4's entire
economy method - find the Wharf's bottleneck ware and feed it - has no
counterpart here because there is no downstream consumer.

**Gap 2: prices barely move.** The `erf` term is bounded, so a commodity price
can only ever travel +/- 100 credits from a hand-authored base, no matter what
happens to supply. Trading is closer to a lookup table than a market.

**Gap 3: only one system is simulated.** `Engine::SpawnFleets()`
([Engine.cpp:2062](source/Engine.cpp#L2062)) spawns only in
`player.GetSystem()`. There is no galaxy-wide ship registry. X4's attention
levels exist because X4 simulates everywhere at two fidelities; this engine
simulates exactly one system at one fidelity.

A fourth, smaller gap: `Orders::Types`
([orders/Orders.h:33](source/orders/Orders.h#L33)) has 11 entries - hold, move
to, gather, attack, mine, harvest, scan - all manual, all in-system, all cleared
on system change. There are no standing orders, so there is no automation.

## 3. The fork constraint decides the approach

Per [CLAUDE.md](CLAUDE.md) this fork must keep merging `upstream/master`.
`AI.cpp` (4754 loc) and `PlayerInfo.cpp` (4736 loc) are the two files upstream
touches most often. So the ranking for any work here is:

**plugin data > new engine files with one registration hook > editing the hot files.**

Everything in Tier 1 below is invisible to a merge. Tier 2 is a handful of
contained diffs. Tier 3 is the one that costs real maintenance.

## 4. Tier 1 - pure plugin, zero C++, merges forever

This box is bigger than it looks.

**The tick loop.** An `invisible` mission with `repeat` and an `on daily` trigger
fires every day the calendar advances. That is the simulation heartbeat. Combined
with `event <name> <minDays> <maxDays>`, a mission can schedule universe changes
into the future and re-arm itself.

**Player-owned stations.** Model wares as low-mass outfits and use **planetary
storage** as the warehouse: `outfit (storage): <ware>` reads it, and an
`outfit <ware> <n>` action in an `on daily` block moves it. A hull-parts plant
becomes "consume 5 ore + 2 energy from storage, produce 1 hull part". Vertical
integration, payback times and bottlenecks are all expressible this way.

**Territory that changes hands.** Drive the same event machinery the Free Worlds
campaign uses ([data/human/campaign events.txt:40](data/human/campaign%20events.txt#L40)
flips a system's government and swaps its spawn tables on a fixed date), but
trigger it from conditions instead of dates. That gives a rolling faction war
with no script.

**Income from territory.** `tribute` already pays daily and is readable as a
condition, so it is the closest existing analogue to X4 sector income.

**Drop the clock.** The 230-day war deadline is one dated event. A sandbox plugin
can delete or defer it and let the war fire off player actions instead.

**Empire-size gating.** Switch the `fleet size limitation` gamerule to
`administrative capacity` for an X4-style "you need admin infrastructure to run a
bigger empire" constraint, at zero cost.

### The one real wall in Tier 1

`"credits"` is read-only - it is registered with a getter and no setter
([PlayerInfo.cpp:3956](source/PlayerInfo.cpp#L3956)) - and `payment` takes a
literal, not an expression ([GameAction.cpp:185](source/GameAction.cpp#L185)).
So a plugin cannot pay out a computed amount. The data-only workaround is binary
decomposition: parallel missions paying 1k / 2k / 4k / 8k, each gated on a
condition bit. It works, and it is ugly.

## 5. Tier 2 - small additive engine changes

Four changes, in value order.

**5.1 Let `payment` take a value expression.** About 20 lines in
[GameAction.cpp:185](source/GameAction.cpp#L185), reusing the expression parser
`ConditionAssignments` already has. This single change unlocks everything in
Tier 1: a simulated economy that can actually pay out a number it computed.
Do this first; it has the best ratio of unlocked capability to merge risk of
anything in this note.

**5.2 Make the economy a supply chain.** Add per-commodity `consumes` /
`produces` terms to `System::Price` and a consumption pass in `StepEconomy`.
Both functions are about 15 lines today. Widen `LIMIT` or the `erf` coefficient
so shortages can spike prices rather than moving them 100 credits. This is the
change that makes trading feel like X4 instead of like a lookup table, and it is
confined to two small functions plus a data loader.

**5.3 A standing-orders order type.** Add `TRADE_ROUTE` to `Orders::Types` and
let it survive system changes. The order system was recently factored into
[source/orders/](source/orders/), so the enum addition is clean - but the
handling lands in `AI::FollowOrders`, which is the first hot file touched. Keep
it to one `case` block.

**5.4 A `stationsim` data type.** New files `source/StationSim.{h,cpp}`,
registered in `source/CMakeLists.txt`, loaded by `UniverseObjects`, ticked from
the same daily hook as the economy. Owns production chains properly instead of
faking them with outfits-in-storage. Additive: new type, new files, two
registration lines.

## 6. Tier 3 - the one that actually costs you

**Off-screen simulation.** To get X4's living galaxy - NPC traders that really
move goods, fleets that really fight in systems you are not in, factions that
lose sectors while you are away - you need a persistent galaxy-wide ship registry
and a statistical resolver, which is X4's low-attention mode. That is a new core
subsystem, and it will collide with `Engine.cpp` and `AI.cpp` on every upstream
merge.

If it is wanted anyway, the merge-survivable shape is a self-contained
`GalaxySim` class ticked from the existing daily hook, reading and writing only
`System` and `Government` state, never touching the in-system ship list. Track
abstract fleet strength numbers per system, not real `Ship` objects. Fidelity is
lost; mergeability is kept.

## 7. Recommended build order

A plugin named "Foundations" plus exactly one engine patch:

1. Patch `payment` to accept expressions (Tier 5.1). Half a day, about 20 lines,
   will not conflict.
2. Plugin: daily-tick mission, station production via planetary storage,
   tribute-based sector income.
3. Plugin: switch `fleet size limitation` to `administrative capacity` so empire
   size is gated.
4. Plugin: remove the fixed war date, drive faction territory from conditions.
5. Only if that is still not enough: the `StepEconomy` supply-chain change
   (Tier 5.2).

Steps 2-4 never conflict with upstream at all. That order reaches the X4 loop
without off-screen simulation, and defers every expensive decision until the
cheap ones have been proven insufficient.

## 8. Code touchpoints, for rechecking

| What | Where |
|---|---|
| Daily economy tick | [GameData.cpp:422](source/GameData.cpp#L422), called from [Engine.cpp:1537](source/Engine.cpp#L1537) |
| Per-system production | [System.cpp:1101](source/System.cpp#L1101); constants [System.cpp:38](source/System.cpp#L38) |
| Price formula | [System.cpp:1307](source/System.cpp#L1307) |
| Fleet spawning (player system only) | [Engine.cpp:2062](source/Engine.cpp#L2062) |
| Escort order types | [orders/Orders.h:33](source/orders/Orders.h#L33) |
| What events may change | [UniverseObjects.cpp:143](source/UniverseObjects.cpp#L143) |
| Mission triggers incl. `on daily` | [Mission.cpp:379](source/Mission.cpp#L379) |
| `payment`, `event`, `give ship` actions | [GameAction.cpp:172](source/GameAction.cpp#L172) onward |
| `credits` is getter-only | [PlayerInfo.cpp:3956](source/PlayerInfo.cpp#L3956) |
| Fleet size limitation gamerule | [Gamerules.cpp:110](source/Gamerules.cpp#L110), [data/gamerules.txt](data/gamerules.txt) |
| The war date to remove | [data/human/campaign events.txt:40](data/human/campaign%20events.txt#L40) |
| Condition namespace reference | [wiki/Player-Conditions.md](wiki/Player-Conditions.md) |
| Plugin layout reference | [wiki/CreatingPlugins.md](wiki/CreatingPlugins.md) |

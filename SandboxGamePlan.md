# Building an X4-style sandbox game on the Endless Sky codebase

A feasibility study and phased plan for forking this checkout into a new game:
no story, open sandbox from minute one, mine / trade / fight / influence the
galaxy, with a simulated economy and a living NPC population.

Written 2026-09-14 against this fork at commit `759c92b84` (0.11.3). Every
architectural claim was checked against the source; file and line references are
given so they can be rechecked when they drift.

Companion document: [FoundationsMod.md](FoundationsMod.md) covers the *cheaper*
version of this idea - pushing stock Endless Sky toward X4 with a plugin plus
one or two small engine patches, while staying mergeable with upstream. Read
that first if a full fork is not obviously justified. This document assumes the
fork is on the table.

## Contents

1. [The finding that reshapes the project](#1-the-finding-that-reshapes-the-project)
2. [Licensing decides the business model](#2-licensing-decides-the-business-model)
3. [What you inherit](#3-what-you-inherit)
4. [What you delete](#4-what-you-delete)
5. [Phase 1 - one ship, a living galaxy](#5-phase-1---one-ship-a-living-galaxy)
6. [Phase 2 - fleets and automation](#6-phase-2---fleets-and-automation)
7. [Phase 3 - structures and ownership](#7-phase-3---structures-and-ownership)
8. [Risks](#8-risks)
9. [First milestone: the vertical slice](#9-first-milestone-the-vertical-slice)
10. [Code touchpoints](#10-code-touchpoints)

---

## 1. The finding that reshapes the project

**Endless Sky already simulates the entire galaxy. It throws that away every
time the player takes off.**

This is the single most important fact for scoping the work, and it is not
documented anywhere in the repository:

- `Engine::ships` holds ships from *any* system, not only the player's.
  [Engine::Place()](source/Engine.cpp#L317) explicitly handles ships in other
  systems: "If the position is still (0, 0), the special ship is in a different
  system [...] place it in flight", then calls `Fleet::Place(*ship->GetSystem(), *ship)`.
- [MoveShip()](source/Engine.cpp#L1948) runs **full physics on every ship in the
  list regardless of system**. The system check appears only at
  [Engine.cpp:2021](source/Engine.cpp#L2021), *after* movement and boarding have
  already happened, and it gates just three things: fighter launch, weapon fire,
  and anti-missile / tractor registration.
- [AI::Step()](source/AI.cpp#L720) iterates every ship in the list with **no
  system filter at all**.
- Only [FillCollisionSets()](source/Engine.cpp#L2047) is scoped to
  `player.GetSystem()`.
- Per-system commodity supply is already simulated *and* persisted:
  `GameData::WriteEconomy()` writes it into the save file
  ([PlayerInfo.cpp:5226](source/PlayerInfo.cpp#L5226)).

The galaxy feels dead for two mundane reasons, not one architectural one:

1. [Engine::Place()](source/Engine.cpp#L317) calls `ships.clear()` on every
   takeoff, so the world is rebuilt from nothing each flight.
2. [SpawnFleets()](source/Engine.cpp#L2062) only ever populates
   `player.GetSystem()`.

### What this means for scope

Phase 1 is **not** "write a galaxy simulator". It is population management,
level-of-detail, and persistence, layered on a simulator that already runs. That
is a materially cheaper and lower-risk project than it appears from the outside.

The corollary: the missing piece is not tactical AI, which is already good (see
section 3), but **strategic** AI. Nothing in the codebase decides what to
produce, what to haul, or where to expand.

---

## 2. Licensing decides the business model

This is the first design constraint, not a footnote. Decide it before writing
code; it is unrecoverable later.

| Component | Licence | Consequence |
|---|---|---|
| All code (`source/`) | **GPL-3+** | Any derivative is GPL-3+. Selling it is permitted; keeping source closed is not. |
| Art, 162 entries | CC-BY-SA-4.0 | Share-alike: derivative art stays CC-BY-SA, with attribution. |
| Art, 119 entries | CC0 | Unencumbered. |
| Art, 72 entries | public domain | Unencumbered. |
| Art, 19 entries | CC-BY-SA-3.0 | Attribution + share-alike; watch 3.0/4.0 compatibility when mixing. |
| Art, 11 entries | CC-BY-4.0, CC-BY-3.0, Unsplash | Attribution, varying terms. |

Counts are from [copyright](copyright) (387 licence blocks total).

**If the goal is a closed-source commercial product**, neither the engine nor
Michael Zahniser's ship art can be used, and the premise collapses - what is left
is a from-scratch game that happens to be inspired by Endless Sky.

**If GPL-3 plus an open art repository is acceptable**, proceed. Commercial sale
of GPL software is well established, including on Steam.

### This also ends the upstream relationship

Everything in [CLAUDE.md](CLAUDE.md) about staying mergeable with
`endless-sky/endless-sky` stops applying the day this starts. Start a **new
repository** rather than diverging `animagr/endless-sky`, and accept that active
upstream development - fixes, balance, new regions - stops flowing to you.

---

## 3. What you inherit

This is the entire argument for starting here rather than from scratch.

**Content** (counted from `data/`):

| Definition | Count |
|---|---|
| Systems | 694 |
| Planets | 619 |
| Ship hulls | 917 |
| Outfits | 937 |
| Fleet templates | 223 |
| Governments | 128 |

Plus 347 MB of art and 45 MB of audio. A hand-built galaxy on this scale is
several years of authoring work.

**Engine:**

- Renderer, physics, collision detection, projectile and damage model.
- A text DSL (`DataFile` / `DataNode` / `DataWriter`) that already serves game
  content, plugins, save files and preferences through one parser.
- Save/load, accounts, depreciation, reputation, scanning, boarding and capture.
- A `UI` / `Panel` stack for the entire interface.

**Tactical AI, which is genuinely good.** `AI.cpp` already implements
`DoMining`, `DoSurveillance`, `DoPatrol`, `DoSwarming`, `DoHarvesting`,
`DoCloak`, `DoScatter`, `DoAppeasing`, `MoveInFormation`, `AimTurrets` and
predictive `AutoFire`, driven by ~30 personality traits
([Personality.h](source/Personality.h)). **You are not writing combat AI.**

**What is missing:** strategic AI. No agent decides production, logistics, or
expansion. That is the core of Phase 1.

---

## 4. What you delete

- **2330 mission definitions**, and most of `data/human/` (3.5 MB of campaign
  content).
- The fixed war clock ([data/human/campaign events.txt:40](data/human/campaign%20events.txt#L40)).
- Story-gated starts in [data/starts.txt](data/starts.txt), replaced with sandbox
  starts.

**One caveat that is easy to get wrong:** do not delete the job-board generators
along with the campaign chains. Procedurally generated jobs are the early-game
economy and the cheapest content in the project. Strip the authored storylines,
keep the generators, then extend them.

---

## 5. Phase 1 - one ship, a living galaxy

Six workstreams, in rough dependency order.

### 5.1 Persistent population

A new `GalaxyPopulation` class owning ships across all systems. Stop clearing
`ships` in `Engine::Place()`. Spawn and despawn against a per-system target
population instead of against proximity to the player.

*Moderate difficulty. The physics already works; this is bookkeeping and
lifetime management.*

### 5.2 Level of detail

**The load-bearing piece, and the main schedule risk.** Three tiers:

| Tier | Where | Cost |
|---|---|---|
| Full | Player's system | Current behaviour: physics, collision, weapons |
| Kinematic | Adjacent systems | Movement and AI, no collision or projectiles |
| Statistical | Everywhere else | A ship in transit is a timer and a manifest, not a position |

X4's attention levels are the right reference for the semantics, including the
deliberate asymmetries (combat resolved statistically, docking not blocked by
collision). Expect to tune this repeatedly.

### 5.3 Economic agents

NPC traders that genuinely read `System::Supply`, buy, fly, sell, and write the
result back, so supply numbers move because ships moved them. This is what makes
the economy feel alive. Today the player's `purchases` map is the **only demand
sink in the galaxy** ([GameData.cpp:422](source/GameData.cpp#L422)).

### 5.4 Economy rewrite

Add production and consumption chains to `System::Price`, and a consumption pass
to `StepEconomy`. Widen the `erf` band: today a commodity price can only travel
**+/- 100 credits** from a hand-authored base
([System.cpp:1307](source/System.cpp#L1307)), so a shortage can never spike a
price.

*Small diff, disproportionate effect. Both functions are about 15 lines today.*

### 5.5 Persistence

Extend the save format to cover galaxy state. `Ship::Save`
([Ship.cpp:906](source/Ship.cpp#L906)) and `GameData::WriteEconomy` already
exist - this widens their scope rather than inventing a format.

### 5.6 Strategic AI

Faction-level agents that build, expand, raid, and lose territory, driven off the
same daily tick the economy already uses
([Engine.cpp:1537](source/Engine.cpp#L1537)). Plus procedural job generation to
replace the deleted mission content.

**Rough scale for Phase 1:** 6-12 months for one experienced C++ developer to
reach genuinely good. Sections 5.1 and 5.4 alone could produce a playable
prototype in 4-6 weeks.

---

## 6. Phase 2 - fleets and automation

Depends on Phase 1's LOD existing. Without tier-3 statistical simulation,
automated ships have nowhere to run.

- **Standing orders.** Extend [Orders::Types](source/orders/Orders.h#L33) with
  `TRADE_ROUTE`, `MINE`, `PATROL`, and make orders survive a system change.
  Today all 11 order types are manual, in-system, and cleared on transition. The
  order system was recently factored into [source/orders/](source/orders/), so
  the extension point is cleaner than it once was.
- **Off-screen work resolution.** An automated miner should earn while the player
  is elsewhere. This falls out of tier-3 LOD almost for free.
- **A fleet management UI.** Endless Sky has no such panel; in X4 the map *is*
  the game. This is a bigger job than it sounds and is the most commonly
  underestimated item in this document. Budget real time.
- Optionally, crew skill and training, if X4's pilot-progression layer is wanted.

**Rough scale:** 3-6 months, the majority of it UI work.

---

## 7. Phase 3 - structures and ownership

The most net-new code, because almost nothing here has an existing analogue.

- **A constructible `Station` entity.** Today there is `Planet` and
  `StellarObject`, both immutable universe furniture. You need something
  ownable, damageable, and extensible with modules.
- **Construction, modules, production chains, storage and managers.**
- **Territory ownership and defence.** Partly reachable through the existing
  `government` and `tribute` machinery ([Planet.h:167](source/Planet.h#L167)).
- **New art.** Endless Sky ships no station-module sprites. This is the only
  phase with a hard art dependency as well as a code one.

**Rough scale:** 4-8 months, plus art production.

---

## 8. Risks

### 8.1 Performance is the real unknown

[AI::CacheShipLists()](source/AI.cpp#L5122) is O(governments^2 x ships) and runs
every frame. With 128 governments defined this is already the hot spot, and it
will fall over well before reaching X4-like ship counts. Plan to rewrite it with
spatial or per-system partitioning during 5.2, and expect LOD tuning to consume
more time than scheduled.

Secondary concern: `AI::Step()` currently does whole-list scans for targeting and
fence counting. Those all need system bucketing once the population grows.

### 8.2 Permanent loss of upstream

See section 2. Endless Sky is actively developed; forking for real means every
future fix and content addition has to be ported by hand or forgone.

### 8.3 Scope creep between phases

Phase 2's fleet UI and Phase 3's station art are both classic underestimates.
Each phase should ship as a playable game on its own before the next begins.

---

## 9. First milestone: the vertical slice

Do **not** start Phase 1 in written order. Build a slice that kills the
performance question first, because it is the only risk that can invalidate the
entire plan.

1. Pick roughly 20 systems.
2. Stop clearing `Engine::ships`; keep the population persistent across takeoff.
3. Implement **tier-3 statistical LOD only** - skip the intermediate kinematic
   tier for now.
4. Wire roughly 200 trader agents that actually move supply numbers.
5. Measure frame time. Then measure it again at 2000 ships.

**Two to four weeks.** If frame time holds, the three-phase plan is credible and
its foundation is already built. If it does not, that has been learned in week 3
instead of month 8, for a fraction of the cost.

---

## 10. Code touchpoints

| What | Where |
|---|---|
| Ship list cleared every takeoff | [Engine.cpp:317](source/Engine.cpp#L317) (`ships.clear()`) |
| Full physics on all ships, any system | [Engine.cpp:1948](source/Engine.cpp#L1948) |
| System gate (weapons/launch only) | [Engine.cpp:2021](source/Engine.cpp#L2021) |
| AI runs on all ships, no system filter | [AI.cpp:720](source/AI.cpp#L720) |
| Collisions scoped to player system | [Engine.cpp:2047](source/Engine.cpp#L2047) |
| Fleet spawning, player system only | [Engine.cpp:2062](source/Engine.cpp#L2062) |
| O(gov^2 x ships) per frame | [AI.cpp:5122](source/AI.cpp#L5122) |
| Daily economy tick | [GameData.cpp:422](source/GameData.cpp#L422), called [Engine.cpp:1537](source/Engine.cpp#L1537) |
| Per-system production | [System.cpp:1101](source/System.cpp#L1101); constants [System.cpp:38](source/System.cpp#L38) |
| Price formula (+/- 100 credit band) | [System.cpp:1307](source/System.cpp#L1307) |
| Economy already persisted | [PlayerInfo.cpp:5226](source/PlayerInfo.cpp#L5226) |
| Ship serialization | [Ship.cpp:906](source/Ship.cpp#L906) |
| Escort order types (11, all manual) | [orders/Orders.h:33](source/orders/Orders.h#L33) |
| Tactical AI behaviours | [AI.h](source/AI.h) (`Do*` methods) |
| Personality traits | [Personality.h](source/Personality.h) |
| Asset licences | [copyright](copyright) |
| Engine architecture notes | [source/CLAUDE.md](source/CLAUDE.md) |
| Content format notes | [data/CLAUDE.md](data/CLAUDE.md) |

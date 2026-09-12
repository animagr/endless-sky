# Endless Sky Master Guide

A consolidated guide to **Endless Sky**, built for the overlay panel. It runs from the loan you sign before you own a ship, through flying, trading, outfitting and combat, and out the far end into capturing alien warships.

Endless Sky is a free, open-source 2D space sandbox. There is no XP, no skill tree, and no level. **Your ship is your character sheet.** Everything you can do is decided by what hull you fly and what you have bolted into it, so almost every decision in this guide is really an outfitting decision.

Sources consolidated here: the official **Player's Manual** (endless-sky GitHub wiki), the developer's **Official FAQ** (Michael Zahniser, Steam), the Steam guide **A Real Beginner's Guide** (TOPPEST HONK and Drakkon), and the reddit quickstart **Endgame Fleet in 90 Days**. Every number below was checked against the **Endless Sky 0.11.3 source** in the local checkout at C:\Claude\Testing\endless-sky — starts and loans from data/starts.txt, ship and outfit stats from data/human/ships.txt and data/human/outfits.txt, rules from data/gamerules.txt, keys from keys.txt, and the mechanics themselves from source/Ship.cpp, source/CaptureOdds.cpp, source/Depreciation.cpp, source/PlayerInfo.cpp and source/Politics.cpp.

The four source guides were written for versions 0.9.12 to 0.9.13. A lot has changed. Where an old guide gives a figure that no longer matches the code, this guide uses the current value. See **Corrections Against The Older Guides** for the full list of what changed.

## How To Use This Guide

Read The First Hour, then play. Come back to the deep sections when you hit the problem they solve.

- Bleeding money and not sure why? Read Time, Money, And The Loan.
- Stranded with no fuel? Read Hyperspace Travel And Fuel.
- Losing fights you think you should win? Read Combat, then Outfitting Your Ship, in that order.
- Want a bigger ship without grinding for it? Read Boarding, Plundering, And Capturing.
- Want to skip the entire early game? Read Fast Progression: The Capture Route.
- Can't find the story? Read The Story.

## What Endless Sky Is

You are a starship captain in the year 3013. Human space spans roughly a hundred star systems connected by fixed hyperspace links, plus alien regions beyond it. The galaxy runs without you: merchants fly trade routes, pirates raid them, militia and Navy patrols fight the pirates, and all of that happens whether or not you are in the system.

The game is single-player and free, with no in-game purchases. Nothing you do to any ship makes a real person angry.

There is one hard failure condition: **if your flagship is destroyed, you are dead and the game is over.** There is no escape pod and no respawn into another ship you own. Everything else is recoverable.

## Controls

Key bindings are shown and rebindable under Preferences in the main menu. Nearly every clickable button also has a shortcut letter — hold Alt or Option and underlines appear under the shortcut letter for each button.

### Navigation Keys

- Forward thrust: Up arrow
- Turn left: Left arrow
- Turn right: Right arrow
- Reverse: Down arrow
- Stop ship: Shift + Down
- Fire afterburner: A
- Auto steer: E
- Land on planet or station: L
- Initiate hyperspace jump: J
- Jump as a fleet: Shift + J

### Fleet Command Keys

- Deploy or recall fighters: D
- Fight my target: F
- Toggle hold fire: Y
- Gather around me: G
- Change formation: Shift + G
- Hold position: H
- Toggle ammo usage: U
- Harvest flotsam: Z
- Scan my target: not bound by default

### Targeting Keys

- Select nearest hostile ship: R
- Select nearest ship of any kind: Shift + R
- Select next ship: N
- Select next escort: Shift + N
- Talk to selected ship: T
- Talk to selected planet: Shift + T
- Board selected ship, or cycle boarding target: B
- Board disabled escort: Shift + B
- Select nearest asteroid: V
- Scan selected ship: S

### Weapon Keys

- Fire primary weapons: Tab
- Select secondary weapon: W
- Fire secondary weapon: Q
- Toggle cloaking device: C
- Mouse turning, hold: Left Alt
- Toggle turret tracking: not bound by default
- Turret aim override, hold: not bound by default

### Interface Keys

- Main menu: Escape
- Star map: M
- Player info: I
- Toggle fullscreen: F11
- Toggle fast-forward: Caps Lock
- Pause: P
- Show help: F1
- Message log: /
- Performance info: F3

Escape or Ctrl+W closes most dialogs and panels.

### Mouse And Fleet Shortcuts

- Select ship, planet, or asteroid: left click
- Board a ship or land on a planet: double click
- Zoom in or out: minus and plus, or scroll wheel
- Select escorts inside a box: left click and drag
- Order escorts to a location, ship, or asteroid: right click
- Assign a group hotkey to selected escorts: Ctrl + number key
- Select that group later: number key
- Add that group to the current selection: Shift + number key
- Get every escort ready to jump: hold the jump key, then release to jump together

### Quantity Multipliers

These work in the Trading panel, the Hire Crew panel, the Shipyard, and the Outfitter.

- Shift: 5x
- Ctrl or Command: 20x
- Alt or Option: 500x

They combine. Shift plus Ctrl is 100x. All three together is 50,000x. The text above the buy and sell columns updates to show the current multiplier, so you can confirm before clicking.

### Outfitter And Shipyard Letter Keys

The outfitter's letter keys try several things in order, which is what makes them fast:

- B, Buy: buy from the outfitter and install on the selected ship
- S, Sell: sell from cargo first, then from planetary storage, then off the ship
- I, Install: install from cargo first, then from planetary storage
- U, Uninstall: uninstall from the ship into cargo, overflowing into storage
- C, Cargo: transfer from planetary storage into cargo, or buy straight into cargo
- R, Store: move from cargo into planetary storage, or uninstall from the ship into storage

In the shipyard: B buys a ship, S sells the ship with its outfits, and R sells the ship but keeps its outfits in planetary storage. R is the one you want when trading up — it saves you from selling a rare outfit along with the hull.

### Map Panel Keys

- Search for a planet, system, ship, or outfit: F
- Rotate jump destination: J or Tab
- Add a jump destination: Shift + J or Tab
- Compare two ships or outfits: Shift + click
- Ports view: P
- Missions view: I
- Outfitter view: O
- Shipyard view: S
- Starry view: T

## Starting A New Game

### The Five Starting Scenarios

Current versions offer several starts, not just the classic one. They differ in where you begin, how much money you have, and how badly the loan hurts.

- **Endless Sky (default)**: New Boston in Rutilicus, Dirt Belt. 480,000 credits, matched by a 480,000 mortgage at 0.4% daily. Credit score 400. This is the intended experience and everything below assumes it unless stated.
- **Deep Desire**: Midgard in Aludra, the Deep. 680,000 credits against a 680,000 mortgage at 0.2% daily. More money and half the interest rate, but the Deep is isolated and its shipyards sell different stock.
- **Birthday Surprise**: Mainsail in Alhena, the Paradise Worlds. 500,000 credits and **no loan at all**. Credit score 600. This is the easy start — no daily mortgage payment eating your income.
- **Escaping Squalor**: Delve in Scheat, Syndicate space. 480,000 credits against a 600,000 mortgage at 0.5% daily, credit score 200. The hardest start: you owe more than you were given and the rate is punishing. You will pay more in interest than the loan itself if you ride it out the full year.
- **Hai Origins**: locked. It only unlocks after you complete the main human campaign.

If this is your first game, take the default. If you have played before and want to skip the debt spiral, take Birthday Surprise.

### The Default Start And Its Loan

Before you own anything, you sign for 480,000 credits over one year at **0.4% daily interest**. That works out to **2,503 credits per day**, and 433,567 credits in interest if you ride the loan the whole year.

That daily payment is the single most important number in your early game. Everything you do for the first few in-game weeks is measured against it.

Check the Bank panel on any planet to see your principal, credit score, interest rate, and daily payment. You can pay down principal early, and you should when you can spare it — but leave a reserve, because missing a payment costs you five credit score points and adds the interest to your balance.

### Choosing Your First Ship

Three ships are for sale at New Boston. There is no wrong answer, but they play very differently.

### Shuttle

- Cost: 180,000 credits
- Shields 500, hull 600
- 6 bunks, 20 tons cargo, 400 fuel
- Outfit space 120, weapon capacity 10, engine capacity 60

The cheapest and most flexible option. Nearly as nimble as a fighter, so it outruns most pirates and can dodge missiles. Weapon capacity of 10 means it effectively cannot carry a gun, so it is useless in a fight — but it does not need to fight. Six bunks makes it the best passenger hauler of the three, and passenger jobs pay well. The 400 fuel is four jumps, the largest starting tank.

The hidden strength of six bunks: it is enough crew to attempt boarding actions later, which is what the capture route builds on.

### Star Barge

- Cost: 190,000 credits
- Shields 600, hull 1,000
- 3 bunks, 50 tons cargo, 300 fuel
- Outfit space 130, weapon capacity 20, engine capacity 40
- Comes with an Anti-Missile Turret installed

The freighter. 50 tons of cargo is two and a half times the Shuttle's, which means more simultaneous delivery jobs and real trade runs. It is slow and turns badly, and pirates specifically like eating freighters. The stock anti-missile turret is genuinely useful — it will not kill anything, but it shoots down incoming missiles while you run.

This is the most reliable way to pay off the loan without taking risks.

### Sparrow

- Cost: 225,000 credits
- Shields 1,400, hull 300
- 3 bunks, 15 tons cargo, 300 fuel
- Outfit space 130, weapon capacity 25, engine capacity 40
- Comes with two Beam Lasers, and has four gun ports

The combat option, and the developer's stated advice is that it is for players who already know the game. High shields, thin hull, so it wants to break off and recharge rather than trade hull damage. Four gun ports but only 25 weapon capacity, so you choose between many tiny guns and one real one.

The safe way to earn money in a Sparrow is not pirate hunting — it is mining asteroids.

### The Intro Missions

Whichever ship you pick, an old man at the spaceport asks for a ride to his retirement. Following his prompts walks you through delivery and passenger missions and teaches the basic loop. New players should take it.

You can decline. He occupies a bunk, which matters if you are planning boarding actions immediately. If you fail one of the intro missions you may be offered a second chance; later story missions generally are not so forgiving.

### Saving And Snapshots

- Your game saves automatically **every time you take an action while landed**.
- The game keeps the three most recent previous saves, reachable via Manage Pilots.
- Quitting while landed saves your progress. Quitting while in flight loses everything since you last took off.
- A **snapshot** is a manual save that is never overwritten or deleted. Make one from the Load / Save panel before buying a ship, before a risky mission, and before any irreversible story choice.
- A special snapshot named **autosave** updates whenever you hit a new main-story milestone.

### Where Saves Live

- Windows: C:\Users\yourusername\AppData\Roaming\endless-sky\saves\
- Linux: ~/.local/share/endless-sky/saves/
- macOS: ~/Library/ApplicationSupport/endless-sky/saves/

## The First Hour

### Your First Ten Minutes

1. Pick a ship. Star Barge if you want a steady start, Shuttle if you want speed and bunks.
2. Open the **Job Board (J)** while still landed. Take every job you can carry that goes the same direction.
3. Buy a **Local Map** from the outfitter for 1,000 credits. It is the cheapest thing in the game that stops you being lost.
4. Depart (D). Note that departing consumes one in-game day.
5. Press M, click your destination, press M again to close, then press J. Autopilot flies and jumps for you.
6. Land with L. Landing on an inhabited world refuels and repairs you for free.
7. Turn in your jobs, take new jobs going somewhere else, repeat.

That loop pays the mortgage. Do it until you have a cushion.

### Reading The Cockpit

Down the right side of the screen:

- A ring of **blue** around your ship outline is shields.
- A ring of **gold** is hull. When hull gets low you become disabled, not destroyed.
- Below that are three bars: **fuel** (gold, broken into jumps), **energy**, and **heat**.
- Below those are ammunition counters for any secondary weapons.

Each break in the fuel bar is one jump. Learn to read that at a glance — it is what keeps you from stranding.

### The Radar

Hollow circles are planets, stations, and stars. Filled circles are ships. Circle size reflects object size. Small white dots are missiles; smaller projectiles and beams are not shown.

Colours tell you attitude:

- Green: yours, including your escorts
- Blue: friendly — planets that will let you land, ships not hostile to you
- Yellow: hostile but currently busy attacking someone else
- Red: attacking you, or a planet that will not let you land
- Grey: disabled ships, and planets you cannot land on

Grey is the colour of opportunity. A grey ship is a boarding target.

## Time, Money, And The Loan

### How Days Pass

Time only advances when you do two things:

- Take off from a planet: one day
- Make a hyperspace jump: one day

Flying around inside a system costs no time at all. You can spend an hour of real time dogfighting in one system and the calendar will not move.

This makes rush missions exactly computable. Count the jumps on your route, add one day for departing, and add one more day for every stop you need to make to refuel.

### The Mortgage

The daily payment comes out automatically when you have the money, and your credit rating ticks up one point. If you cannot pay, the day's interest is added to your loan and your credit score drops by five.

Paying down principal early is usually correct once your income is stable, because it cuts the interest you pay for the rest of the year. The one reason not to: you cannot fly on a paid-off loan, and a stranded captain with no reserve is worse off than one carrying debt.

### Credit Score And New Loans

- The game tracks your net worth over the last 100 days of game time to estimate your daily income. That determines how large a loan you can qualify for.
- Your credit score sets the interest rate you are offered.
- Score goes up one per payment made, down five per payment missed.

If you plan to take a second loan later to buy a big ship, keeping a small balance alive and paying it faithfully builds the score for you.

### Crew Salaries

Every crew member except you costs **100 credits per day**. Three specific rules matter:

- Extra crew above the required minimum only cost you money **on your flagship**. Escorts are charged only for their required crew.
- **Parked ships cost nothing.** Parking is the fix for a ship you want but cannot currently afford to run.
- Destroyed ships stop costing you, which is not much consolation.

A small fleet is a permanent daily drain on top of the mortgage. Buy the second ship when the income justifies it, not because you can afford the sticker price.

### Depreciation

Ships and outfits lose value with age. The current numbers:

- There is a **7 day grace period** during which an item still sells for full price.
- After that, value decays daily and bottoms out at **25% of original value**.
- It reaches that 25% floor at **1,000 days** of age, a little under three years.
- A one-year-old ship sells for roughly **41%** of what you paid.

Two consequences worth internalising:

- **Plundered outfits and captured ships sell at the 25% floor**, always. A captured Bastion is not 1.3 million credits in your pocket; it is around 325,000. Captured hulls are worth far more flown than sold.
- If you are going to trade up, do it early. Flipping a ship inside the 7 day grace window costs you nothing at all.

## Making Money

### Job Board Missions

This is the best early income by a wide margin, and the developer says so directly. The rules:

- Take **multiple jobs at once**, all going the same direction. Ideally all to the same planet.
- Payment scales with distance. Farther is better paid.
- **Rush missions and tourists pay extra.**
- Clicking a job shows its destination on the map even in systems you have not explored.
- Yellow map pointers are available jobs; the pointer dims if you lack the cargo or bunk space. Accepted jobs turn blue and stay visible on your map.

The whole skill here is route packing: fill every ton and every bunk with work that terminates in the same place.

### Trading

Buy low, carry, sell high. The trading panel shows each good's price and an indicator of whether that price is high or low for the galaxy.

- Goods like clothing, plastic, and electronics vary a lot system to system but within a narrow band. Good for short hops.
- Goods like food, medical supplies, and heavy metals vary enormously by region but need long hauls to profit.
- Once you have visited a system your computer remembers its prices. Press M with a commodity selected and the map colour-codes systems by that commodity's price.
- Prices drift day to day, and dumping tens of thousands of tons on one planet depresses that price locally for a few days.

Trading is steadier than jobs but pays less per day. Most players run cargo as a supplement to a full job board, not instead of it.

The broad economic geography, useful for planning long hauls: the Core is resource-rich and mining-heavy. Syndicate worlds are industry and manufacturing. Earth is a permanent food sink for ten billion people. North of Earth are the Paradise Worlds, the luxury goods market, and beyond them the Deep, isolated and self-sufficient. South of Earth is the Dirt Belt, poor, where food and medicine are precious. Farther south is farm country, then the Rim with its old industry. Pirate worlds sit on the fringes at both ends.

### Mining Asteroids

Most asteroids are indestructible scenery. Minable ones behave differently: they orbit the star in a **clockwise elliptical orbit** rather than drifting in straight lines. Some are obviously coloured differently, but the most valuable ones look almost identical to ordinary rock — the orbit is the reliable tell.

- Shoot one until it breaks apart, then fly through the fragments to collect them.
- Weapons with high **hull damage** break asteroids fastest.
- Press V to select the nearest asteroid.
- Your Outfitters map view tracks which minerals you have harvested in each system.
- Press Z to tell your fleet to harvest flotsam, which saves a lot of manual collecting once you have escorts.

Mining is the safe way to make money in a combat ship, and it is entirely time-free — it costs no in-game days at all, only real time.

### Bounty Hunting

Bounty and escort jobs require a minimum **combat rating**, so you cannot take them at day one. Combat rating increases every time you disable an enemy ship. The formula is (target's cost + 250,000) divided by 500,000, so disabling anything at all gives you at least half a point and killing something expensive gives more.

The Player Info panel shows both a raw experience number and a rank, where the rank is the natural logarithm of the experience. The rank is what planets check when you demand tribute; the raw number is what job offers check.

The smallest bounty jobs need a raw rating above 2, the next tier above 7. A handful of pirate kills gets you started.

Modern bounty targets are **tracked**, so their current system shows on your map, and **marked**, so other NPCs leave them alone and they will still be alive when you arrive. Targets spawn one to three jumps from where the job was posted.

### Plundering And Capturing

Once you can afford a mid-tier ship, this becomes the most lucrative activity in the game by a large margin. It gets its own section below — see Boarding, Plundering, And Capturing.

### Tribute

With a high enough combat rating you can hail a planet (press L to target it, then Shift+T) and click **Demand Tribute**. The planet launches its defense fleet. Destroy the whole fleet, hail again, and demand tribute a second time — now they agree to pay you a daily income, and they will always let you land regardless of reputation.

Be aware of what this actually costs you now: demanding tribute registers as an **atrocity** against that government, not a minor offence. It provokes the defense fleets' governments and does lasting reputation damage. Dominating pirate worlds is nearly free because you had no reputation with pirates worth protecting. Dominating a Republic world is not.

## Flying

### Newtonian Movement

There is no friction. Once you are moving you keep moving until you thrust the other way. Every ship has two separate engine systems: **steering** to change facing, and **thrusters** to accelerate. To slow down you turn around and thrust.

Holding the Reverse key turns you to face the opposite of your current velocity — that is the fast way to set up a stop. Shift+Down is a full autopilot stop.

Some ships can mount reverse thrusters, but none of the starting three can.

You cannot collide with planets, stations, or asteroids; you pass above or below them. Projectiles do collide with asteroids, which is why asteroid fields are useful cover.

### Autopilot

The autopilot handles the three fiddly jobs:

- **L** flies to the nearest landable planet and slows for landing. Press it repeatedly to cycle between landable planets in system.
- **J** brings you to a stop and then engages the hyperdrive.
- **B** matches speed with a boarding target and closes to grapple range.

Any movement key immediately cancels the autopilot. For the hyperdrive that only works up until the drive actually engages — after that you are committed to the jump.

### Landing

Landing on an inhabited world **fully refuels and repairs your ship, for free**. Landing also shakes off all but the most determined pursuit, so a planet is an escape route as well as a shop.

## Hyperspace Travel And Fuel

### Setting A Course

Press M, click a destination. If it is connected by links you know, the shortest known route is drawn. If it is not, you have to explore toward it link by link.

Your sensors find the **location** of every system within a hundred light years by parallax, whether or not you know a link to it. That range is drawn on the map as a dark grey circle. So systems can appear on your map that you have no route to yet.

Press M again to close the map, then J to go. If your route has several hops, the autopilot keeps jumping until you arrive or run dry.

### Fuel Per Jump

- **Hyperdrive**: 100 fuel per jump. Costs 50,000 credits, takes 20 outfit space.
- **Scram Drive**: 150 fuel per jump. Costs 90,000, takes 30 space. It jumps faster — shorter spool-up — at the cost of half again the fuel.
- **Jump Drive**: 200 fuel per jump. Costs 1,000,000, takes 20 space. It ignores hyperspace links entirely and jumps to any system in range, which is what opens up the rest of the galaxy.

Hyperdrives and jump drives of the same type do not stack.

A **Fuel Pod** is 20,000 credits, 8 outfit space, and adds 100 fuel — exactly one more hyperdrive jump.

### Not Getting Stranded

Space stations do not sell fuel. **Only planets refuel you.** That is the trap that catches new players.

- Count the jumps before you commit. If your route passes through uninhabited systems, land and top off before you enter that stretch.
- If you have one jump of fuel left and you are exploring, land now. Do not gamble that the next system is inhabited.
- If you do get stranded: press Shift+R or N to select a friendly ship, press T to hail, and ask for help. Most captains will give a novice fuel.
- You can also occasionally get fuel from boarding a disabled ship, but do not plan around it.

### Ramscoops

A ramscoop pulls deuterium out of the solar wind and slowly refills your tank without landing.

- **Ramscoop**: 60,000 credits, 10 outfit space, rating 1.
- **Catalytic Ramscoop**: 320,000 credits, 16 outfit space, rating 7. It is a reward from a Free Worlds mission.

Two mechanical details the old guides get wrong:

- Collection rate scales with the **square root** of your ramscoop rating, so the Catalytic is about 2.6 times a basic ramscoop, not 7 times. Stacking basic ramscoops has heavy diminishing returns.
- **Every ship collects a trickle of fuel with no ramscoop at all**, provided it is close to the star. This is the "universal ramscoop" rule and it is on by default. It is slow, but hovering over the star in an empty system will eventually refill you.

Collection is dramatically better near the star and falls off with distance, so park close in when you are topping up.

### Jumping As A Fleet

Hold the jump key rather than tapping it. Your escorts line up and slow down; when you release, everyone jumps together.

The escort icons at the bottom left tell you the state: **yellow** means not ready yet, **red** means unable to jump — usually out of fuel.

Jumping as a fleet matters most when you are entering a hostile system. If you jump alone, you arrive alone and every enemy in the system concentrates fire on you before your escorts trickle in.

Keep your escorts' fuel capacity roughly in line with your own. If they cannot follow, your fleet arrives in pieces. Note also that when refueling, **your escorts are topped off first** — so if you have skimped on your own tank you can end up the one left short.

## Outfitting Your Ship

Most of the strategy in Endless Sky lives here, not in your flying.

### The Three Space Limits

Every hull has three separate caps, and they are the reason you cannot simply bolt the best of everything on:

- **Outfit space**: the total budget for everything.
- **Weapon capacity**: a sub-limit on weapons only.
- **Engine capacity**: a sub-limit on thrusters and steering only.

A Sparrow has 130 outfit space but only 25 weapon capacity, so downgrading its engines does not let it carry two particle cannons. The weapon cap is the binding constraint, and no amount of freeing up general space changes it.

Three outfits trade one kind of space for another:

- **Cargo Expansion**: 30,000 credits, +15 cargo, -20 outfit space
- **Outfits Expansion**: 50,000 credits, +15 outfit space, -20 cargo
- **Bunk Room**: 40,000 credits, +4 bunks, -20 outfit space

Bunk Rooms are the key outfit for anyone planning to capture ships, and Outfits Expansions are how a cargo hauler becomes a warship.

### Power Sources

A generator makes energy; batteries store it so you can briefly draw more than you generate, such as during a weapon salvo.

- Beam weapons are the exception that needs no stored energy. Everything else does, so a ship with no battery cannot fire burst weapons at all.
- **Engines take priority over weapons.** If you are short on power, you lose guns before you lose thrust.
- Overpowered engines with an underpowered generator is the classic rookie mistake. Once your batteries drain you may not be able to steer and thrust at the same time.
- If you can afford it, favour a strong generator over big batteries. Batteries run out and then you have to leave the fight; a generator does not.

**Recharging shields draws energy** — one unit of energy per unit of shields. A ship that flies fine at full shields can brown out while recovering from a hit. Budget for that.

### Engines

- **Ion drives**: energy-efficient, available almost everywhere, older technology.
- **Plasma drives**: space-efficient but hotter. Made by Delta V Corporation in the galactic south, hard to find elsewhere.
- **Atomic engines**: sold on a few Deep worlds. Powerful, expensive, and very energy-hungry.

One large thruster is always more efficient than two small ones summing to the same size. You can mix types freely if you want to.

Steering and thrusters are bought separately, so you choose your own balance:

- Mostly forward guns? You need steering, because you have to point at things.
- Mostly turrets? Trade steering for top speed.

Acceleration and turn rate both depend on mass, so a full cargo hold makes you sluggish. This is why a loaded freighter is such a good pirate target.

### Hull And Shields

**You cannot increase your shield capacity.** The shield matrix is built into the hull, so the only way to have more shields is to buy a stronger ship. Shield generators only control how fast the matrix **recharges**, which is still valuable — a strong regenerator can hold your shields up under sustained blaster fire.

Hull is the layer under shields. It does not regenerate on its own without specific outfits.

### Weapons: Guns Versus Turrets

Each hull has a fixed number of gun ports and turret mounts. What you put in them is up to you.

- **Guns** fire in the direction your ship faces. This includes homing missiles, which launch forward and then track. Fixed guns are angled slightly inward so their fire converges near maximum range.
- **Turrets** rotate freely and lead their targets automatically. With no target selected they just point forward — or, if you set turret firing mode to opportunistic in the settings, each turret independently tracks whatever it can most easily hit. Focused fire mode makes them all follow your selected target.

**Anti-missile systems** occupy a turret mount and cannot hurt ships at all. They fire automatically, only when missiles are in range. Sacrificing a turret for one feels wasteful right up until a missile boat is bombarding you from outside your weapons range.

Missile design has adapted: some missiles, notably the Sidewinder, carry ablative armour that makes them harder to shoot down. And ships with several identical launchers fire in **salvos** rather than one at a time, specifically to saturate anti-missile defences.

If your guns are not firing at all, check three things: do you have a battery, is the weapon a secondary that needs to be selected with W and fired with Q, and is it an anti-missile turret that only fires at missiles.

### Heat

Space is cold but it is also a vacuum, so your only way to shed heat is radiating it. Exceed your maximum temperature and **your ship shuts down until it cools**, which in combat means you die.

- Weapons generate far more heat than anything else, so a ship that runs cool in transit can overheat thirty seconds into a fight.
- When outfitting, read the combined heat figure for engines and weapons firing together, not the individual numbers.
- Test-fly a new build: go to an empty corner of a safe system and hold down thrust and fire at once for a while. If it overheats there, it will overheat in combat.

Heat is also a weapon. Pirates run their ships hot as a rule, and heat-dealing weapons can tip them over the edge. Heat weapons work best on targets whose shields are already down.

### Ammunition And Secondary Weapons

Weapons that consume ammunition do not fire with your primary key. Select the weapon with **W**, fire it with **Q**. This is deliberate — it stops you burning expensive missiles on a Sparrow.

- Missiles can be destroyed by anti-missile systems or by hitting asteroids.
- Against a target with anti-missile, fire at close range so their defences have less time to react.
- Do not do that with heavy rockets or anything with a blast radius, or you will damage yourself.
- Your escorts default to only firing secondaries when the enemy fleet outguns yours. Change that in preferences or with the toggle ammo usage key, U.

### Fighters And Drones

Some hulls have fighter bays or drone bays. Bays ship empty; you buy the fighters separately.

- Neither fighters nor drones have hyperdrives. You must recall them (D) before you jump, or they get left behind.
- **Drones** need no crew. **Fighters** cost one crew salary each.
- You cannot use a fighter or drone as your flagship.
- If you do leave one behind, go back for it.

### Outfitter Workflow

A few mechanics that save a lot of time once you have more than one ship:

- Shift-click or ctrl-click ship icons to select **several ships at once**, then buy or sell outfits for all of them simultaneously. This is how you kit out six identical escorts in one go.
- **Deselecting all ships** makes purchases go into cargo instead of installing.
- Selling an outfit keeps it in stock until you buy it back or take off.
- Pressing **U** puts an outfit into planetary **storage** instead of selling it. Storage is permanent and planet-specific.
- The checkboxes at bottom left control what the list shows: outfits on the selected ships, on sale, in cargo, and in storage.
- **Whenever you sell a ship, its outfits stay available for repurchase until you leave the planet.** This is the trick for moving a rare outfit onto a new hull.

That last point generalises into a genuinely useful exploit: to get an outfit that is not sold locally, buy a **ship** that comes with it preinstalled, move the outfit to the ship you actually want, and sell the empty hull. You lose the depreciation on a same-day flip, which inside the 7 day grace period is nothing.

You can hover the mouse over any attribute on any outfit or ship to get a tooltip explaining what it does.

## Combat

### When To Run

The best response to most fights is not to have them.

- Fly toward any government ship. They will shield you from pirates.
- Land on a planet. That shakes almost all pursuit.
- Jump out — but note you must be **nearly stationary** to engage the hyperdrive, and heavy incoming fire can keep knocking you off a stop.
- Hail the attackers (T) and offer a **bribe**.
- Jettison cargo from the ship info panel and run while they stop to scoop it up.

### Core Combat Principles

- **Divide and conquer.** Fighting four ships one at a time costs you far less than fighting four at once. Run away — the faster ships reach you first, and you fight them separately from the slow ones.
- **Focus fire.** Pick one target and kill it. Spreading damage across a group kills nothing.
- **Kill the weakest first**, unless you have something like an Ion Cannon that can remove the strongest ship from the fight outright.
- **Range wins.** If your weapons reach farther than theirs, sit at the edge of your range. A Sparrow with one Heavy Laser beats a stock Sparrow purely on reach.
- **Use asteroids as cover.** Projectiles that hit rock are destroyed. If you are hunting pirates and you carry no missiles yourself, pick an asteroid-heavy system to fight in — their missiles suffer and yours do not exist.
- **Avoid the system centre.** That is where ships jump in. Lead your target well away from it before starting, or reinforcements and militia will arrive mid-fight and steal your capture.
- **Speed is a stat.** A faster ship chooses whether to fight at all, can strafe outside a slower ship's gun arc, and can outrun missiles. Do not buy toughness at the cost of all your speed.

### Maneuvers

- **Strafing**: approach at full speed, pass the target at the right distance, then thrust continuously while keeping the nose on them. You settle into a circle around the target. If they turn slowly you can hold your forward guns on them while only their turrets can answer. Easiest with auto-aim on.
- **Blindside attack**: if you carry only turrets and they carry mostly fixed guns, and you out-turn them, just sit behind them. A Blackbird does this extremely well.
- **Jinking**: the AI leads its shots using your position **and** your velocity. Weaving unpredictably makes long-range fire — particle cannons especially — miss badly.

Automatic aiming and firing can be enabled in preferences, and for long-range weapons you will not hit much without it. Bear in mind that every AI ship has the same targeting algorithms you do, so flying skill alone will not beat a genuinely stronger ship.

### Fleet Battles

When you are one ship in a large battle:

- Concentrate your fire on whatever ship most of your allies are attacking.
- Hunt the missile boats loitering at the edges before they unload on your side.
- Act as a diversion and pull a chunk of the enemy fleet away.
- Or load up on anti-missile systems and screen your allies.
- You can fire **past** allied ships but enemies cannot fire past them, so a damaged ship can hide behind friends. An allied ship that is already exploding is a free shield — just do not be next to it at the end.

### Combat Rating

Rating rises when you **disable** an enemy ship, by (its cost + 250,000) / 500,000. Destroying without disabling gives nothing extra, so a disabling weapon loadout builds rating faster.

Rank, shown in Player Info, is the natural log of that total. Rank thresholds gate the tribute system; raw rating gates job offers.

## Boarding, Plundering, And Capturing

This is the highest-value activity in the game and the reason experienced players skip the trade grind entirely.

### Disabling A Ship

A ship becomes disabled somewhere between **50% and 10% hull**, depending on hull size — small ships disable at close to 50%, capital ships closer to 10%. A Shuttle disables around 40%; a 20,000-hull capital around 14%.

Once disabled it takes only a few more shots to destroy it, so stop shooting. Disabled ships are grey on radar and stay put.

Wait until the system is clear of hostiles, then come back and board at leisure.

### Boarding

Press **B** to board your current target, or the nearest disabled ship if your target is not disabled. Autopilot matches velocity and closes to grapple range for you. Shift+B boards a disabled escort.

Plundering a ship is a crime against **that ship's government**. If it is a pirate, nobody cares.

### Plundering

The safe option. You take cargo and any outfits accessible from outside — weapons, engines, generators.

- Items are listed by **price per ton**, most valuable first.
- You cannot install outfits in deep space, so plunder goes into your **cargo hold** and you need free tons for it.
- At the next outfitter, plundered items show as "in cargo". Click one to sell it, or install it for free since you already own it.
- Plundered outfits sell at the 25% depreciation floor. **Install them, do not sell them**, wherever you can.

### Capture Math

The risky option, and the profitable one. The boarding dialog shows your odds of capturing and your odds of defending if the attempt fails and their crew counterattack.

The arithmetic, straight from the source:

- Each attacking crew member contributes **1** strength.
- Each defending crew member contributes **2**. Defenders have their own ship as cover.
- Each crew member can wield exactly **one** hand-to-hand outfit, and the game assigns your best ones first. Ten laser rifles with three crew gives you the bonus from three rifles.
- Your odds of winning each round are your strength divided by the combined strength.
- The loser of each round loses one crew member.

Worked example from the FAQ, still accurate: four crew with four laser rifles gives (1 + 0.6) x 4 = 6.4 attack. A target with two crew has 2 x 2 = 4 defence. First-round odds are 6.4 / (6.4 + 4) = 61%.

The practical consequence: **attacking a ship with the same crew as yours is far worse than a coin flip.** You need a large crew advantage, hand-to-hand outfits, or both. This is why Bunk Rooms are the first thing a would-be pirate buys.

### Hand-To-Hand Outfits

Human-space options, with their attack and defence values:

- **Laser Rifle**: 8,000 credits. +0.6 attack, +0.8 defence. The all-rounder.
- **Fragmentation Grenades**: 17,000 credits. +1.3 attack, +0.3 defence. Offence specialist.
- **Nerve Gas**: 75,000 credits. +2.8 attack, +0.8 defence. The best human attack outfit, and illegal in many places.
- **Security Station**: 42,000 credits. +3.4 defence, no attack. Pure anti-boarding insurance for a flagship.

Alien space sells better ones — the Korath Repeater Rifle among them — which is one reason the capture route ends up out there.

### After A Capture

- Some of your crew must transfer over to fly the prize, so **you cannot capture anything if you are down to one crew member** — that is you.
- A captured ship is repaired only to **1.5 times its disable threshold**. One hit re-disables it. Keep it out of the fight until shields recover.
- An **undercrewed ship is not broken, it is slow.** Turn rate and acceleration both scale linearly with crew over required crew. A ship needing 5 crew and carrying 3 turns and accelerates at 60%. Older guides describe random control failures; that is no longer how it works.
- Landing on an inhabited planet automatically hires replacements up to the required minimum.
- Captured hulls sell at the 25% floor, so fly them or park them rather than cashing them out.

### Repairing Friendly Ships

If a friendly ship is disabled, select it and press B to board it. You repair it just enough to fly. They will sometimes pay you for the trouble. Shift+B does this for your own escorts.

If **you** are disabled, cycle to a friendly with N or Shift+R, hail with T, and ask for help. They can repair you, refuel you, or fight off your attackers.

## Fleets

### Why A Fleet Costs Money

A second ship is not free even if you captured it. Every required crew member on it costs 100 credits a day, forever, and unlike the mortgage there is no end date. Ten crew is 1,000 a day, which is close to half your starting mortgage payment.

Early on, keep crews small. Buy the second ship when a route or a mission justifies the running cost, not when you happen to have the cash.

Two small escorts are generally more cost-effective than one large ship, but small escorts die easily in fights that a heavier hull would survive without a scratch. And when an escort dies, its cargo and passengers die with it — including mission-critical cargo that would not fit in your flagship.

### Flagship

Your flagship is simply the **first ship in the list**. To change it, land somewhere with a shipyard or outfitter and drag the ship icons into a new order. You can also reorder from the Player Info panel while landed.

Choose the flagship on one criterion above all others: **survivability**. Everything else is replaceable. If the flagship dies, the run is over.

Do not fly an interceptor as your flagship unless it is all you own.

### Escort Orders

- **F**, fight my target: your whole fleet focuses the ship you have selected. This is the single most useful fleet key — it is how you stop a heavy warship from grinding you down while your escorts chase fighters.
- **G**, gather around me: escorts come to you. Press it again to release them.
- **Shift+G**: cycle the formation they gather in.
- **H**, hold position: escorts stop where they are. G releases them again.
- **D**: deploy or recall carried fighters and drones.
- **Y**: hold fire.
- **U**: toggle whether escorts spend ammunition.
- **Z**: harvest flotsam.
- **Right click**: order selected escorts to a location, ship, or asteroid.
- **Ctrl + number** assigns a group; the bare number selects it later.

### Parking

Land, press **I** for Player Info, click a ship, click **Park**. A parked ship stays on that planet, does not follow you, and **costs no salary**.

Park when:

- A ship is bleeding you dry and you have not found a route that pays for it.
- A mission requires you to arrive alone, or requires a target survive that your escorts would happily destroy.
- You want a specific small ship for a boarding run without your fleet's crossfire vaporising the prize.

There is a button to park everything at once, and per-ship buttons for selective parking.

You can also rename any ship: Player Info, click the ship, click its name in the top left corner.

You can reorder weapon hardpoints the same way — click a ship in Player Info while landed, then drag weapons. Useful for putting turrets forward and anti-missile aft so it covers you while you run.

### Transferring Outfits Between Ships In Space

There is no direct transfer, but there is a reliable workaround, and it matters enormously when you are plundering far from a friendly outfitter:

1. Gather the fleet with **G**, then head to the system's edge, away from traffic.
2. Stop with **Shift+Down** and order escorts to hold with **H**.
3. Select one escort and **right-click on the centre of your flagship**, so it parks itself directly on top of you.
4. Confirm that escort's cargo hold is empty.
5. Jettison the outfits from your flagship. The escort scoops them before they despawn.

Slow, but it lets a fleet of six ships share one flagship's worth of plunder.

## Reputation, Scanning, And Contraband

### How Reputation Works

- Simply **attacking** a friendly ship makes it and its allies hostile until you bribe them, leave the system, or land. That part is temporary.
- **Permanent** reputation change only happens if you **disable, board, or destroy** the ship. The size of the hit scales with how many crew were aboard.
- Attacking a ship with **no crew at all** — a drone — does no permanent reputation damage. This is why the Navy surveillance drone theft is survivable: disable a drone, board it, take the Surveillance Pod, leave the system, and the Republic calms down.
- Demanding tribute is an **atrocity**, in a different and much worse category.

### Scanning

To scan another ship you need the right hardware:

- **Cargo Scanner**: 8,000 credits. Shows what they are carrying.
- **Outfit Scanner**: 24,000 credits. Shows what is installed.
- **Tactical Scanner**: 144,000 credits.

Target the ship, fly close, hold **S**. Stay in range until it completes and a results dialog appears.

### Interference Plating

If you get scanned while carrying contraband, your chance of being caught is 1 / (1 + total interference).

**Interference Plating** costs 50,000 credits, takes 4 outfit space, and provides 0.5 interference each.

- 2 plates: interference 1.0, so a 50% chance of being fined.
- 6 plates: interference 3.0, so a 25% chance.

Diminishing returns are steep. Six plates costs 300,000 credits and 24 outfit space to go from 50% to 25%.

## The Story

### Finding The Main Campaign

There is one implemented main campaign: the **Free Worlds**. There is no Navy or Syndicate storyline, and no way to buy Navy ships — though you can capture them.

To find it: keep clicking the **Spaceport** button on planets in the southern half of human space — the **Rim**, the **South**, and the **Dirt Belt**. Eventually a Free Worlds mission is offered. Spaceport missions come from conversations, unlike the job board, so you have to keep visiting spaceports and checking.

Some conversations let you ask questions and make real choices. A few can end with your character dead, imprisoned, or enslaved. Read them.

### The War Clock

The civil war fires on a fixed date: **4 July 3014**. The game starts **16 November 3013**. That is exactly **230 days** of peacetime, and it happens whether you are ready or not.

That number is why the speedrun route below exists. Time only passes on takeoffs and jumps, so 230 days is a large but finite budget of actions, and a player who spends them efficiently can be flying alien capital ships before the war starts.

### Failing Missions

- Intro missions may offer a second chance.
- Later story missions usually do not.
- Take snapshots before anything irreversible.
- The **autosave** snapshot updates at each main-story milestone, so it is your fallback.

One specific trap: **buying a smaller ship can fail your active missions.** If your new ship has less cargo space or fewer bunks than the old one, passengers and mission cargo get dropped when you take off. Before trading down, check the Hire Crew panel for passenger count and the Trading panel for special cargo.

## Fast Progression: The Capture Route

This section is a condensed version of the reddit quickstart *Endgame Fleet in 90 Days*, updated for current ship names. **It contains heavy spoilers**, it requires a lot of save-scumming, and it deliberately uses out-of-character knowledge. Following it exactly is not fun for everyone. Read it as an illustration of what the capture mechanics allow.

### Why It Works

Normal progression means months of low-margin missions to pay off a loan and slowly upgrade. Capturing skips the money entirely. A ship you take is worth its full value **flown**, even though it only sells for 25%, and each captured ship can capture more ships. The curve is exponential rather than linear.

The clock helps too. Days pass only on takeoff and jump, so a route that minimises landings and jumps costs almost no calendar time no matter how long you spend fighting.

### What It Costs You

- Extensive save-scumming. Disabled prizes get destroyed by crossfire constantly.
- Repeatedly leaving and re-entering a planet to reroll system spawns.
- Hostile first contact with several alien species, out of order and out of context.
- Most of the story, which you will now play with an invincible fleet.

### Phase 1: Shuttle To First Warship

Fly a **Shuttle** — faster and cheaper than a Star Barge, and six bunks means it can be built for boarding. Optionally buy a Sparrow first, strip its plasma engines into storage, sell it, and buy the Shuttle: that gets you better engines than the Shuttle can normally have, and it demonstrates the buy-ship-for-its-outfits trick you will use repeatedly.

Head to **Glaze** in **Aldhibain**. Sell your shield generator and hyperdrive, buy Bunk Rooms and Laser Rifles, hire crew.

Aldhibain spawns southern pirates alongside militia and merchants, so pirates get disabled there constantly. Wait for merchants or militia to disable one, wait for the attackers to be distracted, then board. Once captured, immediately order the prize to gather on you, limp away from the system centre, tell it to hold position, and land to save.

A Shuttle can realistically take a **Clipper** this way — a ship that costs 1.3 million new. Sell the Shuttle, refit the Clipper for boarding, and go take something bigger, likely a **Bastion**. A Bastion with Outfits Expansions and Bunk Rooms holds around 100 crew.

### Phase 2: Shield Beetles In Hai Space

Stop at **Trinket** in **Sargas** for Fragmentation Grenades, and pay down the loan if you have spare cash — but leave a small balance alive so your credit score keeps climbing.

Then run for **Cloudfire** in **Wah Ki**, deep in Hai space. You will not be stopping to refuel, so you depend on hailing merchants and patrols for fuel. The empty systems near **Ultima Thule** are thick with pirates and thin on friendlies, so top off at **Nihal** by hovering over the star before crossing.

Wah Ki is a permanent bloodbath: Hai fleets, Hai and human merchants, and **Large Plundering Unfettered** raiders all spawn there. Stay at the system edge and pick off disabled **Unfettered Shield Beetles** that drift clear of the main fight.

A refitted Shield Beetle can capture two more per day. Six escorts is enough to beat essentially anything in human space. Capture a **single variant type** so your escorts can share one outfitting plan — the Pulse variant, two Ion Cannons and six Pulse Cannons, is a reasonable pick.

### Phase 3: A Big Hull

You want a large crew capacity and a large cargo hold to make plundering efficient. Two options:

- The **Bactrian** route. Bactrians normally need a license earned through a long side-mission chain, but they also spawn rarely in the **Large Northern Pirates** fleet, which is common at **Haven** in **Arneb**. Pirates will not let you land, so raise your combat rating to rank 8 by killing pirates, then demand tribute, beat the defense fleet, and dominate the planet — that grants permanent landing rights and a small daily income. Then save-scum: leave and re-enter over and over to reroll the spawns until a Bactrian appears. Lure it away from the fray, focus fire with F, and disable it without killing it.
- The **Rano'erek** route, which is simpler now. Skip Arneb entirely and go from Hai space to Alcyone, then take a Korath heavy freighter later in Phase 4.

At Haven you can also buy Heavy Lasers and Heavy Laser Turrets without detouring to the Deep — they come preinstalled on Headhunters and Aeries for sale locally. Buy the ship, move the weapons to your Beetles, sell the hull, repeat. Beam weapons are what you want on escorts, because they disable reliably instead of destroying your prizes.

### Phase 4: Korath Jump Drives

Take the fleet to **Stormhold** in **Alcyone**, dominate it, land, and buy Nerve Gas.

Your targets are in **Durax**, which has the highest spawn rate of the Korath raiding fleet in human space. Durax spawns **Korath Raid** — a **Palavret** escorted by two **'olofez** chasers — into an ongoing three-way brawl with core pirates and Syndicate patrols.

Korath ships try to jump out when their shields drop, so overwhelm them: focus your Heavy Laser Beetles on one at a time with F.

The prize is the **Jump Drive**, which ignores hyperspace links and opens the rest of the galaxy. Korath outfits generally are excellent — a Palavret can hold a **Triple Plasma Core** worth around 5 million, and Shield Beetles can carry off heat shunts, systems cores, and fuel processors. Plunder heavily on every run.

A fully crewed large hull with Nerve Gas can capture an entire Palavret, which comes with a pile of Korath Repeater Rifles — the best hand-to-hand gear you will have seen so far. With a Palavret in hand you can go take a **Rano'erek**, the Korath heavy freighter, from one of the nearby Korath Exile home systems.

### Phase 5: Automata

With jump drives installed, head north to **Mesuket**, which spawns **Large Kor Mereti** and **Large Kor Sestor** automata fleets.

Automata need **no crew at all**, so capturing them costs you nothing in salaries. Combat against them is suicide, so do not fight them in a warship — switch your flagship to a Shield Beetle, put its weapons into storage, and fill the freed space with systems cores for pure survivability.

Take as many **Kar Ik Vot 349s** or other automata as you can stand, then land at **Laki Nemparu** in **Kashikt** to install jump drives and buy a Command Center.

At that point you have one of the strongest fleets in the galaxy, well before the war starts, with the whole 230-day budget still mostly unspent.

### Name Changes Since That Guide Was Written

The reddit guide predates a Korath renaming pass. If you are following the original text:

- "Korath Raider" is now **Palavret**
- "Korath Dredger" is now **Rano'erek**
- The "Chaser" fighters are now **'olofez**
- "Diamond Regenerator" is the **Hai Diamond Regenerator**
- "Outfit Expansion" is spelled **Outfits Expansion**

Also, the Jump Drive now costs **200 fuel per jump**, not 150, which changes your tank planning. Everything else in the route — the systems, the planets, the fleet spawns — still checks out against the current data files.

## Corrections Against The Older Guides

The four source guides date from 0.9.12 and 0.9.13. Here is what has changed since, verified against the 0.11.3 source:

- **The starting loan is 480,000 credits, not 450,000.** The daily payment is 2,503, and a full year of interest is 433,567.
- **There are five starting scenarios now**, not one. Two of them change the loan substantially, and one has no loan at all.
- **The Star Barge ships with an Anti-Missile Turret.** The beginner's guide describes it as having "zero defensive armament."
- **The Shuttle has 6 bunks and 400 fuel**, which is four jumps — more than either alternative.
- **Scram Drives cost 150 fuel per jump**, not 125. **Jump Drives cost 200**, not 150.
- **Every ship collects fuel near a star even with no ramscoop.** The universal ramscoop rule is on by default.
- **Ramscoop output scales with the square root of the rating**, so the Catalytic Ramscoop is roughly 2.6x a basic one, not 3x or 7x.
- **Undercrewing scales turn rate and acceleration linearly** rather than causing random control failures. A ship at 3 of 5 crew handles at 60%.
- **Captured ships are repaired to 1.5x their disable threshold**, a slightly more generous rule than "just enough not to be disabled."
- **Depreciation bottoms out at 25% after 1,000 days**, with a 7-day full-value grace period at the start. A one-year-old ship fetches about 41%.
- **Bounty targets are tracked and marked.** Their system shows on your map, and other NPCs will not kill them before you arrive. They spawn one to three jumps out, not "always within two."
- **Demanding tribute is classified as an atrocity** against that government. The FAQ's breezy description of galaxy conquest understates the reputation cost.
- **Extra crew only cost salary on your flagship.** Escorts pay only their required crew.
- **The Korath ships have all been renamed.** See the list in the previous section.
- Fleet formations (Shift+G), flotsam harvesting (Z), escort group hotkeys, planetary outfit storage, and the outfitter's letter-key workflow are all newer additions the older guides do not mention at all.

## Common Mistakes

- **Flying into an uninhabited system on your last jump of fuel.** Stations do not refuel you. Only planets do.
- **Buying engines you cannot power.** Engines take priority over weapons, so an over-engined ship fights with no guns once its batteries drain.
- **Forgetting that shield recharge draws energy.** A build that flies fine at full shields can brown out recovering from a hit.
- **Not test-firing a new build.** Hold thrust and fire together in an empty system. If it overheats there, it will overheat in combat, and overheating means a total shutdown.
- **Trading down into a smaller ship with active missions.** Passengers and mission cargo get dropped on takeoff and the missions fail. Check the Hire Crew and Trading panels first.
- **Selling plundered outfits.** They go at the 25% floor. Install them.
- **Selling a ship with a rare outfit still installed.** Use R in the shipyard to retain outfits in planetary storage instead.
- **Fighting at the system centre.** Reinforcements jump in there. Militia will show up and destroy the pirate you were about to board.
- **Buying escorts before you can pay their salaries.** 100 credits per crew per day, forever, on top of the mortgage.
- **Boarding a ship with a similar crew count to yours.** Defenders are worth double. Without a large crew advantage or hand-to-hand outfits you will lose people and possibly the ship.
- **Not taking snapshots.** The autosave covers story milestones only. Everything else is on you.
- **Using an interceptor as your flagship.** If it dies, the game is over. Survivability beats every other stat on that one hull.
- **Ignoring the job board in favour of trading.** Stacked jobs going one direction outearn any trade route in the early game.

## Sources

- Endless Sky Player's Manual, endless-sky GitHub wiki
- Official FAQ by Michael Zahniser, Steam Community guide
- (WIP) Endless Sky: A Real Beginner's Guide by TOPPEST HONK, with fleet and fuel sections by Drakkon, Steam Community guide
- Endgame Fleet in 90 Days: A Quickstart Guide, r/endlesssky, with its own credits to Paying the Iron Price, Optimal Piracy and Korath Farming, and the automata farming threads
- Endless Sky 0.11.3 source and data files, local checkout at C:\Claude\Testing\endless-sky

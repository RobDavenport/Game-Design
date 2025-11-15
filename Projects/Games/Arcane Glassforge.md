# Arcane Glassforge

**Elevator pitch**  
You are a Glasswright who assembles crystalline circuits on a small glassboard. Each turn you draft and place tiles (crystals, lenses, cisterns). When you **Run** the board all tiles resolve at once — sparks flow, charges accumulate, and clean or catastrophic chain reactions happen. The joy is in building clever, visually satisfying engines under resource limits and risk.

**Target / Scope**  
Platform: PC (Steam / itch). Solo dev MVP. Session length: single runs ≈ 8–12 turns (10–20 minutes). Focus: compact, replayable runs with strong emergent combos and immediate visual payoff.

---

## Core Loop

1. **Draft** (choose 1 of 3 tiles / buy from shop)
    
2. **Place** (spend up to **3 energy** per turn to place tiles anywhere on a **5×5** board)
    
3. **Run** (press _Run_ → one tick: all-board-at-once resolution; triggers: tick-start → adjacency activations → threshold triggers → end-of-tick effects)
    
4. **Decide** (keep building, buy relic, or accept next node objective)  
    Repeat until run goal met or turn limit reached.
    

---

## Pillars (design goals)

- **Set → Submit → Observe:** No mid-resolution micromanagement.
    
- **Decision density:** ~2–4 meaningful choices/turn (placement + buy + relic use).
    
- **Emergence over content:** depth via combinations, not handcrafted levels.
    
- **Readable feedback:** every tile has a clear counter, glow and audio cue on activation.
    

---

## Board & Economy

- Board: **5×5** grid.
    
- Energy per turn: **3** (tiles have cost 1–2).
    
- Run length: default **10** turns (adjustable per node).
    
- Win condition: reach target score (node contract) OR survive X turns with a high score for meta unlock.
    

---

## Tile taxonomy (implement 12 core tiles first — data-driven)

(Values are starting numbers for tuning)

**Support (general)**

- **Spark Prism** (Generator) — cost 1. Produces **1 spark** each tick.
    
- **Crystal Cistern** (Buffer) — cost 1. Holds up to **3 sparks**; release on tick-end as 3 sparks to adjacent Consumers.
    
- **Shimmer Lens** (Amplifier) — cost 1. If adjacent to a Consumer, **+50%** score from that Consumer this tick.
    

**Consumers / Scorers**

- **Runic Vault** (Consumer) — cost 2. Gains **+1 charge** per adjacent activated tile; at **5 charges**, consumes charges → pays **10 points** and emits a visual burst (can chain trigger neighbors).
    

**Chain / Flow**

- **Facet Conduit** (Repeater) — cost 1. When any adjacent tile activates, has **40%** chance to retrigger a random adjacent tile this tick.
    
- **Splitter** (Routing) — cost 1. Alternates sending one incoming spark to two different neighboring tiles (round-robin).
    

**Risk / Spice**

- **Overload Shard** (Volatile) — cost 2. Doubles adjacent Consumer payout but adds **+2 heat** to board; at heat ≥ 6 triggers explosion removing one random adjacent tile.
    
- **Wild Prism** (Randomizer) — cost 1. 15% chance on activation to retrigger a random tile anywhere on the board.
    

**Timing**

- **Sequencer** (Phase) — cost 1. Activates every **2** ticks instead of every tick (useful for delayed combos).
    
- **Mirror Facet** (Reflect) — cost 1. When neighbor fires, reflect half the effect to opposite neighbor (static orientation variant for MVP: make two variants N–S / E–W).
    

**Utility**

- **Stabilizer Filigree** — cost 1. Each stabilizer reduces board heat by **1** at end-of-tick; needed for Overload engines.
    
- **Consumer X (low-tier)** — cost 1. Simple small-payout consumer for early turns.
    

> Tiles are data entries (type, cost, parameters). Implement behavior as tiny scriptable hooks (onTickStart, onNeighborActivate, onThreshold, onTickEnd).

---

## Relics / Meta (persistent modifiers)

Ship 6 relics in MVP (pick from below):

- **Bellows of the Guild** — Generators produce +1 spark on any Consumer activation.
    
- **Guild Seal** — +1 placement energy per run.
    
- **Ancient Shard** — once per run: convert any Buffer into an immediate big payout.
    
- **Glassblower’s Temper** — Stabilizers reduce overload chance extra.
    
- **Shard of Echoes** — first Facet Conduit retriggers are guaranteed (not chance).
    
- **Shimmering Lens** — first Amplifier each run is doubled.
    

Relics unlock via meta-progression (XP or currency from successful runs).

---

## UI / Feedback (MVP essentials)

- 5×5 grid with per-tile hover tooltip (name, cost, small description, current counters).
    
- Top bar: energy left, turn number, target score. Big **RUN** button (center).
    
- Per-tile radial timers or small numeric counters for charges.
    
- Visuals: crystal plane mesh + glyph icon; activation = glow + burst particle + small sound.
    
- Replay / seed export facility (simple seed code) for sharing.
    

---

## Prototype roadmap (8 weeks)

- **Wk1–2:** Core engine: grid, placement, JSON tile loader, energy mechanic, Run button with single tick applying Generator + Consumer simple pipeline.
    
- **Wk3:** Implement adjacency checks, Sequencer, Buffer behavior, basic scoring.
    
- **Wk4:** Add Overload, Facet Conduit, Wild Prism; basic particle + audio hooks; tile hover tooltips.
    
- **Wk5:** Shop/draft + relic system + run node manager (target scores).
    
- **Wk6:** Polishing UI, visual clarity, playtest tuning (iterate rules).
    
- **Wk7:** Add simple meta-progression (unlock relics), daily seed mode skeleton.
    
- **Wk8:** Internal QA, balancing, build Steam/itch prototype.
    

**Success criteria for MVP**

- Core loop present and playable end-to-end.
    
- 12 tiles + 6 relics implemented and adjustable via data files.
    
- Playtests (3–5 testers): majority report “I made interesting combos” and average decisions/turn ≈ 2+; core feedback loop feels rewarding.
    
- Average run ≈ 12 minutes; replays seedable.
    

---

## Metrics to track (analytics)

- Decisions per turn (placements / buys).
    
- Most-picked tile combos.
    
- Average turns per run and runs per session.
    
- % runs ending by overload (failure mode) vs failing by score/time.
    

---

## Final notes / dev advice

- Make tiles **strictly data-driven** from day one. That buys you rapid iteration and re-balancing.
    
- Start with minimal, legible visual/audio feedback — polish where it multiplies perceived quality (particles on consumer burst, one hit sound for run).
    
- Release a very small PLAYABLE loop to close the feedback loop fast; balance with real players before adding more tile types.
    
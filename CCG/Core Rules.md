
**Suggested defaults:** starting hand = **5** cards; starting life = **15**; recommended deck size = **36** cards; wallets = **18 singleton** cards.

---

## Overview

Magepunk is a fast, magepunk-fantasy card game where players harness **primal energies** to summon **Entities**, cast **Spells**, and activate **Nodes**. Players can play quick skirmishes by shuffling two **wallets** (18 cards each) together, or construct larger decks for extended play.

**Win condition:** Reduce your opponent’s life total to **0** to win. If a player is required to draw a card and cannot, that player loses.

---

## Game Components (terms)

- **Entity** — battlefield units: creatures, constructs, humans, beasts, or hybrids. Entities have **Impact** (attack) and **Durability** (health).
    
- **Node** — persistent cards that remain on the battlefield; provide ongoing effects or generate resources.
    
- **Spell** — one-time effects; cast, resolve, and discard.
    
- **Primal Energies:** Biom, Kinesis, Aether, Entropy, Radiance. Each card has one energy identity.
    
- **Resources / Primal Charges:** Many cards may alternatively be played face-down as a resource of their Energy. Resources generate **Primal Charges** which are spent to pay card costs.
    

---

## Deck & Wallet Rules

- **Wallet:** 18 cards, singleton (no duplicates within a wallet). Designed for shuffle-two instant play.
    
- **Deck (constructed):** recommended 36+ cards. Decks may be built by combining wallets or custom-building under your event rules.
    
- **Duplication cap:** Maximum of **2 copies** of any individual card in a constructed deck unless otherwise specified. (Wallets are singleton by design.)
    

---

## Setup

1. Each player shuffles their deck and draws **5** cards.
    
2. Determine who goes first (coin flip or mutual agreement).
    
3. Each player begins at **15 life** (adjustable by house rules).
    

---

## Turn Structure

Each turn follows this sequence of phases:

1. **Draw Phase** — Active player draws 1 card.
    
2. **Main Phase** — Active player may play Entities, Nodes, or Spells by paying costs with Primal Charges, or play cards face-down as resources. Activated abilities that are not Fast may be used now.
    
3. **Attack Phase** — Declare attackers with eligible Entities; defending player assigns blockers. Fast effects may be played at defined windows.
    
4. **Second Main Phase** — Play additional cards or use abilities.
    
5. **End Phase** — End your turn. During the End Phase players may play **Fast** cards and activate **Fast** activated abilities (see Fast). After the Fast response chain resolves the turn ends and control passes to the opponent.
    

---

## Resources — Spending & Refresh

- Cards list color-specific requirements and a **generic** cost. To cast a card face-up you must pay all color requirements with Primal Charges of the matching Energy, plus satisfy any generic cost with Primal Charges of any color.
    
- **Channels / Resource play:** Many cards may be played face-down into your Resource Zone to act as a **resource** (one card = one resource). A resource of Energy X produces **1 Primal Charge of Energy X** each turn. A card played as a resource does **not** provide its face-up effect while serving as a resource.
    
- **Using charges:** Spending a Primal Charge reduces your available charges for that turn. Each Primal Charge can be used once that turn.
    
- **Refresh:** At the **start of each player’s turn** (before the Draw Phase), all Primal Charges refresh: resources produce their Primal Charge again and previously spent charges are no longer considered spent. In other words, resources produce new charges each turn and available charges do not carry over between turns.
    

---

## Playing Cards — Quick Reference

- **To play face-up:** Pay the card’s color + generic costs from your available Primal Charges; place the card on the battlefield (Entity / Node) or resolve it (Spell).
    
- **To play as a resource:** Place the card face-down into your Resource Zone. It will generate 1 Primal Charge of its Energy each turn while it remains there. You cannot later use that same physical card’s face-up effect while it remains as a resource unless another effect returns it to hand or otherwise changes its state.
    
- **Notes:** If an effect requires an Entity or Node to be expended/used as a cost, represent that cost as a sacrifice or as a Primal Charge cost; explicit “exert/tap” mechanics are optional design features and should be defined explicitly if introduced.
    

---

## Abilities

Entities and Nodes can have abilities described on the card. Abilities come in two forms:

### Activated abilities

- Player-initiated, require paying a cost (Primal Charge, sacrificing a resource, etc.).
    
- Example: `Pay 1 Biom: This Entity gains +1 Impact until end of turn.`
    
- **Fast Activated Abilities:** If an activated ability is labeled **Fast**, it may be activated during the opponent’s Main/Attack/End Phase and may be added to a Fast response chain (see Fast).
    

### Triggered abilities

- Automatically trigger when a specified condition occurs (e.g., “When this Entity deals damage, draw a card.”).
    
- Triggered abilities **resolve immediately** when their condition is met following any Fast chain resolution windows. Triggered abilities themselves are **not** Fast (they are not added to Fast chains).
    

---

## Fast — Responses & Chains

- **Fast** is a keyword applied to Spells and some activated abilities to indicate they may be used outside the normal play sequence.
    
- When a Fast Spell or Fast activated ability is played, it is added to a **response chain**. Players may then alternate adding additional Fast Spells or Fast activated abilities to that chain (starting with the non-active player when the chain begins).
    
- When a player **declines** to add anything more to the chain, the entire chain **resolves immediately** in **last-in, first-out (LIFO)** order (the most recently added effect resolves first, then the next, and so on).
    
- Only one Fast chain may be open at any time. After it resolves, normal play resumes.
    

---

## Combat — Attacking & Blocking (multi-block allowed)

Combat follows an attacker/defender model:

1. **Declare attackers:** Active player declares which of their eligible Entities are attacking and which player or Node they attack. Entities played this turn cannot attack unless an effect explicitly allows it (see Activation Delay).
    
2. **Declare blockers:** Defending player may assign zero or more of their eligible Entities to block each attacking Entity. A single attacking Entity may be blocked by any number of defending Entities. Each defending Entity may block at most one attacking Entity, unless a card’s text states otherwise.
    
3. **Assign blocker order:** For each attacking Entity that has multiple defenders assigned, the attacking player orders the blocking Entities (this determines damage assignment order).
    
4. **Damage assignment & resolution:**
    
    - Each attacking Entity assigns its Impact damage to the first blocking Entity in order; it must assign lethal damage to a blocker before assigning to the next.
        
    - All blocking Entities assign their Impact damage simultaneously to the attacking Entity they block.
        
    - If an attacking Entity is **unblocked**, it deals its Impact as damage to the defending player (or target Node if the rules specify).
        
    - If an Entity receives total damage equal to or greater than its Durability during this combat, it is destroyed and moved to the discard area immediately.
        
5. **Combat ends; post-combat:** Combat damage remains marked on Entities until the end of the turn (see Damage & Reset below). Triggered abilities that result from dealing/receiving combat damage trigger at the appropriate time.
    

---

## Activation Delay

- Entities have an **activation delay** when they enter the battlefield: they cannot attack or use activated abilities that require them to be expended/used on the same turn they were played, unless a card specifically states otherwise.
    

---

## Damage & Reset

- **Entity damage is ephemeral:** Damage marked on Entities (from combat or Spells) is cleared at the end of the turn (after End Phase resolution).
    
- **Player life** is persistent and does not reset.
    
- If an Entity’s accumulated damage during a turn equals or exceeds its Durability, that Entity is destroyed immediately.
    
- If a card or effect would cause simultaneous results (both players drop to ≤ 0 life), apply your chosen tie policy (default: simultaneous loss → draw).
    

---

## Example Combat Clarifications (simple reference)

- If Attacker A (Impact 5) is blocked by Blockers B (Durability 2) and C (Durability 3) and the attacker orders [B then C]: Attacker assigns at least 2 to B, then remaining 3 to C; B and C simultaneously assign their Impact to the attacker. Any lethal assignment destroys the target immediately and related triggers occur.
    

---

## Win & Loss Conditions (explicit)

- A player **wins** immediately when an opponent’s life total is reduced to **0** or below.
    
- A player **loses** if they are required to draw a card and cannot.
    
- If both players would lose simultaneously, the match is a **draw** (or apply event-level tiebreakers).
    

---

## Design Notes

- Wording should be clear and deterministic. Core mechanical primitives include healing/repair, tempo, preparation/sequencing, transformation/harvest (Entropy), and one-off interventions.
    
- Subtypes (e.g., cyborg, wizard, beast) can be added to Entities, Nodes, and Spells to provide flavor and mechanical variety.
    
- The **Fast** keyword allows responsive play without the complexity of an open stack system.
    

---

## Glossary (quick)

- **Impact** — Entity’s attack value.
    
- **Durability** — Entity’s health threshold (damage ≥ Durability → destroyed).
    
- **Primal Charge** — one unit of resource produced by a face-down resource card (or by other effects).
    
- **Channel / Resource Zone** — area where cards played face-down as resources reside.
    
- **Fast** — keyword allowing a Spell or Activated Ability to be played off-turn and added to the Fast response chain.
    
- **Discard** — where Spells and destroyed Entities go; separate from deck.
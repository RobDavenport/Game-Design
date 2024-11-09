Themes:
1. Asteroid Mining
2. Terraforming Planet/Moon
3. Lunar/Planet Habitat Creation

Roguelike deckbuilding tile laying game, blending the simplistic board-game like gameplay of Party House (UFO 50) and the tile placing puzzle elements from Tiny Islands. Each level, players follow these steps:

1. Choose to draw a tile, or end the level.
2. Draw a random tile.
4. Place the tile on a grid tile.
5. Back to 1.

At the end of the level, tiles will be scored based off of their base values, as well as potential adjacency or placement bonuses depending on their location.

Win Objective:
Deciding between:
1. Complete a level with 4 "win" points on tiles within the time limit
2. Generate enough of a certain resource to pass the level.

Related Games:
1. Party House (UFO 50)
2. Tiny Islands

46-50 Possible Tiles

Potential Stories (GPT)
To tie the random draw mechanic into your asteroid mining theme while adding a corporate backstory, you could create a narrative where the player is a contractor or franchise owner of a mining operation, relying on a **central corporate supplier** that handles logistics but makes questionable decisions about delivery and drone types. Here’s how it could look:
### Story Context

The player is an independent operator contracted by a massive, bureaucratic mining corporation (think _AstroMining Unlimited_ or _Frontier Industries_). They’re tasked with managing the extraction and resource management on remote asteroids. However, due to corporate cost-cutting, inefficiency, or even the unpredictable nature of deep-space supply chains, the player’s **supplies are randomized**. Each “day,” corporate sends a batch of drones (tiles) from their main distribution center, but the types and quality vary widely, sometimes even including dangerous or malfunctioning units.

Danger Bust Mechanics:
Consider:
1. Instead of full bust, gain partial resources (like 25, 50 or 75%)
2. Gain full resources, but pay flat or scaling penalty after collection

Party House Reference
![[Pasted image 20241106000308.png]]
![[Pasted image 20241109232312.png]]

First Goal:
8 Items Per Shop (3 static, 5 random)
15-20 Unique Tiles

Consider a different approach for now, like:
~12 Total cards per shop
~2 Always available Basic cards, same for each run
~7 Always available this run, randomized for each run (1 or 2 win cards guaranteed)
~3 Rotating per shop round


Tile List:
Basic Tiles
Basic tiles are simple, straight forward, and generally just generate some resources.
1. Mining Bot:
	1. Cost: 1
	2. +1 Credit
	3. Always in Static Shop
2. Energy Bot
	1. Cost 2
	2. +1 Energy
	3. Always available in Static Shop
3. Dangerous Mining Bot
	1.  Not Purchasable
	2. +2 Credit
	3. 1 Danger
4. Win Bot
	1. Cost 40
	2. +1 Win point

More Basics (3-10 total)
1. Danger + high Energy
2. High Energy
3. Danger + high credits
4. Danger + med energy + med credits
5. High credits, -1 energy
6. 

Minor Payoff Tiles (Soft Payoffs, 5-10):
1. Score calculation: Earns +X credits if placed at the edge of the grid
2. Score Calculation: Earns +X credits/energy if placed next to a danger tile
3. Score Calculation: Earns +X credits/energy if not placed adjacent to a Danger Tile
4. Score Calculation: Earns +X credits/energy if nearby an Credit/Energy producer
5. On Place: Generates +1 energy/credits for each (3x3) neighboring energy/credit-producing tile.
	1. could be same resource or swap (ie nearby energy = +credits, )
 

Danger Mitigation Tiles
Helps mitigate against danger, either reducing danger of the tiles, or increasing the danger cap
1. LINE Danger Reducer, passive
2. AREA danger reducer, passive
3. Global +1 danger cap increaser

Booters
have an ability which removes a tile from the grid for the rest of the round
1. LINE booter, activated, credits
2. LINE booter, on entry, credits
3. ADJACENT Booter, on activate
4. ADJACENT Booter, on entry

Peekers 
Can view the next tile and keep/draw it or discard it
1. Peek on Use, Credits
	1. Cost 4
	2. +1 Credits
2. Peek on Entry, Larger Credits
	1. Cost 7
	2. +2 Credits
3. Peek on Entry, Energy
	1.  Cost 5
	2. +1 Energy
4. Peek on Use, Energy
	1. Cost 7
	2.  +2 energy

Tutor Tiles, 
Can fetch any tile from the deck next
1. Basic Tutor (cost energy)
2. Free tutor (no benefits)
3. Swap a current tile with any other tile in deck (current tile is removed)

Soft Payoff Tiles
1. Generates +1 credits/energy for each tile matching condition on LINE, on placed
2. Generates +X credits/energy for each unique tile placed in the area around it
	1. scales with "randomness"

Hard Payoff Tiles (Scaling)
Assorted tiles which are made to combo with specific situations or strategies, designed for late game scaling
1. Scaling Credit Generator. Gains +1 credit every time it enters the grid
	1. scaling over time, or "refreshing the grid"
2. Danger scaling credit/energy generator, gain X credits per danger producer on grid, at end of round
	1. scales with danger and danger prevention
3. "Scales with self" credit generator. Each copy of this on the grid increases earnings, at end of round
	1. simple stacking scaler
4. Scales with Basic mining bot credit generator, at end of round
	1. "weenie" approach which turns base cards into something better
	2. scales with basic cards + grid size
5. Scales with "Empty cells on line". Scales with grid size, at end of round
	1. scaling over time, grid size etc
	2. works as danger mitigation


Utility Tiles:
1. On Placed, refresh all neighboring tiles user-activated abilities
2. Activated: Increase a neighboring tile's Credits by 1
4. Activated: Summon and score the next tile (draws it)
5. Activated: Score an adjacent already placed tile instantly
6. LINE Evacuator: On placement, all LINE tiles are returned to deck including self
7. AREA Evacuator: On placement, all AROUND tiles are returned to deck including self


Prototype Game Mode:
Only static tiles, 10 total (+2 basics):
1. Win Bot
2. AREA Danger Reducer
3. 
4. "Scales with Self" Payoff (EoT Ability)
5. Adjacent Booter (Activated)
6. Peeker (Activated)
7. Fetcher (Activated)
8. High Energy
9. Danger + value credits/energy
10.  

Potential gpt prompt:
primary gameplay mechanics:

- placing tiles

- buying tiles at the end of each round

- attempting to reach 4 "win points" to win a run

- drawing 3 or more danger will end the round prematurely

resource dynamics

- two main resources.

- 1 is for obtaining new tiles. primary resource, easily generated

- 1 is for using bot abilities as well as buying grid expansions. not as easily generated. more grid space allows more tiles to be placed on each round

theme preferences:

- something sci fi

- needs to work well with low poly 3d models, or pixel art

- super short development cycle (1month), so modularity and flexibility

- bonus points for something which lets players connect with certain tiles or strategies, for example, cool and interesting robots.

- no time or skill to do organic objects

level structure:

- player draws a random tile and places that tile somewhere on the grid

- then they can decide to end the level, to avoid busting, or continue drawing another tile.

- should the player draw 3 danger, they must end the level after placing it

- a window to activate any last tile abilities

- on level end, all resource earnings are calculated and pooled into the players inventory to be used in the shop

shop screen:

- contains limited tiles, assortment of static, pre-selected and randomly selected tiles

- can purchase tiles to add to the deck for the next round

- can spend 2nd resource to expand the grid

- upon ending, goes into the next level, where the grid is reset

key gameplay mechanics:

- push your luck

- engine building

- deck building

- tile placement

story/theme constraints:

2. tiles being drawn randomly, with player only controlling position of them needs to make sense

3. players purchasing new tiles to add to the deck at the shop, needs to make sense

4. all of the tiles from the deck, and those purchased from the shop, remain in the deck until the end of the run

5. the level being a blank grid which is reset between each level, needs to make sense



TODOs:
- Add visual indicator for score on tile visual 
	- learn about rich text
- Add ViewDeck UI
- come up with the rest of the cards
- Attribute OFL Fonts
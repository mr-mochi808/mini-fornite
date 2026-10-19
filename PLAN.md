# Mini Fortnite: Game Plan

Status: planning only. No code has been written yet.

Screen designs (Claude Design canvas): https://claude.ai/artifact/6rRrLP9ERLzuG9efn8PwyX

## Decisions so far

| Topic | Choice |
|---|---|
| Platform | Browser (HTML + JavaScript), opened as a web page |
| Rendering | Three.js (3D) |
| Camera | Third-person, camera behind the player |
| Opponents | AI bots **and** online multiplayer |
| Players per match | 20 (empty spots are filled with AI bots) |
| Art style | Realistic-looking, but not a Fortnite copy, and kid-appropriate |
| Hosting | Free hosting site, so friends can join from anywhere |
| First-version mechanics | Time limit vote, gun upgrades through kills, gun crafting, turret dog, overtime duel, shrinking storm, loot, harvesting |
| Not in first version | Building (walls, ramps, floors) |

## Mechanics

### 1. Time limit vote
- Before every match, players vote on how long the match lasts: **10, 20 or 30 minutes**
- **Multiplayer:** everyone in the lobby votes; the option with the most votes wins
- **Single player (against AI bots):** the player's own vote decides
- The match timer is always shown in the HUD
- The storm shrinks over the chosen time
- When the timer runs out, the match goes to **overtime** (see section 4)

### 2. Gun upgrades (kill progression)
Players unlock the next gun by getting kills. Guns do not come from chests.

| # | Gun | More kills needed after the previous gun | Total kills | Details |
|---|---|---|---|---|
| 1 | **Glow pistol** | Start | 0 | Ordinary pistol, blue, glows in the dark |
| 2 | **Big pistol** | 10 | 10 | Bigger and stronger than the glow pistol; 6 bullets before you reload |
| 3 | **Crafted gun** | 15, plus all 4 parts | 25 | Built from parts found on the map (see Crafting below) |
| 4 | **Double pistol** | 20 | 45 | One pistol in each hand; needs gun 3 crafted first |
| 5 | **Shotgun** | 25 | 70 | Close range, high damage |
| 6 | **AK-47 Nerf gun** | 30 | 100 | Nerf-style look; 10 shots per reload |
| 7 | **Electric crossbow** | 35 | 135 | Shoots electric bolts; 15 shots per reload |
| 8 | **Turret dog** | 40 | 175 | Not a gun: a dog companion with a turret on its back (see below) |

- Each gun needs 5 more kills than the one before it: 10, then 15 more, then 20 more, and so on
- **Kills add up across rounds.** Your kill total is saved after every match
- **Guns unlock between matches, not during a match.** At the end of a match your kills are added to your total, and the results screen shows any gun you unlocked with a message and sound
- The HUD and the results screen show how many more kills you need for the next gun
- Bots use guns from the same list, picked to match their difficulty

#### Keeping guns between rounds
- Every gun you unlock is saved and stays yours in future rounds
- At the start of each round you **pick one gun** from the guns you own; that is the only gun you use for the round
- Progress is saved:
  - **Single player:** saved in the browser
  - **Multiplayer:** saved on the server under the player's name

#### Crafting (gun 3)
Find these 4 parts around the map (in chests, in houses, or dropped by eliminated players), then open the crafting menu to build the gun:

1. **Shotgun base**: the body of the gun
2. **Ammo cartridge**: holds the ammo
3. **Sniper scope**: lets you zoom in to aim
4. **Knife**: attached to the front for close-up attacks

- Crafting becomes possible once you have 25 total kills
- You must find all 4 parts in a single match and craft the gun before the match ends; once crafted, it is saved like any other gun
- The finished gun can shoot, zoom in with the scope, and stab with the knife
- The HUD shows which parts you have found
- Gun 4 stays locked until gun 3 is crafted, even if you have enough kills

#### Trophies
- Every **25 total kills** earns a **trophy** (25, 50, 75, ...)
- Trophies are saved and shown next to your name in the lobby, in the kill feed and on the results screen
- The more trophies you have, the higher you rank on the leaderboard
- Trophies will later be used to get **skins and backpacks** (to be designed later)

#### Turret dog (gun 8)
- A dog that follows you and fires its turret at nearby enemies on its own
- Has **100 health**, so it can take 100 damage before it is knocked out
- When you unlock it, you name it: **Mochi** or **Tough**
- Its name and health bar show above its head
- **Bones** are scattered around the map. If your dog is knocked out, pick up one bone to bring it back to life with full health

### 3. Shrinking storm
- The match starts with the whole map safe
- In phases, a circle shows where the next safe zone will be, then the storm closes in to it
- Players outside the circle take damage every second; the damage goes up in later phases
- Phase timing is stretched to fit the voted time limit (10, 20 or 30 minutes)
- HUD: minimap with the storm circle, time until the next shrink

### 4. Overtime and final countdown
- When the timer runs out and more than one player is still alive, the match goes to **overtime**
- The **two players with the most kills in this match** survive; everyone else is eliminated on the spot
- If players are tied for a top-two spot, all the tied players go into the final countdown
- The survivors fight in the **final countdown**, which has no time limit: it lasts until only one player is left
- The last one standing wins; there are no ties
- During the final countdown the storm stays at a small circle, so the survivors can't hide from each other

### 5. Player movement
- Walk, sprint, jump, crouch (WASD + Space + Shift + Ctrl)
- Mouse controls the camera; clicking locks the pointer
- Health (100) and shield (100); shield absorbs damage first
- Eliminated players drop their ammo, healing items and gun parts

### 6. Harvesting
- Pickaxe is always available and can't be dropped
- Hitting trees gives **wood**, rocks give **stone**, cars/fences give **metal**
- Each hit gives a set amount of material; the object breaks after a set number of hits

### 7. Loot
- **Chests:** press E to open; they give ammo, healing items and sometimes gun parts
- **Bones:** lying around the map, used to revive the turret dog
- **Ammo:** picked up from chests and the floor; R to reload
- **Healing:** bandages (health up to 75), medkit (full health), small shield (+25, up to 50), big shield potion (+50)
- Headshots do extra damage

### 8. Match flow
1. Lobby
2. Time limit vote
3. Each player picks one gun from the guns they own
4. Everyone spawns at random points
5. The match ends when one player is left, or goes to overtime when the timer runs out
6. Results screen: placement, kills this match, new kill total, guns unlocked, trophies earned

### 9. AI bots
- Fill the match up to 20 players
- Behaviour states: **loot**, **wander**, **fight**, **flee storm**
- Harder bots start with better guns
- Difficulty setting changes their aim accuracy and reaction time

### 10. Online multiplayer
- Node.js server using WebSockets
- Hosted on a free hosting site (plan: **Render** free tier) so friends can join from anywhere with a link
- The free tier goes to sleep when nobody is playing, so the first player may wait about a minute for it to wake up
- Up to 20 players per room; bots fill the empty spots
- The server is in charge of the game state (positions, health, kills, unlocks, storm, votes) so players can't cheat
- Rooms: create or join a match with a code
- Bots run on the server so they work the same in single-player and online matches
- Single player runs the same logic locally, with only bots

## HUD
- Health and shield bars
- Current gun, ammo in the magazine, and kills needed for the next gun
- Trophy count
- Gun parts found (for gun 3)
- Material counts (wood, stone, metal)
- Match timer and minimap with the storm circle
- Players remaining, kill feed
- Turret dog's name and health once unlocked
- "Final countdown" banner during overtime

## Map
- One small island with grass, trees, rocks, a few houses and some cars
- Houses contain chests
- Bones and gun parts spread around the map
- Some areas are darker so the glow pistol stands out
- Built from simple shapes first, then swapped for realistic models

## Art style
- **Realistic-looking:** real-looking grass, trees, rocks, houses and lighting with shadows, a sky and a day/night feel
- **Not a Fortnite copy:** our own characters, colors and style
- **Kid-appropriate:**
  - No blood or gore
  - Eliminated players disappear in a flash of light and drop their loot
  - Guns look like toys or sci-fi blasters (like the glow pistol and the Nerf AK-47), not real weapons
- Free models and textures from sites that allow use in any project (for example Poly Haven, Kenney and Quaternius)

## Build order (milestones)
1. **Setup:** project structure, Three.js scene, map with ground, trees and rocks
2. **Movement:** player character, third-person camera, collisions
3. **Glow pistol:** shooting, damage, health, eliminations
4. **Harvesting and loot:** pickaxe, materials, chests, ammo, healing
5. **Guns:** guns 2 and 4 to 7
6. **Crafting:** the 4 gun parts, crafting menu, gun 3
7. **Turret dog:** dog companion, turret AI, naming Mochi or Tough, bones to revive
8. **Saving and progression:** saved kill total, unlocks between matches, trophies, picking one gun at the start of each round
9. **Storm, timer, vote and overtime:** time limit vote, shrinking circle, minimap, final countdown
10. **AI bots and match flow:** bots, results screen (a complete single-player game at this point)
11. **Multiplayer:** Node.js server, rooms, syncing players, lobby voting, saving progress on the server
12. **Polish:** sounds, effects, menus, balancing

## Later ideas (not in the first version)
- Building (walls, ramps, floors, editing)
- Battle bus and skydiving
- Vehicles
- Skins and backpacks, bought with trophies
- Emotes

## Open questions
None right now. New questions will be added here as building starts.

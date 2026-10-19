# Mini Fortnite: Game Plan

Status: planning only. No code has been written yet.

## Decisions so far

| Topic | Choice |
|---|---|
| Platform | Browser (HTML + JavaScript), opened as a web page |
| Rendering | Three.js (3D) |
| Camera | Third-person, camera behind the player |
| Opponents | AI bots **and** online multiplayer |
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

| # | Gun | Kills to unlock | Details |
|---|---|---|---|
| 1 | **Glow pistol** | Start | Ordinary pistol, blue, glows in the dark |
| 2 | **Big pistol** | 10 | Bigger and stronger than the glow pistol; 6 bullets before you reload |
| 3 | **Crafted gun** | 15, plus all 4 parts | Built from parts found on the map (see Crafting below) |
| 4 | **Double pistol** | 20 | One pistol in each hand; needs gun 3 crafted first |
| 5 | **Shotgun** | 25 | Close range, high damage |
| 6 | **AK-47 Nerf gun** | 30 | Nerf-style look; 10 shots per reload |
| 7 | **Electric crossbow** | 35 | Shoots electric bolts; 15 shots per reload |
| 8 | **Turret dog** | 40 | Not a gun: a dog companion with a turret on its back (see below) |

- The required kills go up by 5 for each gun: 10, 15, 20, 25, 30, 35, 40
- When you unlock a gun, a message and sound play, and the new gun is equipped
- The HUD shows how many kills you need for the next gun
- Bots also unlock guns as they get kills, so they get tougher during the match

#### Keeping guns between rounds
- Every gun you unlock is saved and stays yours in future rounds
- At the start of each round you **pick one gun** from the guns you own; that is the only gun you start the round with
- During the round you keep upgrading from that gun's level (for example, start with the shotgun and your kills work toward the AK-47 Nerf gun)
- Progress is saved:
  - **Single player:** saved in the browser
  - **Multiplayer:** saved on the server under the player's name

#### Crafting (gun 3)
Find these 4 parts around the map (in chests, in houses, or dropped by eliminated players), then open the crafting menu to build the gun:

1. **Shotgun base**: the body of the gun
2. **Ammo cartridge**: holds the ammo
3. **Sniper scope**: lets you zoom in to aim
4. **Knife**: attached to the front for close-up attacks

- The finished gun can shoot, zoom in with the scope, and stab with the knife
- The HUD shows which parts you have found
- Gun 4 stays locked until gun 3 is crafted

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
- The **two players with the most kills** survive; everyone else is eliminated on the spot
- Those two fight a 1v1 duel during a **final countdown**
- The last one standing wins

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
6. Results screen: placement, kills, guns unlocked

### 9. AI bots
- Fill the match up to a set player count (for example 20)
- Behaviour states: **loot**, **wander**, **fight**, **flee storm**
- Unlock guns through kills like players do
- Difficulty setting changes their aim accuracy and reaction time

### 10. Online multiplayer
- Node.js server using WebSockets
- The server is in charge of the game state (positions, health, kills, unlocks, storm, votes) so players can't cheat
- Rooms: create or join a match with a code
- Bots run on the server so they work the same in single-player and online matches
- Single player runs the same logic locally, with only bots

## HUD
- Health and shield bars
- Current gun, ammo in the magazine, and kills needed for the next gun
- Gun parts found (for gun 3)
- Material counts (wood, stone, metal)
- Match timer and minimap with the storm circle
- Players remaining, kill feed
- Turret dog's name and health once unlocked
- Final countdown timer during overtime

## Map
- One small island with grass, trees, rocks, a few houses and some cars
- Houses contain chests
- Bones and gun parts spread around the map
- Some areas are darker so the glow pistol stands out
- Built from simple shapes first, real models later

## Build order (milestones)
1. **Setup:** project structure, Three.js scene, map with ground, trees and rocks
2. **Movement:** player character, third-person camera, collisions
3. **Glow pistol:** shooting, damage, health, eliminations
4. **Harvesting and loot:** pickaxe, materials, chests, ammo, healing
5. **Gun upgrades:** kill tracking, guns 2 and 4 to 7, unlock messages
6. **Crafting:** the 4 gun parts, crafting menu, gun 3
7. **Turret dog:** dog companion, turret AI, naming Mochi or Tough, bones to revive
8. **Saving and gun picking:** save unlocked guns, pick one gun at the start of each round
9. **Storm, timer, vote and overtime:** time limit vote, shrinking circle, minimap, final countdown
10. **AI bots and match flow:** bots, results screen (a complete single-player game at this point)
11. **Multiplayer:** Node.js server, rooms, syncing players, lobby voting, saving progress on the server
12. **Polish:** sounds, effects, menus, balancing

## Later ideas (not in the first version)
- Building (walls, ramps, floors, editing)
- Battle bus and skydiving
- Vehicles
- Skins and emotes

## Open questions
- **Kills across rounds:** with about 20 players in a match you can get at most 19 kills, so reaching 10 to 40 kills needs kills to carry over between rounds. Should kills add up across rounds, or should players respawn during a match so more kills are possible?
- **Kill counts:** is each number the total (10 total for gun 2, 15 total for gun 3) or the extra kills after the last gun (10, then 15 more, then 20 more)?
- **Final countdown:** how long is it, and who wins if both players are still alive when it ends? (suggested: 60 seconds, then the player with more health wins)
- **Overtime tie:** if several players have the same kill count, who gets into the duel? (suggested: the one who got the kills first)
- How many players per match? (suggested: 20)
- Blocky and simple art, or try to look like Fortnite?
- Where should the multiplayer server be hosted?

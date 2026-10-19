# Mini Fortnite: Game Plan

Status: planning only. No code has been written yet.

## Decisions so far

| Topic | Choice |
|---|---|
| Platform | Browser (HTML + JavaScript), opened as a web page |
| Rendering | Three.js (3D) |
| Camera | Third-person, camera behind the player |
| Opponents | AI bots **and** online multiplayer |
| First-version mechanics | Time limit vote, gun upgrades through kills, shrinking storm, loot, harvesting |
| Not in first version | Building (walls, ramps, floors) |

## Mechanics

### 1. Time limit vote
- Before every match, players vote on how long the match lasts: **10, 20 or 30 minutes**
- **Multiplayer:** everyone in the lobby votes; the option with the most votes wins
- **Single player (against AI bots):** the player's own vote decides
- The match timer is always shown in the HUD
- The storm shrinks over the chosen time, so the last circle closes when the timer runs out (see Open questions for what happens if more than one player is still alive)

### 2. Gun upgrades (kill progression)
Everyone starts with the same gun. You unlock the next gun by getting enough kills before the time runs out. Guns do not come from chests.

| # | Gun | How you get it | Details |
|---|---|---|---|
| 1 | **Glow pistol** | Start of the match | Ordinary pistol, blue, glows in the dark |
| 2 | **Big pistol** | Kills | Bigger and stronger than the glow pistol; 6 bullets before you reload |
| 3 | **Crafted gun** | Find parts around the map and craft it | Must be crafted to unlock gun 4 (see Crafting below) |
| 4 | **Double pistol** | Kills, after crafting gun 3 | One pistol in each hand |
| 5 | **Shotgun** | Kills | Close range, high damage |
| 6 | **AK-47 Nerf gun** | Kills | Nerf-style look; 10 shots per reload |
| 7 | **Electric crossbow** | Kills | Shoots electric bolts; 15 shots per reload |
| 8 | **Turret dog** | Kills | Not a gun: a dog companion with a turret on its back (see below) |

- When you unlock a gun, a message and sound play, and the new gun is equipped
- You keep your older guns and can switch back to them with number keys or the mouse wheel
- The HUD shows how many kills you need for the next gun
- Bots also unlock guns as they get kills, so they get tougher during the match

#### Crafting (gun 3)
- Gun parts are hidden around the map, for example in chests, in houses, or dropped by harvested objects
- Collect all the parts, then open the crafting menu to build the gun
- Gun 4 stays locked until gun 3 is crafted

#### Turret dog (gun 8)
- A dog that follows you and fires its turret at nearby enemies on its own
- Has **100 health**, so it can take 100 damage before it is knocked out
- When you unlock it, you name it: **Mochi** or **Tough**
- Its name and health bar show above its head

### 3. Player movement
- Walk, sprint, jump, crouch (WASD + Space + Shift + Ctrl)
- Mouse controls the camera; clicking locks the pointer
- Health (100) and shield (100); shield absorbs damage first
- Eliminated players drop their ammo, healing and gun parts

### 4. Harvesting
- Pickaxe is always available and can't be dropped
- Hitting trees gives **wood**, rocks give **stone**, cars/fences give **metal**
- Each hit gives a set amount of material; the object breaks after a set number of hits
- Materials may be used as crafting ingredients for gun 3

### 5. Loot
- **Chests:** press E to open; they give ammo, healing items and sometimes gun parts
- **Ammo:** picked up from chests and the floor; R to reload
- **Healing:** bandages (health up to 75), medkit (full health), small shield (+25, up to 50), big shield potion (+50)
- Headshots do extra damage

### 6. Shrinking storm
- The match starts with the whole map safe
- In phases, a circle shows where the next safe zone will be, then the storm closes in to it
- Players outside the circle take damage every second; the damage goes up in later phases
- Phase timing is stretched to fit the voted time limit (10, 20 or 30 minutes)
- HUD: minimap with the storm circle, time until the next shrink

### 7. Match flow
1. Lobby
2. Time limit vote
3. Everyone spawns at random points with the glow pistol
4. The match ends when one player is left or the timer runs out
5. Results screen: placement, kills, highest gun unlocked

### 8. AI bots
- Fill the match up to a set player count (for example 20)
- Behaviour states: **loot**, **wander**, **fight**, **flee storm**
- Unlock guns through kills like players do
- Difficulty setting changes their aim accuracy and reaction time

### 9. Online multiplayer
- Node.js server using WebSockets
- The server is in charge of the game state (positions, health, kills, unlocks, storm, votes) so players can't cheat
- Rooms: create or join a match with a code
- Bots run on the server so they work the same in single-player and online matches
- Single player runs the same logic locally, with only bots

## HUD
- Health and shield bars
- Current gun, ammo in the magazine, and kills needed for the next gun
- Gun parts collected (for gun 3)
- Material counts (wood, stone, metal)
- Match timer and minimap with the storm circle
- Players remaining, kill feed
- Turret dog's name and health once unlocked

## Map
- One small island with grass, trees, rocks, a few houses and some cars
- Houses contain chests
- Some areas are darker so the glow pistol stands out
- Built from simple shapes first, real models later

## Build order (milestones)
1. **Setup:** project structure, Three.js scene, map with ground, trees and rocks
2. **Movement:** player character, third-person camera, collisions
3. **Glow pistol:** shooting, damage, health, eliminations
4. **Harvesting and loot:** pickaxe, materials, chests, ammo, healing
5. **Gun upgrades:** kill tracking, guns 2 and 4 to 7, unlock messages, switching guns
6. **Crafting:** gun parts, crafting menu, gun 3
7. **Turret dog:** dog companion, turret AI, naming Mochi or Tough
8. **Storm, timer and vote:** time limit vote, shrinking circle, minimap
9. **AI bots and match flow:** bots, results screen (a complete single-player game at this point)
10. **Multiplayer:** Node.js server, rooms, syncing players, lobby voting
11. **Polish:** sounds, effects, menus, balancing

## Later ideas (not in the first version)
- Building (walls, ramps, floors, editing)
- Battle bus and skydiving
- Vehicles
- Skins and emotes

## Open questions
- How many kills unlock each gun? (suggested: 1 kill each for guns 2 and 4, 2 kills each for guns 5 to 7, 3 kills for the dog)
- What is gun 3, the crafted gun? (for example a scrap blaster or a slingshot) And which parts are needed to build it?
- Do the upgrades reset every match, or do players keep them between matches?
- What happens if the timer runs out with more than one player alive? (suggested: the player with the most kills wins)
- If the turret dog is knocked out, does it come back after some time, or is it gone for the rest of the match?
- Is the time limit vote a tie? (suggested: pick randomly among the tied options)
- How many players per match? (suggested: 20)
- Blocky and simple art, or try to look like Fortnite?
- Where should the multiplayer server be hosted?

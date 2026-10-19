# Mini Fortnite: Game Plan

Status: planning only. No code has been written yet.

## Decisions so far

| Topic | Choice |
|---|---|
| Platform | Browser (HTML + JavaScript), opened as a web page |
| Rendering | Three.js (3D) |
| Camera | Third-person, camera behind the player |
| Opponents | AI bots **and** online multiplayer |
| First-version mechanics | Shrinking storm, loot and weapons, harvesting |
| Not in first version | Building (walls, ramps, floors) |

## Mechanics

### 1. Player movement
- Walk, sprint, jump, crouch (WASD + Space + Shift + Ctrl)
- Mouse controls the camera; clicking locks the pointer
- Health (100) and shield (100); shield absorbs damage first
- When a player is eliminated they drop all their items as loot

### 2. Harvesting
- Pickaxe is always in slot 1 and can't be dropped
- Hitting trees gives **wood**, rocks give **stone**, cars/fences give **metal**
- Each hit gives a set amount of material; the object shrinks and breaks after a set number of hits
- Materials are counted in the HUD (they will feed building later, if added)

### 3. Loot and weapons
- **Inventory:** 5 slots plus the pickaxe; number keys 1 to 6 or the mouse wheel to switch
- **Chests:** placed around the map; press E to open, they spill 2 to 3 items
- **Floor loot:** single items spawned on the ground
- **Rarity levels:** Common (grey), Uncommon (green), Rare (blue), Epic (purple), Legendary (gold). Higher rarity means more damage
- **Weapons (starting set):**
  - Assault rifle: medium range, automatic
  - Shotgun: close range, high damage, slow
  - SMG: close range, very fast fire
  - Sniper: long range, one shot then reload
- **Ammo:** light, medium, heavy and shells, picked up separately; R to reload
- **Healing:** bandages (health up to 75), medkit (full health), small shield (+25, up to 50), big shield potion (+50)
- Headshots do extra damage

### 4. Shrinking storm
- The match starts with the whole map safe
- In phases, a circle shows where the next safe zone will be, then the storm closes in to it
- Players outside the circle take damage every second; the damage goes up in later phases
- The final circle closes to nothing, which forces the last players to fight
- The HUD shows a minimap, the time until the next shrink, and the distance to the safe zone

### 5. Match flow
- Lobby, then everyone spawns (first version: random spawn points; a battle bus and skydiving can come later)
- The last player or bot alive wins with a "Victory Royale" screen
- When eliminated: a results screen with placement and eliminations, then spectate or go back to the lobby

### 6. AI bots
- Fill the match up to a set player count (for example 20)
- Simple behaviour states: **loot** (go to nearby chests), **wander**, **fight** (shoot at players they can see), **flee storm** (move into the safe zone)
- Difficulty setting changes their aim accuracy and reaction time

### 7. Online multiplayer
- Node.js server using WebSockets
- The server is in charge of the game state (positions, health, loot, storm) so players can't cheat by editing their own browser
- Players send inputs; the server sends back the world state many times per second
- Bots run on the server so they work the same in single-player and online matches
- Rooms: create or join a match with a code
- Single-player mode runs the same server logic locally, with only bots

## HUD
- Health and shield bars
- Inventory bar with ammo counts
- Material counts (wood, stone, metal)
- Minimap with the storm circle
- Players remaining, eliminations, storm timer
- Kill feed

## Map
- One small island with grass, trees, rocks, a few houses and some cars
- Houses contain chests
- Built from simple shapes first, real models later

## Build order (milestones)
1. **Setup:** project structure, Three.js scene, map with ground, trees and rocks
2. **Movement:** player character, third-person camera, collisions
3. **Harvesting:** pickaxe, breakable objects, material counts
4. **Weapons and loot:** shooting, damage, chests, inventory, healing
5. **Storm:** shrinking circle, damage, minimap
6. **AI bots and match flow:** bots, win and lose screens (a complete single-player game at this point)
7. **Multiplayer:** Node.js server, rooms, syncing players
8. **Polish:** sounds, effects, menus, balancing

## Later ideas (not in the first version)
- Building (walls, ramps, floors, editing)
- Battle bus and skydiving
- Vehicles
- Skins and emotes

## Open questions
- How many players per match? (suggested: 20)
- Should the art be blocky and simple, or try to look like Fortnite?
- Where should the multiplayer server be hosted? (free options like Render or Fly.io, or only on your own computer for now)
- Any weapons or items you want to add or remove from the list above?

## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json

## JSON Example

```json
{
    "seasonalInfo": {
        "breakthroughPokemon": ["Galarian Mr"],
        "spindaPatterns": [6, 7],
        "season": null
    },
    "tasks": [
        {
            "text": "Catch 7 Pokémon",
            "type": "catch",
            "rewards": [
                {
                    "type": "encounter",
                    "name": "Magikarp",
                    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm129.icon.png",
                    "imageWidth": 112,
                    "imageHeight": 83,
                    "canBeShiny": true,
                    "shinyAssetUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_129_00_shiny.png",
                    "combatPower": { "min": 99, "max": 117 }
                }
            ]
        }
    ]
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `seasonalInfo` | object | Seasonal research information |
| `seasonalInfo.breakthroughPokemon` | array | Pokémon available from 7-day Research Breakthrough (names may be abbreviated) |
| `seasonalInfo.spindaPatterns` | array | Currently available Spinda pattern numbers |
| `seasonalInfo.season` | string\|null | Current season name, null if not applicable |
| `tasks` | array | Field research tasks |
| `tasks[].text` | string | Task requirement description |
| `tasks[].type` | string | Task category: "catch", "throw", "battle", "explore", "training", "buddy", "rocket", "sponsored", or "ar" (may be omitted for special events) |
| `tasks[].rewards` | array | Possible rewards for completing the task |
| `rewards[].type` | string | Reward type: "encounter" (Pokémon) or "item" |
| `rewards[].name` | string | Pokémon or item name |
| `rewards[].image` | string | Reward icon URL |
| `rewards[].imageWidth` | integer | Icon width in pixels |
| `rewards[].imageHeight` | integer | Icon height in pixels |
| `rewards[].canBeShiny` | boolean | Whether Pokémon can be shiny (encounters only) |
| `rewards[].shinyAssetUrl` | string\|null | Shiny sprite URL (encounters only) |
| `rewards[].combatPower` | object | CP range at level 15 (encounters only) |
| `rewards[].combatPower.min` | integer | Minimum CP |
| `rewards[].combatPower.max` | integer | Maximum CP |
| `rewards[].quantity` | integer | Item quantity (items only) |

## Data Structure

The endpoint returns a JSON object with two top-level keys:
1. `seasonalInfo`: Current season/long-term research information
2. `tasks`: Array of active field research task objects

This differs from other endpoints which return arrays directly.

## Seasonal Information

**Research Breakthrough** (`breakthroughPokemon`):
- Pokémon encountered after completing 7 field research stamps
- Resets at 1:00 PM local time each day
- Array may contain abbreviated names (e.g., "Galarian Mr" for Galarian Mr. Mime)
- Rotates monthly or seasonally
- Always catchable (100% catch rate)
- Fixed level 15, minimum 10/10/10 IVs

**Spinda Patterns** (`spindaPatterns`):
- Array of currently available Spinda form numbers (0-8)
- Obtained from "Make 5 Great Curveball Throws in a row" task
- Pattern is randomly selected from available patterns upon encounter
- Each pattern has distinct spot placement
- Patterns rotate monthly

**Season** (`season`):
- Current season name ("Precious Paths", "Rising Heroes", etc.)
- May be null between seasons or during special periods
- Affects spawn pools, research rewards, and event schedules

## Field Research Tasks

### Task Categories (`type`)

Tasks are categorized by required action:
- **catch**: Catch specific types, species, or quantities of Pokémon
- **throw**: Make specific throw types (Nice, Great, Excellent, Curveball)
- **battle**: Win raids, trainer battles, Team GO Rocket battles, or Max Battles
- **explore**: Walk distance, spin PokéStops, take snapshots, hatch eggs
- **training**: Power up, evolve, Mega Evolve Pokémon, earn XP/Stardust
- **buddy**: Interact with buddy (earn hearts, candies, take snapshots)
- **rocket**: Defeat Team GO Rocket members
- **sponsored**: Special partner tasks (rare)
- **ar**: AR Scanning tasks for PokéStop/Gym data collection
- May be omitted for event-specific or miscellaneous tasks

### Task Text

`text` provides human-readable task requirement:
- Exactly as displayed in-game
- May include specific numbers ("Catch 7 Pokémon")
- May include type requirements ("Catch 10 Fire-type Pokémon")
- May include special conditions ("in a row", "with Weather Boost")

### Reward System

**Reward Types**:

1. **Encounter** (`type: "encounter"`):
   - Pokémon encounter reward
   - Includes full encounter details:
     - `name`: Pokémon species name
     - `image`: Icon (dimensions in `imageWidth`, `imageHeight`)
     - `canBeShiny`: Whether shiny variant possible from this task
     - `shinyAssetUrl`: Shiny sprite URL (null if not available)
     - `combatPower`: CP range object with `min` and `max`
   - Encounter level always 15 (similar to raids 1-3 star)
   - Minimum IVs: 10/10/10

2. **Item** (`type: "item"`):
   - Physical item reward (Poké Balls, Berries, TMs, Rare Candy, etc.)
   - Includes:
     - `name`: Item name with quantity ("×10", "×5")
     - `image`: Item icon
     - `imageWidth`, `imageHeight`: Icon dimensions
     - `quantity`: Number of items received

**Multiple Rewards**:
- When `rewards` array has multiple entries, **one** is randomly selected
- Probability is typically equal across options, but may be weighted
- Weights not exposed in data; determined server-side
- Common for encounter tasks to offer multiple species options
- Item tasks may offer different item types as alternatives

**Guaranteed vs Random**:
- Single entry in `rewards`: Guaranteed specific reward
- Multiple entries: Random selection from pool

### Combat Power Calculations

Encounter CP ranges for level 15 Pokémon:
- `min`: 10/10/10 IVs (minimum from research)
- `max`: 15/15/15 IVs (100% perfect)

CP formula considers:
- Species base stats (Attack, Defense, Stamina)
- Level (always 15 for research)
- Individual Values (10-15 for each stat)

### Shiny Mechanics

For encounter rewards:
- `canBeShiny: true`: Shiny variant is possible
- Shiny rate typically ~1/500 (standard wild rate)
- Specific tasks may have boosted rates during events
- `shinyAssetUrl`: Provides visual preview of shiny form
- Shiny determination occurs when task reward is claimed
- Cannot be re-rolled; encounter is locked upon task completion

## Research Task Rotation

**Monthly Rotation**:
- Pool of tasks changes on 1st of each month at 1:00 PM PT
- Aligns with Research Breakthrough rotation
- Some tasks persist across months (common tasks)
- New tasks introduced for current events/seasons

**Event Modifications**:
- Special events add temporary tasks
- Event tasks often have better rewards (shinies, rare Pokémon)
- Event tasks removed when event ends
- May completely replace standard pool during major events

**PokéStop Distribution**:
- Each PokéStop has one task per day
- Task type is fixed for that PokéStop for the day
- Resets at midnight local time
- All players receive same task from same PokéStop on same day
- Allows community task reporting and coordination

## AR Scanning Tasks

`type: "ar"` tasks:
- Require uploading AR scan of PokéStop or Gym
- Rewards typically include Poké Balls, Berries, or Poffins
- Help Niantic build 3D maps for AR features
- Optional; can be deleted without penalty
- One AR task available per PokéStop per day

## Stacking and Management

- Players can hold up to 3 field research tasks simultaneously
- Completed tasks can be stacked (up to 100+ encounters)
- Stacked encounters useful for:
  - Special research requirements
  - Community Day catch bonuses
  - Stardust events
- Tasks can be deleted to make room for better tasks

## Important Notes

- Task completion is server-verified (no client manipulation)
- Progress persists across app restarts
- "In a row" tasks reset upon failure
- Weather Boost tasks require Pokémon caught during specific weather
- Some tasks require specific conditions (buddy walking distance, raid tier)
- Encounter rewards do not flee; unlimited catch attempts
- Items from tasks are added directly to bag (no inventory selection)
- Task rewards bypass storage limits temporarily
- Historical tasks not maintained in dataset; only current month available

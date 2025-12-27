# Pokemon GO Data - JSON Files Documentation

This repository contains Pokemon GO game data in JSON format, the data includes current events, raid bosses, field research tasks, egg hatches, and Team GO Rocket lineups.

## Files

| File | Size | Description |
|------|------|-------------|
| `combined.min.json` | 136 KB | All data combined in one file |
| `events.min.json` | 104 KB | Events only |
| `raids.min.json` | 7.8 KB | Raid bosses only |
| `research.min.json` | 39 KB | Field research tasks only |
| `eggs.min.json` | 24 KB | Egg hatches only |
| `rocketLineups.min.json` | 38 KB | Team GO Rocket lineups only |

## File Structures

### combined.min.json

Contains all data in one JSON object with five top-level keys:

```json
{
  "events": [...],
  "raids": [...],
  "research": {...},
  "eggs": [...],
  "rocketLineups": [...]
}
```

### events.min.json

Array of 64 event objects:

```json
[
  {
    "eventID": "winter-holiday-part-2-2025",
    "name": "Winter Holiday Part 2",
    "eventType": "event",
    "heading": "Event",
    "link": "https://leekduck.com/events/winter-holiday-part-2-2025/",
    "image": "https://cdn.leekduck.com/assets/img/events/...",
    "start": "2025-12-24T10:00:00.000",
    "end": "2025-12-29T20:00:00.000",
    "timezone": "Local Time",
    "extraData": null
  }
]
```

**Fields:**
- `eventID` (string): Unique identifier
- `name` (string): Event name
- `eventType` (string): Type category (event, raid-hour, community-day, etc.)
- `heading` (string): Display category
- `link` (string): LeekDuck event page URL
- `image` (string): Event banner image URL
- `start` (string): ISO 8601 datetime
- `end` (string): ISO 8601 datetime
- `timezone` (string|null): "Local Time" or null for UTC
- `extraData` (object|null): Additional event data

### raids.min.json

Array of 16 current raid boss objects:

```json
[
  {
    "name": "Zekrom",
    "tier": "5-Star Raids",
    "canBeShiny": true,
    "types": [
      {"name": "dragon", "image": "https://leekduck.com/assets/img/types/dragon.png"},
      {"name": "electric", "image": "https://leekduck.com/assets/img/types/electric.png"}
    ],
    "combatPower": {
      "normal": {"min": 2217, "max": 2307},
      "boosted": {"min": 2771, "max": 2884}
    },
    "boostedWeather": [
      {"name": "windy", "image": "https://leekduck.com/assets/img/weather/windy.png"},
      {"name": "rainy", "image": "https://leekduck.com/assets/img/weather/rainy.png"}
    ],
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm644.icon.png"
  }
]
```

**Fields:**
- `name` (string): Pokemon name
- `tier` (string): Raid tier (1-Star Raids, 3-Star Raids, 5-Star Raids, Mega Raids)
- `canBeShiny` (boolean): Shiny availability
- `types` (array): Pokemon types with name and image URL
- `combatPower` (object): CP ranges for normal and weather-boosted catches
- `boostedWeather` (array): Weather conditions that boost this Pokemon
- `image` (string): Pokemon sprite URL

### research.min.json

Object containing seasonal info and field research tasks:

```json
{
  "seasonalInfo": {
    "breakthroughPokemon": ["Galarian Mr"],
    "spindaPatterns": [6, 7],
    "season": null
  },
  "tasks": [
    {
      "text": "Catch 10 Pokémon",
      "type": "catch",
      "rewards": [
        {
          "type": "item",
          "name": "×1500",
          "image": "https://cdn.leekduck.com/assets/img/items/Stardust.png",
          "quantity": 1500
        }
      ]
    },
    {
      "text": "Make 3 Great Throws in a row",
      "type": "throw",
      "rewards": [
        {
          "type": "encounter",
          "name": "Lileep",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm345.icon.png",
          "canBeShiny": true,
          "combatPower": {"min": 515, "max": 553}
        }
      ]
    }
  ]
}
```

**seasonalInfo Fields:**
- `breakthroughPokemon` (array): Research Breakthrough rewards
- `spindaPatterns` (array): Available Spinda pattern numbers
- `season` (string|null): Current season name

**tasks Fields:**
- `text` (string): Task description
- `type` (string|undefined): Task category (catch, throw, battle, buddy, etc.)
- `rewards` (array): Possible rewards

**Reward object (item):**
- `type` (string): "item"
- `name` (string): Item name
- `image` (string): Item icon URL
- `quantity` (number): Amount received

**Reward object (encounter):**
- `type` (string): "encounter"
- `name` (string): Pokemon name
- `image` (string): Pokemon icon URL
- `canBeShiny` (boolean): Shiny availability
- `combatPower` (object): CP range with min and max

### eggs.min.json

Array of 89 Pokemon available from eggs:

```json
[
  {
    "name": "Larvesta",
    "eggType": "2 km",
    "isAdventureSync": false,
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm636.icon.png",
    "canBeShiny": true,
    "combatPower": {"min": 855, "max": 855},
    "isRegional": false,
    "isGiftExchange": false,
    "rarity": 4,
    "rarityTier": "Very Rare"
  }
]
```

**Fields:**
- `name` (string): Pokemon name
- `eggType` (string): Egg distance (1 km, 2 km, 5 km, 7 km, 10 km, 12 km)
- `isAdventureSync` (boolean): Available from Adventure Sync rewards
- `image` (string): Pokemon sprite URL
- `canBeShiny` (boolean): Shiny availability
- `combatPower` (object): Hatch CP range with min and max
- `isRegional` (boolean): Regional exclusive
- `isGiftExchange` (boolean): Available from friend gift eggs
- `rarity` (number): Rarity tier (1-4)
- `rarityTier` (string): Rarity description (Common, Rare, Very Rare, etc.)

### rocketLineups.min.json

Array of 26 Team GO Rocket battle lineups:

```json
[
  {
    "name": "Giovanni",
    "title": "Team GO Rocket Boss",
    "type": "",
    "gender": "Male",
    "firstPokemon": [
      {
        "name": "Persian",
        "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm53.icon.png",
        "types": ["normal"],
        "isEncounter": false,
        "canBeShiny": false
      }
    ],
    "secondPokemon": [
      {
        "name": "Kangaskhan",
        "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm115.icon.png",
        "types": ["normal"],
        "isEncounter": false,
        "canBeShiny": false
      },
      {
        "name": "Rhyperior",
        "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm464.icon.png",
        "types": ["ground", "rock"],
        "isEncounter": false,
        "canBeShiny": false
      }
    ],
    "thirdPokemon": [
      {
        "name": "Tornadus (Incarnate)",
        "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm641.fINCARNATE.icon.png",
        "types": ["flying"],
        "isEncounter": true,
        "canBeShiny": false
      }
    ]
  }
]
```

**Fields:**
- `name` (string): Grunt, Leader, or Boss name
- `title` (string): Role (Team GO Rocket Grunt, Team GO Rocket Leader, Team GO Rocket Boss)
- `type` (string): Type specialty for themed grunts
- `gender` (string): "Male" or "Female"
- `firstPokemon` (array): First battle slot options
- `secondPokemon` (array): Second battle slot options
- `thirdPokemon` (array): Third battle slot options (reward Pokemon)

**Pokemon object in lineup:**
- `name` (string): Pokemon name
- `image` (string): Pokemon icon URL
- `types` (array): Pokemon type(s)
- `isEncounter` (boolean): Can be caught after battle
- `canBeShiny` (boolean): Shiny availability

## Usage Notes

- All dates/times are in ISO 8601 format
- Events with `"timezone": "Local Time"` occur at the specified time in each player's local timezone
- Events with `"timezone": null` use UTC time
- Image URLs point to LeekDuck's CDN
- Shiny availability indicated by `canBeShiny` boolean field
- CP ranges represent catch values, not max possible CP

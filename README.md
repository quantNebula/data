# Pokemon GO Data - JSON Files Documentation

This repository contains Pokemon GO game data in JSON format. Data is automatically scraped and updated, including current events, raid bosses, field research tasks, egg hatches, and Team Rocket lineups.

## Files

| File | Description |
|------|-------------|
| `combined.min.json` | All data combined in one file |
| `events.min.json` | Events only |
| `raids.min.json` | Raid bosses only |
| `research.min.json` | Field research tasks only |
| `eggs.min.json` | Egg hatches only |
| `rocketLineups.min.json` | Team Rocket lineups only |

## CDN Links

| File                     | CDN URL                                                                    |
|--------------------------|----------------------------------------------------------------------------|
| `combined.min.json`      | https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json      |
| `events.min.json`        | https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json        |
| `raids.min.json`         | https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json         |
| `research.min.json`      | https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json      |
| `eggs.min.json`          | https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json          |
| `rocketLineups.min.json` | https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json |

---

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

---

### events.min.json

Array of event objects representing current and upcoming Pokemon GO events.

#### Example Event Object

```json
{
  "eventID": "winter-weekend-2025",
  "name": "Winter Weekend",
  "eventType": "event",
  "heading": "Event",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2025/...",
  "start": "2025-12-27T10:00:00.000",
  "end": "2025-12-28T20:00:00.000",
  "timezone": "Local Time",
  "extraData": {
    "event": {
      "bonuses": [
        {
          "text": "There's no daily limit for how many GO Points you can earn!",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/none.png"
        }
      ],
      "features": [],
      "shinies": [],
      "customSections": {}
    }
  }
}
```

#### Fields

| Field | Type | Description |
|-------|------|-------------|
| `eventID` | `string` | Unique identifier for the event. |
| `name` | `string` | The display name of the event. |
| `eventType` | `string` | The type/category of the event. Determines the structure of `extraData`. |
| `heading` | `string` | The display heading for the event. |
| `image` | `string` | URL to the header/thumbnail image for the event. |
| `start` | `string` | The start date/time of the event in ISO 8601 format. |
| `end` | `string` | The end date/time of the event in ISO 8601 format. |
| `timezone` | `string\|null` | The timezone context. `"Local Time"` for local events, `null` for UTC. |
| `extraData` | `object\|null` | Detailed information specific to the `eventType`. Can be `null`. |

#### Event Types

| Event Type | Description |
|------------|-------------|
| `event` | General event type. |
| `pokestop-showcase` | PokéStop Showcases. |
| `max-mondays` | Dynamax events on Mondays. |
| `go-pass` | Seasonal or monthly pass progression. |
| `timed-research` | Limited-time research events. |
| `go-battle-league` | GBL updates and cups. |
| `pokemon-spotlight-hour` | Weekly spotlight hours. |
| `raid-battles` | Raid boss rotations. |
| `raid-hour` | Weekly raid hours. |
| `community-day` | Community Day events. |
| `raid-day` | Special Raid Day events. |
| `team-go-rocket` | Team Rocket takeovers. |
| `max-battles` | Max Battle events. |
| `pokemon-go-tour` | Major GO Tour events. |
| `research` | Masterwork or paid research. |
| `season` | Seasonal overviews. |

#### Note on Start/End Dates

The `start` and `end` fields are DateTime strings in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format. Most Pokemon GO events occur based on the user's local timezone. The `timezone` field indicates `"Local Time"` for local events. If `timezone` is `null`, the times are in UTC.

#### Extra Data Structures

The `extraData` object varies based on `eventType`. The key inside `extraData` typically matches the event type (with dashes removed).

##### Event (`event`)

```json
"extraData": {
  "event": {
    "bonuses": [{ "text": "...", "image": "..." }],
    "bonusDisclaimers": [],
    "features": [],
    "shinies": [],
    "customSections": {
      "spawns": { "paragraphs": [], "lists": [], "pokemon": [] },
      "eggs": { ... },
      "raids": { ... },
      "research": { ... }
    }
  }
}
```

##### Community Day (`communityday`)

```json
"extraData": {
  "communityday": {
    "spawns": [{ "name": "Piplup", "image": "...", "canBeShiny": true }],
    "bonuses": [{ "text": "Increased Spawns", "image": "..." }],
    "bonusDisclaimers": [],
    "shinies": [{ "name": "Piplup", "image": "...", "canBeShiny": false }],
    "specialresearch": [
      {
        "step": 1,
        "name": "Community Day Classic: Piplup (1/3)",
        "tasks": [{ "text": "Catch 3 Pokémon", "reward": { "text": "Piplup", "image": "..." } }],
        "rewards": [{ "text": "Piplup", "image": "..." }]
      }
    ]
  }
}
```

##### Raid Battles (`raidbattles`)

```json
"extraData": {
  "raidbattles": {
    "bosses": [{ "name": "Mega Glalie", "image": "...", "canBeShiny": true }],
    "shinies": [{ "name": "Glalie", "image": "...", "canBeShiny": false }],
    "tiers": { "mega": [], "fiveStar": [], "threeStar": [], "oneStar": [] },
    "alternationPattern": "",
    "featuredAttacks": []
  }
}
```

##### Spotlight Hour (`spotlight`)

```json
"extraData": {
  "spotlight": {
    "name": "Delibird",
    "canBeShiny": true,
    "image": "...",
    "bonus": "2× Catch Stardust",
    "list": [{ "name": "Delibird", "canBeShiny": true, "image": "..." }]
  }
}
```

##### Max Mondays (`maxmondays`)

```json
"extraData": {
  "maxmondays": {
    "featured": { "name": "Omanyte", "image": "...", "canBeShiny": true },
    "bonus": "December 29, 2025"
  }
}
```

##### PokéStop Showcase (`pokestopshowcase`)

```json
"extraData": {
  "pokestopshowcase": {
    "featured": [{ "name": "Eevee", "image": "...", "canBeShiny": false }],
    "description": "..."
  }
}
```

##### Timed Research (`timedresearch`)

```json
"extraData": {
  "timedresearch": {
    "name": "Best Friends Forever Timed Research",
    "description": "Event-exclusive Timed Research",
    "isPaid": false,
    "price": null,
    "tasks": [
      {
        "step": 1,
        "name": "Best Friends Forever Timed Research (1/4)",
        "tasks": [{ "text": "Make a new friend", "reward": { ... } }],
        "rewards": [{ "text": "×10", "image": "..." }]
      }
    ],
    "availability": {
      "start": "Monday, December 15, at 10:00 a.m.",
      "end": "Monday, December 29, 2025, at 8:00 p.m."
    }
  }
}
```

##### Raid Hour (`raidhour`)

```json
"extraData": {
  "raidhour": {
    "featured": { "name": "Blacephalon Raid Hour", "image": "", "canBeShiny": false }
  }
}
```

##### Raid Day (`raidday`)

```json
"extraData": {
  "raidday": {
    "featured": [{ "name": "Reshiram", "image": "...", "canBeShiny": true }],
    "bonuses": [{ "text": "...", "image": "..." }],
    "shinies": [{ "name": "Reshiram", "image": "...", "canBeShiny": false }]
  }
}
```

##### Research / Masterwork (`research`)

```json
"extraData": {
  "research": {
    "name": "Masterwork Research: A Precious Catch",
    "researchType": "masterwork",
    "isPaid": true,
    "price": 7.99,
    "description": "Ticket-exclusive Masterwork Research",
    "tasks": [
      { "step": 1, "name": "Masterwork Research: A Precious Catch (1/?)", "tasks": [], "rewards": [] }
    ]
  }
}
```

##### GO Tour (`gotour`)

```json
"extraData": {
  "gotour": {
    "eventInfo": { "name": "...", "location": "...", "ticketPrice": null },
    "habitats": { "General": { "info": "...", "spawns": [] } },
    "eggs": { "2km": [], "5km": [], "7km": [], "10km": [] },
    "raids": { "oneStar": [], "threeStar": [], "fiveStar": [], "mega": [] },
    "shinies": [],
    "sales": []
  }
}
```

##### Season (`season`)

```json
"extraData": {
  "season": {
    "name": "Precious Paths",
    "eggs": { "2km": [], "5km": [], "route": [], "adventure": [] },
    "specialResearch": ["..."],
    "features": ["..."]
  }
}
```

---

### raids.min.json

Array of raid boss objects representing current raid bosses in Pokemon GO.

#### Example Raid Object

```json
{
  "name": "Alolan Sandshrew",
  "tier": "1-Star Raids",
  "canBeShiny": true,
  "types": [
    { "name": "ice", "image": "https://leekduck.com/assets/img/types/ice.png" },
    { "name": "steel", "image": "https://leekduck.com/assets/img/types/steel.png" }
  ],
  "combatPower": {
    "normal": { "min": 688, "max": 739 },
    "boosted": { "min": 860, "max": 924 }
  },
  "boostedWeather": [
    { "name": "snow", "image": "https://leekduck.com/assets/img/weather/snowy.png" }
  ],
  "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm27.fALOLA.icon.png"
}
```

#### Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | The name of the Raid Boss. |
| `tier` | `string` | The difficulty tier of the raid. |
| `canBeShiny` | `boolean` | Indicates if the Raid Boss can be encountered in its shiny form. |
| `types` | `array` | An array of type objects representing the Pokémon's types. |
| `combatPower` | `object` | Details about the catch CP range. |
| `boostedWeather` | `array` | An array of weather objects representing conditions that boost this boss. |
| `image` | `string` | URL to the Pokémon's icon image. |

#### Raid Tiers

| Tier | Description |
|------|-------------|
| `1-Star Raids` | Easy solo raids. |
| `3-Star Raids` | Moderate difficulty raids. |
| `5-Star Raids` | Legendary raids requiring groups. |
| `Mega Raids` | Mega Evolution raids. |

#### Type Object

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | The name of the type (e.g., "ice", "fire", "steel"). |
| `image` | `string` | URL to the type icon image. |

#### Combat Power Object

| Field | Type | Description |
|-------|------|-------------|
| `normal` | `object` | Object containing `min` and `max` CP for a standard catch (Level 20). |
| `boosted` | `object` | Object containing `min` and `max` CP for a weather-boosted catch (Level 25). |

#### Boosted Weather Object

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | The name of the weather condition (e.g., "snow", "partly cloudy", "windy"). |
| `image` | `string` | URL to the weather icon image. |

---

### research.min.json

Object containing seasonal research information and an array of field research tasks.

#### Example Research Object

```json
{
  "seasonalInfo": {
    "breakthroughPokemon": ["Galarian Mr. Mime"],
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
          "combatPower": { "min": 515, "max": 553 }
        }
      ]
    }
  ]
}
```

#### Top-Level Fields

| Field | Type | Description |
|-------|------|-------------|
| `seasonalInfo` | `object` | Information pertinent to the current season's research rotation. |
| `tasks` | `array` | A list of active field research tasks. |

#### Seasonal Info Object

| Field | Type | Description |
|-------|------|-------------|
| `breakthroughPokemon` | `array` | List of Pokémon names available in the weekly Research Breakthrough box. |
| `spindaPatterns` | `array` | List of numbers representing available Spinda patterns. |
| `season` | `string\|null` | The name of the current season (can be `null`). |

#### Task Object

| Field | Type | Description |
|-------|------|-------------|
| `text` | `string` | The text description of the task (e.g., "Catch 5 Fire-type Pokémon"). |
| `type` | `string\|undefined` | The category of the task (e.g., "catch", "throw", "battle", "explore", "buddy"). Optional field. |
| `rewards` | `array` | A list of possible rewards for completing this task. |

#### Reward Object

The `rewards` array contains objects that represent either an **item** or a **Pokémon encounter**.

##### Item Reward

| Field | Type | Description |
|-------|------|-------------|
| `type` | `string` | Always `"item"`. |
| `name` | `string` | The name/description of the item. |
| `image` | `string` | URL to the item icon image. |
| `quantity` | `number` | The quantity of the item rewarded. |

##### Encounter Reward

| Field | Type | Description |
|-------|------|-------------|
| `type` | `string` | Always `"encounter"`. |
| `name` | `string` | The name of the Pokémon. |
| `image` | `string` | URL to the Pokémon icon image. |
| `canBeShiny` | `boolean` | Indicates if the Pokémon can be shiny. |
| `combatPower` | `object` | Object containing `min` and `max` CP for the encounter. |

---

### eggs.min.json

Array of Pokémon objects representing all Pokémon currently available from eggs.

#### Example Egg Object

```json
{
  "name": "Bulbasaur",
  "eggType": "1 km",
  "isAdventureSync": false,
  "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm1.icon.png",
  "canBeShiny": true,
  "combatPower": { "min": 637, "max": 637 },
  "isRegional": false,
  "isGiftExchange": false,
  "rarity": 4,
  "rarityTier": "Very Rare"
}
```

#### Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | The name of the Pokémon. |
| `eggType` | `string` | The distance required to hatch the egg. |
| `isAdventureSync` | `boolean` | Indicates if this egg is obtained specifically through Adventure Sync rewards. |
| `image` | `string` | URL to the Pokémon's icon image. |
| `canBeShiny` | `boolean` | Indicates if the Pokémon can be encountered in its shiny form from this egg. |
| `combatPower` | `object` | Object containing the CP range (`min` and `max`) for the hatched Pokémon. |
| `isRegional` | `boolean` | Indicates if this Pokémon is exclusive to a specific region. |
| `isGiftExchange` | `boolean` | Indicates if this egg is obtained by opening Gifts from friends (typically 7km eggs). |
| `rarity` | `number` | A numeric representation of rarity (1 to 5). |
| `rarityTier` | `string` | A text description of the rarity tier. |

#### Egg Types

| Egg Type | Description |
|----------|-------------|
| `1 km` | 1 kilometer eggs. |
| `2 km` | 2 kilometer eggs. |
| `5 km` | 5 kilometer eggs. |
| `7 km` | 7 kilometer eggs (from Gifts). |
| `10 km` | 10 kilometer eggs. |
| `12 km` | 12 kilometer "Strange" eggs (from Team Rocket). |

#### Rarity Tiers

| Rarity Level | Tier Name |
|--------------|-----------|
| 1 | Common |
| 2 | Uncommon |
| 3 | Rare |
| 4 | Very Rare |
| 5 | Ultra Rare |

---

### rocketLineups.min.json

Array of Team Rocket member objects representing battle lineups for Grunts, Leaders, and Giovanni.

#### Example Rocket Lineup Object

```json
{
  "name": "Cliff",
  "title": "Team Rocket Leader",
  "type": "",
  "gender": "Male",
  "firstPokemon": [
    {
      "name": "Larvitar",
      "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm246.icon.png",
      "types": ["rock", "ground"],
      "isEncounter": true,
      "canBeShiny": true
    }
  ],
  "secondPokemon": [
    {
      "name": "Annihilape",
      "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm979.icon.png",
      "types": ["fighting", "ghost"],
      "isEncounter": false,
      "canBeShiny": false
    }
  ],
  "thirdPokemon": [
    {
      "name": "Tyranitar",
      "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm248.icon.png",
      "types": ["rock", "dark"],
      "isEncounter": false,
      "canBeShiny": false
    }
  ]
}
```

#### Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | The name of the Team Rocket member. |
| `title` | `string` | The rank or title of the member. |
| `type` | `string` | The type specialty of the member (e.g., "fire", "water"). Empty for Leaders/Boss. |
| `gender` | `string` | The gender of the member (`"Male"` or `"Female"`). |
| `firstPokemon` | `array` | List of possible Pokémon in the first battle slot. |
| `secondPokemon` | `array` | List of possible Pokémon in the second battle slot. |
| `thirdPokemon` | `array` | List of possible Pokémon in the third battle slot. |

#### Rocket Titles

| Title | Description |
|-------|-------------|
| `Team Rocket Boss` | Giovanni. |
| `Team Rocket Leader` | Cliff, Sierra, or Arlo. |
| `Team Rocket Grunt` | Type-themed grunts. |

#### Pokemon Object (in lineup arrays)

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | The name of the Shadow Pokémon. |
| `image` | `string` | URL to the Pokémon's icon image. |
| `types` | `array` | List of strings representing the Pokémon's types (e.g., `["rock", "ground"]`). |
| `isEncounter` | `boolean` | Indicates if this Pokémon can be encountered (rescued) after defeating the member. |
| `canBeShiny` | `boolean` | Indicates if the Shadow Pokémon can be shiny. |

---

## Usage Notes

- All dates/times are in ISO 8601 format
- Events with `"timezone": "Local Time"` occur at the specified time in each player's local timezone
- Events with `"timezone": null` use UTC time
- Image URLs point to external CDN sources
- Shiny availability indicated by `canBeShiny` boolean field
- CP ranges represent catch/hatch values, not max possible CP
- Data is automatically updated via web scraper; object counts will vary


# Endpoints

- Minimized: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json)

**Note:** The endpoint URL uses the jsdelivr CDN for the quantNebula/data repo.

# Example Combined Object

```json
{
    "events": [
        {
            "eventID": "winter-weekend-2025",
            "name": "Winter Weekend",
            "eventType": "event",
            "...": "..."
        }
    ],
    "raids": [
        {
            "name": "Alolan Sandshrew",
            "tier": "1-Star Raids",
            "...": "..."
        }
    ],
    "research": {
        "seasonalInfo": { ... },
        "tasks": [ ... ]
    },
    "eggs": [
        {
            "name": "Bulbasaur",
            "eggType": "1 km",
            "...": "..."
        }
    ],
    "rocketLineups": [
        {
            "name": "Cliff",
            "title": "Team GO Rocket Leader",
            "...": "..."
        }
    ],
    "event-links": {
        "winter-weekend-2025": "https://leekduck.com/events/winter-weekend-2025/",
        "...": "..."
    }
}
```

# Description

This file aggregates the data from all other individual JSON files into a single response object. This allows clients to fetch all data in one request.

# Root Keys

| Key | Type | Schema Reference | Description |
|---|---|---|---|
| **`events`** | `array` | [events.md](events.md) | List of all current and upcoming events. |
| **`raids`** | `array` | [raids.md](raids.md) | List of current Raid Bosses. |
| **`research`** | `object` | [research.md](research.md) | Current field research tasks and seasonal info. |
| **`eggs`** | `array` | [eggs.md](eggs.md) | List of Pokémon currently hatching from eggs. |
| **`rocketLineups`** | `array` | [rocketLineups.md](rocketLineups.md) | Current Team GO Rocket member lineups. |
| **`event-links`** | `object` | [event-links.md](event-links.md) | Map of event IDs to LeekDuck URLs. |

---

# Events Array Details

The `events` array contains objects representing current and upcoming Pokémon GO events.

## Event Object Fields

| Field           | Type     | Description
|---------------- |--------- |---------------------
| **`eventID`**   | `string` | The unique identifier for the event, which appears as the last segment in the LeekDuck event URL path.
| **`name`**      | `string` | The name of the event.
| **`eventType`** | `string` | The type of the event. This determines the structure of `extraData`. See [List of Event Types](#list-of-event-types).
| **`heading`**   | `string` | The display heading for the event.
| **`image`**     | `string` | URL to the header/thumbnail image for the event.
| **`start`**     | `string` | The start date of the event in ISO 8601 format. See [Note for Start/End dates](#note-for-startend-dates).
| **`end`**       | `string` | The end date of the event in ISO 8601 format. See [Note for Start/End dates](#note-for-startend-dates).
| **`timezone`**  | `string` | The timezone context (e.g., "Local Time").
| **`extraData`** | `object` | Detailed information specific to the `eventType`. Can be `null`. See [Extra Data](#extra-data).

## List of Event Types

| Event Type | Description |
|---|---|
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
| `team-go-rocket` | Team GO Rocket takeovers. |
| `max-battles` | Max Battle events. |
| `pokemon-go-tour` | Major GO Tour events. |
| `research` | Masterwork or paid research. |
| `season` | Seasonal overviews. |

## Note for Start/End dates

The `start` and `end` fields are DateTime objects encoded in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

Most events in Pokémon GO occur based around a user's local timezone. The `timezone` field typically indicates "Local Time". If the event happens at a specific global time, the ISO string may end in `Z` (UTC), though typically the scraper provides the raw string from the source.

## Extra Data

For some event types, `extraData` contains specific structures. The key inside `extraData` usually matches the `eventType` (with dashes removed or generic naming).

### Event (`event`)
Generic events often include bonuses, spawns, and shinies.

```json
"extraData": {
    "event": {
        "bonuses": [ { "text": "...", "image": "..." } ],
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

### Community Day (`communityday`)

```json
"extraData": {
    "communityday": {
        "spawns": [ { "name": "Piplup", "image": "...", "canBeShiny": true } ],
        "bonuses": [ { "text": "Increased Spawns", "image": "..." } ],
        "bonusDisclaimers": [],
        "shinies": [ { "name": "Piplup", "image": "...", "canBeShiny": false } ],
        "specialresearch": [
            {
                "step": 1,
                "name": "Community Day Classic: Piplup (1/3)",
                "tasks": [
                    {
                        "text": "Catch 3 Pokémon",
                        "reward": { "text": " Piplup ", "image": "..." }
                    }
                ],
                "rewards": [ { "text": "Piplup", "image": "..." } ]
            }
        ]
    }
}
```

### Raid Battles (`raidbattles`)

Used for specific raid events or rotations (e.g., Mega Raids, 5-Star rotations).

```json
"extraData": {
    "raidbattles": {
        "bosses": [
            { "name": "Mega Glalie", "image": "...", "canBeShiny": true }
        ],
        "shinies": [
            { "name": "Glalie", "image": "...", "canBeShiny": false }
        ],
        "tiers": {
            "mega": [],
            "fiveStar": [],
            "threeStar": [],
            "oneStar": []
        },
        "alternationPattern": "",
        "featuredAttacks": []
    }
}
```

### Pokémon Spotlight Hour (`spotlight`)

```json
"extraData": {
    "spotlight": {
        "name": "Delibird",
        "canBeShiny": true,
        "image": "...",
        "bonus": "2× Catch Stardust",
        "list": [
            { "name": "Delibird", "canBeShiny": true, "image": "..." }
        ]
    }
}
```

### Max Mondays (`maxmondays`)

```json
"extraData": {
    "maxmondays": {
        "featured": {
            "name": "Omanyte",
            "image": "...",
            "canBeShiny": true
        },
        "bonus": "December 29, 2025"
    }
}
```

### PokéStop Showcase (`pokestopshowcase`)

```json
"extraData": {
    "pokestopshowcase": {
        "featured": [
            { "name": "Eevee", "image": "...", "canBeShiny": false }
        ],
        "description": "There will be PokéStop Showcases featuring Eevee. Top the leaderboard to win rewards including Premium Items!  Eevee"
    }
}
```

### Timed Research (`timedresearch`)

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
                "tasks": [ { "text": "Make a new friend", "reward": { ... } } ],
                "rewards": [ { "text": "×10", "image": "..." } ]
            }
        ],
        "availability": {
            "start": "Monday, December 15, at 10:00 a.m.",
            "end": "Monday, December 29, 2025, at 8:00 p.m."
        }
    }
}
```

### Raid Hour (`raidhour`)

```json
"extraData": {
    "raidhour": {
        "featured": {
            "name": "Blacephalon Raid Hour",
            "image": "",
            "canBeShiny": false
        }
    }
}
```

### Raid Day (`raidday`)

```json
"extraData": {
    "raidday": {
        "featured": [
            { "name": "Reshiram", "image": "...", "canBeShiny": true }
        ],
        "bonuses": [ { "text": "...", "image": "..." } ],
        "shinies": [ { "name": "Reshiram", "image": "...", "canBeShiny": false } ]
    }
}
```

### Research / Masterwork (`research`)

```json
"extraData": {
    "research": {
        "name": "Masterwork Research: A Precious Catch",
        "researchType": "masterwork",
        "isPaid": true,
        "price": 7.99,
        "description": "Ticket-exclusive Masterwork Research",
        "tasks": [
            {
                "step": 1,
                "name": "Masterwork Research: A Precious Catch (1/?)",
                "tasks": [],
                "rewards": []
            }
        ]
    }
}
```

### GO Tour (`gotour`)

```json
"extraData": {
    "gotour": {
        "eventInfo": { "name": "...", "location": "...", "ticketPrice": null },
        "habitats": {
            "General": { "info": "...", "spawns": [] }
        },
        "eggs": { "2km": [], "5km": [], "7km": [], "10km": [] },
        "raids": { "oneStar": [], "threeStar": [], "fiveStar": [], "mega": [] },
        "shinies": [],
        "sales": []
    }
}
```

### Season (`season`)

```json
"extraData": {
    "season": {
        "name": "Precious Paths",
        "eggs": {
            "2km": [],
            "5km": [],
            "route": [],
            "adventure": []
        },
        "specialResearch": [ "..." ],
        "features": [ "..." ]
    }
}
```

---

# Raids Array Details

The `raids` array contains objects representing current Raid Bosses in Pokémon GO.

## Raid Object Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`name`**          | `string`  | The name of the Raid Boss.
| **`tier`**          | `string`  | The difficulty tier of the raid. See [Raid Tiers](#raid-tiers).
| **`canBeShiny`**    | `boolean` | Indicates if the Raid Boss can be encountered in its shiny form.
| **`types`**         | `array`   | An array of objects representing the Pokémon's types.
| **`combatPower`**   | `object`  | Details about the Catch CP range. See [Combat Power Object](#combat-power-object).
| **`boostedWeather`**| `array`   | An array of objects representing the weather conditions that boost this boss.
| **`image`**         | `string`  | URL to the Pokémon's icon image.

## Raid Tiers

The `tier` field identifies the difficulty or category of the raid. Observed values include:

- `1-Star Raids`
- `3-Star Raids`
- `5-Star Raids`
- `Mega Raids`

## Raid Sub-Objects

### Type Object
| Field | Type | Description |
|---|---|---|
| **`name`** | `string` | The name of the type (e.g., "ice", "fire", "steel"). |
| **`image`** | `string` | URL to the type icon image. |

### Combat Power Object
| Field | Type | Description |
|---|---|---|
| **`normal`** | `object` | Object containing `min` and `max` CP for a standard catch (Level 20). |
| **`boosted`** | `object` | Object containing `min` and `max` CP for a weather-boosted catch (Level 25). |

### Boosted Weather Object
| Field | Type | Description |
|---|---|---|
| **`name`** | `string` | The name of the weather condition (e.g., "snow", "partly cloudy", "windy"). |
| **`image`** | `string` | URL to the weather icon image. |

---

# Research Object Details

The `research` object contains information about current field research tasks and seasonal information.

## Research Root Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`seasonalInfo`**  | `object`  | Information pertinent to the current season's research rotation. See [Seasonal Info](#seasonal-info).
| **`tasks`**         | `array`   | A list of active field research tasks. See [Task Object](#task-object).

## Seasonal Info

| Field | Type | Description |
|---|---|---|
| **`breakthroughPokemon`** | `array` | List of strings representing Pokémon currently available in the weekly Research Breakthrough box. |
| **`spindaPatterns`** | `array` | List of numbers representing available Spinda patterns. |
| **`season`** | `string` | The name of the current season (can be null). |

## Task Object

| Field | Type | Description |
|---|---|---|
| **`text`** | `string` | The text description of the task (e.g., "Catch 5 Fire-type Pokémon"). |
| **`type`** | `string` | The category of the task (e.g., "catch", "throw", "battle", "explore", "buddy"). Optional. |
| **`rewards`** | `array` | A list of possible rewards for completing this task. See [Reward Object](#reward-object). |

## Reward Object

The `rewards` array contains objects that can represent either an item or a Pokémon encounter.

| Field | Type | Description |
|---|---|---|
| **`type`** | `string` | The type of reward: `"item"` or `"encounter"`. |
| **`name`** | `string` | The name of the reward item or Pokémon. |
| **`image`** | `string` | URL to the reward image. |
| **`quantity`** | `number` | (Item only) The quantity of the item rewarded. |
| **`canBeShiny`** | `boolean` | (Encounter only) Indicates if the Pokémon can be shiny. |
| **`combatPower`** | `object` | (Encounter only) The CP range (`min`, `max`) for the encounter. |

---

# Eggs Array Details

The `eggs` array contains objects representing Pokémon currently hatching from eggs.

## Egg Object Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`name`**          | `string`  | The name of the Pokémon.
| **`eggType`**       | `string`  | The distance required to hatch the egg. See [Egg Types](#egg-types).
| **`isAdventureSync`**| `boolean` | Indicates if this egg is obtained specifically through Adventure Sync rewards.
| **`image`**         | `string`  | URL to the Pokémon's icon image.
| **`canBeShiny`**    | `boolean` | Indicates if the Pokémon can be encountered in its shiny form from this egg.
| **`combatPower`**   | `object`  | An object containing the CP range for the hatched Pokémon (`min` and `max`).
| **`isRegional`**    | `boolean` | Indicates if this Pokémon is exclusive to a specific region.
| **`isGiftExchange`**| `boolean` | Indicates if this egg is obtained by opening Gifts from friends (typically 7km eggs).
| **`rarity`**        | `number`  | A numeric representation of rarity (1 to 5).
| **`rarityTier`**    | `string`  | A text description of the rarity tier. See [Rarity Tiers](#rarity-tiers).

## Egg Types

The `eggType` field represents the walking distance group or category of the egg. Observed values:

- `1 km`
- `2 km`
- `5 km`
- `7 km`
- `10 km`
- `12 km`

## Rarity Tiers

Rarity is expressed both as a number (`rarity`) and a human-readable string (`rarityTier`).

| Rarity Level | Tier Name |
|---|---|
| 1 | Common |
| 2 | Uncommon |
| 3 | Rare |
| 4 | Very Rare |
| 5 | Ultra Rare |

---

# Rocket Lineups Array Details

The `rocketLineups` array contains objects representing current Team GO Rocket member lineups.

## Rocket Lineup Object Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`name`**          | `string`  | The name of the Team GO Rocket member.
| **`title`**         | `string`  | The rank or title of the member. See [Rocket Titles](#rocket-titles).
| **`type`**          | `string`  | The type specialty of the member (e.g., "fire", "water"). Empty for Leaders/Boss.
| **`gender`**        | `string`  | The gender of the member ("Male" or "Female").
| **`firstPokemon`**  | `array`   | List of possible Pokémon in the first slot. See [Pokemon Object](#pokemon-object).
| **`secondPokemon`** | `array`   | List of possible Pokémon in the second slot. See [Pokemon Object](#pokemon-object).
| **`thirdPokemon`**  | `array`   | List of possible Pokémon in the third slot. See [Pokemon Object](#pokemon-object).

## Rocket Titles

Observed titles include:

- `Team GO Rocket Boss`
- `Team GO Rocket Leader`
- `Team GO Rocket Grunt`

## Pokemon Object

| Field | Type | Description |
|---|---|---|
| **`name`** | `string` | The name of the Shadow Pokémon. |
| **`image`** | `string` | URL to the Pokémon's icon image. |
| **`types`** | `array` | List of strings representing the Pokémon's types (e.g., `["rock", "ground"]`). |
| **`isEncounter`** | `boolean` | Indicates if this Pokémon can be encountered (rescued) after defeating the member. |
| **`canBeShiny`** | `boolean` | Indicates if the Shadow Pokémon can be shiny. |

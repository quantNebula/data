# Pokémon GO Data API Documentation

This repository provides structured JSON data for Pokémon GO game mechanics, updated automatically every four hours to reflect the current game state.

## Available Endpoints

| Endpoint | Description |
|----------|-------------|
| `https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json` | Egg pool data including distance tiers, Pokémon availability, and rarity |
| `https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json` | Event schedules, bonuses, featured Pokémon, and special mechanics |
| `https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json` | Field research tasks, rewards, and seasonal breakthrough encounters |
| `https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json` | Team GO Rocket battle lineups for Giovanni, Leaders, and Grunts |

---

## Quick Start

### Accessing the Data

All data files are available via CDN:

```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/{filename}
```

### Parallel Data Loading

Load multiple data sources simultaneously for better performance:

```javascript
async function loadAllData() {
  const [eggs, events, research, rocketLineups] = await Promise.all([
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json').then(r => r.json()),
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json').then(r => r.json()),
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json').then(r => r.json()),
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json').then(r => r.json())
  ]);

  return { eggs, events, research, rocketLineups };
}

loadAllData().then(data => {
  console.log('All data loaded:', data);
});
```

---

## Eggs Endpoint

**URL:** `https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json`

The eggs endpoint contains comprehensive data about the Pokémon egg pool in Pokémon GO. This dataset describes which Pokémon can hatch from different egg types, their rarity, shiny availability, and other relevant properties.

### Usage Example

```javascript
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json')
  .then(response => response.json())
  .then(eggs => {
    console.log(`Loaded ${eggs.length} egg entries`);
  });
```

### Data Structure

The file contains a JSON array of Pokémon objects, where each object represents a specific Pokémon (or form variant) available from eggs.

```json
[
  {
    "name": "Bulbasaur",
    "eggType": "2 km",
    "isAdventureSync": false,
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm1.icon.png",
    "imageWidth": 107,
    "imageHeight": 126,
    "canBeShiny": true,
    "combatPower": {
      "min": 637,
      "max": 637
    },
    "isRegional": false,
    "isGiftExchange": false,
    "rarity": 4,
    "rarityTier": "Very Rare"
  }
]
```

### JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Egg Data Schema",
  "description": "Schema for Pokémon GO egg pool data",
  "type": "array",
  "items": {
    "type": "object",
    "required": [
      "name",
      "eggType",
      "isAdventureSync",
      "image",
      "imageWidth",
      "imageHeight",
      "canBeShiny",
      "combatPower",
      "isRegional",
      "isGiftExchange",
      "rarity",
      "rarityTier"
    ],
    "properties": {
      "name": {
        "type": "string",
        "description": "The name of the Pokémon. May include form variations (e.g., 'Galarian Meowth', 'Indeedee (Male)')"
      },
      "eggType": {
        "type": "string",
        "description": "The type of egg this Pokémon can hatch from",
        "enum": ["1 km", "2 km", "5 km", "7 km", "10 km", "12 km"]
      },
      "isAdventureSync": {
        "type": "boolean",
        "description": "Whether this Pokémon is exclusive to Adventure Sync reward eggs"
      },
      "image": {
        "type": "string",
        "format": "uri",
        "description": "URL to the Pokémon's icon image"
      },
      "imageWidth": {
        "type": "integer",
        "description": "Width of the icon image in pixels",
        "minimum": 1
      },
      "imageHeight": {
        "type": "integer",
        "description": "Height of the icon image in pixels",
        "minimum": 1
      },
      "canBeShiny": {
        "type": "boolean",
        "description": "Whether this Pokémon can be encountered in its shiny form when hatched"
      },
      "combatPower": {
        "type": "object",
        "description": "The Combat Power (CP) range for this Pokémon when hatched",
        "required": ["min", "max"],
        "properties": {
          "min": {
            "type": "integer",
            "description": "Minimum Combat Power",
            "minimum": 0
          },
          "max": {
            "type": "integer",
            "description": "Maximum Combat Power",
            "minimum": 0
          }
        }
      },
      "isRegional": {
        "type": "boolean",
        "description": "Whether this Pokémon is normally region-locked"
      },
      "isGiftExchange": {
        "type": "boolean",
        "description": "Whether this Pokémon can only be obtained from 7 km eggs received through friend gifts"
      },
      "rarity": {
        "type": "integer",
        "description": "Numeric rarity value (1-5)",
        "minimum": 1,
        "maximum": 5
      },
      "rarityTier": {
        "type": "string",
        "description": "Human-readable rarity tier",
        "enum": ["Common", "Uncommon", "Rare", "Very Rare", "Ultra Rare"]
      }
    }
  }
}
```

### Property Definitions

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `name` | string | Yes | The Pokémon's name, including form variants (e.g., "Galarian Meowth", "Indeedee (Male)") |
| `eggType` | string | Yes | The egg distance type this Pokémon hatches from |
| `isAdventureSync` | boolean | Yes | Whether this Pokémon is exclusive to Adventure Sync reward eggs |
| `image` | string (URI) | Yes | URL to the Pokémon's icon image |
| `imageWidth` | integer | Yes | Width of the icon image in pixels |
| `imageHeight` | integer | Yes | Height of the icon image in pixels |
| `canBeShiny` | boolean | Yes | Whether this Pokémon can be shiny when hatched |
| `combatPower` | object | Yes | CP range for the hatched Pokémon |
| `isRegional` | boolean | Yes | Whether this Pokémon is normally region-locked |
| `isGiftExchange` | boolean | Yes | Whether this is exclusive to 7 km friend gift eggs |
| `rarity` | integer | Yes | Numeric rarity value (1-5) |
| `rarityTier` | string | Yes | Human-readable rarity tier |

### Egg Types

The `eggType` property indicates which egg distance category the Pokémon hatches from:

- **`"1 km"`** - Starter Pokémon and event-specific eggs
- **`"2 km"`** - Common Pokémon, baby Pokémon, some rare finds
- **`"5 km"`** - Standard Pokémon pool with moderate rarity
- **`"7 km"`** - Regional variants from friend gifts (Alolan, Galarian, Hisuian forms)
- **`"10 km"`** - Rare Pokémon, pseudo-legendaries, and sought-after species
- **`"12 km"`** - Strange Eggs from Team GO Rocket leaders (Dark/Poison/Fighting types)

### Adventure Sync Eggs

When `isAdventureSync` is `true`, the Pokémon **only** hatches from eggs earned through Adventure Sync weekly rewards (walking 25km or 50km in a week), not from regular PokéStop eggs.

### Rarity System

| rarity | rarityTier | Description |
|--------|-----------|-------------|
| 1 | Common | Frequently encountered |
| 2 | Uncommon | Moderately rare |
| 3 | Rare | Less common, desirable |
| 4 | Very Rare | Significantly rare, highly sought |
| 5 | Ultra Rare | Extremely rare, special events |

### Eggs Usage Examples

```javascript
// Get all Pokémon from 10 km eggs
const tenKmPokemon = eggs.filter(p => p.eggType === "10 km");

// Get Adventure Sync exclusives
const adventureSyncPokemon = eggs.filter(p => p.isAdventureSync === true);

// Get all Pokémon that can be shiny from 5 km eggs
const shiny5kmPokemon = eggs.filter(p => 
  p.eggType === "5 km" && p.canBeShiny === true
);

// Get Ultra Rare Pokémon
const ultraRare = eggs.filter(p => p.rarityTier === "Ultra Rare");

// Get all Galarian forms
const galarianForms = eggs.filter(p => p.name.startsWith("Galarian"));

// Group all Pokémon by egg type
const byEggType = eggs.reduce((acc, pokemon) => {
  const key = pokemon.eggType;
  if (!acc[key]) acc[key] = [];
  acc[key].push(pokemon);
  return acc;
}, {});
```

---

## Events Endpoint

**URL:** `https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json`

The events endpoint contains comprehensive data about ongoing and upcoming events in Pokémon GO. This dataset describes event schedules, bonuses, featured Pokémon, raids, research tasks, and special mechanics.

### Usage Example

```javascript
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json')
  .then(response => response.json())
  .then(events => {
    console.log(`Loaded ${events.length} events`);
    const currentEvents = events.filter(e => 
      new Date(e.start) <= new Date() && new Date(e.end) >= new Date()
    );
  });
```

### Data Structure

The file contains a JSON array of event objects, each with core properties and optional `extraData` specific to the event type.

```json
[
  {
    "eventID": "unique-event-id",
    "name": "Event Name",
    "eventType": "event",
    "heading": "Event",
    "image": "https://...",
    "imageWidth": 1280,
    "imageHeight": 720,
    "start": "2026-01-06T10:00:00.000",
    "end": "2026-01-11T20:00:00.000",
    "timezone": "Local Time",
    "extraData": {...}
  }
]
```

### JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Events Data Schema",
  "description": "Schema for Pokémon GO event data",
  "type": "array",
  "items": {
    "type": "object",
    "required": ["eventID", "name", "eventType", "heading", "image", "start", "end"],
    "properties": {
      "eventID": {
        "type": "string",
        "description": "Unique identifier for the event"
      },
      "name": {
        "type": "string",
        "description": "Display name of the event"
      },
      "eventType": {
        "type": "string",
        "description": "Category of event",
        "enum": [
          "event",
          "community-day",
          "raid-battles",
          "raid-day",
          "raid-hour",
          "pokemon-spotlight-hour",
          "go-battle-league",
          "max-mondays",
          "max-battles",
          "team-go-rocket",
          "pokemon-go-tour",
          "season",
          "go-pass",
          "research"
        ]
      },
      "heading": {
        "type": "string",
        "description": "Category label for display"
      },
      "image": {
        "type": "string",
        "format": "uri",
        "description": "Event banner image URL"
      },
      "imageWidth": {
        "type": "integer",
        "minimum": 1
      },
      "imageHeight": {
        "type": "integer",
        "minimum": 1
      },
      "start": {
        "type": "string",
        "format": "date-time",
        "description": "Event start date/time in ISO 8601 format"
      },
      "end": {
        "type": "string",
        "format": "date-time",
        "description": "Event end date/time in ISO 8601 format"
      },
      "timezone": {
        "type": ["string", "null"],
        "description": "Timezone specification ('Local Time', null for UTC, or specific timezone)"
      },
      "extraData": {
        "type": ["object", "null"],
        "description": "Event-type specific data structure"
      }
    }
  }
}
```

### Property Definitions

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `eventID` | string | Yes | Unique identifier (kebab-case, often includes date) |
| `name` | string | Yes | Display name of the event |
| `eventType` | string | Yes | Event category for filtering and display |
| `heading` | string | Yes | Category label shown in UI |
| `image` | string (URI) | Yes | Event banner/promotional image URL |
| `imageWidth` | integer | Yes | Banner image width in pixels |
| `imageHeight` | integer | Yes | Banner image height in pixels |
| `start` | string (ISO 8601) | Yes | Event start timestamp |
| `end` | string (ISO 8601) | Yes | Event end timestamp |
| `timezone` | string/null | No | Timezone context for start/end times |
| `extraData` | object/null | No | Event-specific detailed information |

### Event Types

- **`event`** - General in-game events with spawns, bonuses, and features
- **`community-day`** - Monthly Community Day events
- **`raid-battles`** - Specific Pokémon appearing in raids
- **`raid-day`** - Special raid event days
- **`raid-hour`** - Weekly raid hour events
- **`pokemon-spotlight-hour`** - Weekly spotlight hour featuring one Pokémon
- **`go-battle-league`** - PvP season/league rotation information
- **`max-mondays`** - Weekly Max Battle featured Pokémon
- **`max-battles`** - Dynamax/Gigantamax battle events
- **`team-go-rocket`** - Team GO Rocket takeover events
- **`pokemon-go-tour`** - Large-scale tour events (in-person and global)
- **`season`** - Seasonal game-wide changes
- **`go-pass`** - GO Pass progression track period
- **`research`** - Special or Masterwork Research availability

### Timezone Handling

The `timezone` field indicates how to interpret start/end times:

- **`"Local Time"`** - Times are in the player's local timezone
- **`null`** - Times are in UTC/GMT (often used for global simultaneous starts)
- **Specific timezone string** - Times are in the specified timezone

### Extra Data Structures

The `extraData` object varies by event type:

**Community Day:**
```json
"extraData": {
  "communityday": {
    "spawns": [...],
    "bonuses": [...],
    "shinies": [...],
    "specialresearch": [...],
    "featuredAttack": "...",
    "ticketedResearch": {...}
  }
}
```

**General Event:**
```json
"extraData": {
  "event": {
    "bonuses": [...],
    "features": [...],
    "shinies": [...],
    "customSections": {
      "spawns": {...},
      "raids": {...},
      "research": {...}
    }
  }
}
```

**Raid Battles:**
```json
"extraData": {
  "raidbattles": {
    "bosses": [...],
    "shinies": [...],
    "tiers": {...},
    "featuredAttacks": [...]
  }
}
```

### Events Usage Examples

```javascript
// Get all currently active events
function getCurrentEvents(events) {
  const now = new Date();
  return events.filter(event => {
    const start = new Date(event.start);
    const end = new Date(event.end);
    return now >= start && now <= end;
  });
}

// Get all Community Days
const communityDays = events.filter(e => e.eventType === 'community-day');

// Get events starting in the next 7 days
const nextWeek = new Date();
nextWeek.setDate(nextWeek.getDate() + 7);

const upcomingEvents = events.filter(event => {
  const start = new Date(event.start);
  return start > new Date() && start <= nextWeek;
}).sort((a, b) => new Date(a.start) - new Date(b.start));

// Get upcoming Spotlight Hours
const spotlightHours = events
  .filter(e => e.eventType === 'pokemon-spotlight-hour')
  .map(e => ({
    date: new Date(e.start),
    pokemon: e.extraData?.spotlight?.name,
    bonus: e.extraData?.spotlight?.bonus
  }))
  .sort((a, b) => a.date - b.date);
```

---

## Research Endpoint

**URL:** `https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json`

The research endpoint contains data about current Field Research tasks in Pokémon GO. This dataset describes available research tasks, their requirements, and possible rewards including Pokémon encounters, items, and Stardust.

### Usage Example

```javascript
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json')
  .then(response => response.json())
  .then(data => {
    console.log(`Loaded ${data.tasks.length} field research tasks`);
  });
```

### Data Structure

The file contains a JSON object with two main sections: seasonal information and an array of research tasks.

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
      "rewards": [...]
    }
  ]
}
```

### JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Field Research Data Schema",
  "description": "Schema for Pokémon GO field research tasks",
  "type": "object",
  "required": ["seasonalInfo", "tasks"],
  "properties": {
    "seasonalInfo": {
      "type": "object",
      "description": "Seasonal metadata about research rewards",
      "properties": {
        "breakthroughPokemon": {
          "type": "array",
          "description": "List of Pokémon that can be encountered from Research Breakthrough rewards",
          "items": {"type": "string"}
        },
        "spindaPatterns": {
          "type": "array",
          "description": "Currently available Spinda pattern numbers",
          "items": {"type": "integer"}
        },
        "season": {
          "type": ["string", "null"],
          "description": "Current season name"
        }
      }
    },
    "tasks": {
      "type": "array",
      "description": "Array of field research task objects",
      "items": {
        "type": "object",
        "required": ["text", "rewards"],
        "properties": {
          "text": {
            "type": "string",
            "description": "The task description as shown to players"
          },
          "type": {
            "type": "string",
            "description": "Category of task",
            "enum": ["catch", "throw", "battle", "explore", "training", "buddy", "rocket", "sponsored", "ar"]
          },
          "rewards": {
            "type": "array",
            "description": "Array of possible rewards for this task",
            "items": {
              "type": "object",
              "required": ["type"],
              "properties": {
                "type": {
                  "type": "string",
                  "description": "Type of reward",
                  "enum": ["encounter", "item"]
                },
                "name": {
                  "type": "string",
                  "description": "Name of the Pokémon or item"
                },
                "image": {
                  "type": "string",
                  "format": "uri",
                  "description": "URL to reward icon"
                },
                "imageWidth": {
                  "type": "integer",
                  "minimum": 1
                },
                "imageHeight": {
                  "type": "integer",
                  "minimum": 1
                },
                "canBeShiny": {
                  "type": "boolean",
                  "description": "Whether this Pokémon can be shiny (encounters only)"
                },
                "combatPower": {
                  "type": "object",
                  "description": "CP range for encounter rewards",
                  "properties": {
                    "min": {"type": "integer", "minimum": 0},
                    "max": {"type": "integer", "minimum": 0}
                  }
                },
                "quantity": {
                  "type": "integer",
                  "description": "Quantity of item reward",
                  "minimum": 1
                }
              }
            }
          }
        }
      }
    }
  }
}
```

### Property Definitions

#### Seasonal Info

| Property | Type | Description |
|----------|------|-------------|
| `breakthroughPokemon` | array | Names of Pokémon available from 7-day Research Breakthrough |
| `spindaPatterns` | array | Pattern numbers for Spinda encounters this period |
| `season` | string/null | Current game season name |

#### Task Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `text` | string | Yes | The task requirement text shown to players |
| `type` | string | No | Task category for filtering purposes |
| `rewards` | array | Yes | Array of possible rewards (one is randomly selected) |

### Task Types

- **`catch`** - Catching Pokémon (specific types, quantities, weather boost)
- **`throw`** - Throwing Pokéballs (Nice, Great, Excellent, Curveball)
- **`battle`** - Raids, Trainer Battles, Team GO Rocket battles
- **`explore`** - Walking, spinning PokéStops, hatching eggs
- **`training`** - Powering up, evolving Pokémon, earning XP/Stardust
- **`buddy`** - Buddy-related activities (hearts, treats, walking)
- **`rocket`** - Team GO Rocket specific tasks
- **`sponsored`** - Tasks from sponsored locations
- **`ar`** - AR scanning tasks

### Reward Properties

| Property | Type | Description |
|----------|------|-------------|
| `type` | string | `"encounter"` for Pokémon or `"item"` for items |
| `name` | string | Pokémon name or item name |
| `image` | string (URI) | URL to the icon image |
| `imageWidth` | integer | Icon width in pixels |
| `imageHeight` | integer | Icon height in pixels |
| `canBeShiny` | boolean | Whether Pokémon can be shiny (encounters only) |
| `combatPower` | object | CP range (encounters only) |
| `quantity` | integer | Item quantity (items only) |

### Understanding Rewards

Most tasks have multiple possible rewards listed. **Only one reward is given** when completing the task - the game randomly selects from the array.

```javascript
// This task can reward ANY ONE of these Pokémon
{
  "text": "Make 3 Great Throws",
  "rewards": [
    {"type": "encounter", "name": "Omanyte", ...},
    {"type": "encounter", "name": "Kabuto", ...},
    {"type": "encounter", "name": "Clamperl", ...}
  ]
}
```

### Research Usage Examples

```javascript
// Get all catch-related tasks
const catchTasks = data.tasks.filter(task => task.type === 'catch');

// Find tasks that reward a specific Pokémon
function findTasksForPokemon(pokemonName) {
  return data.tasks.filter(task =>
    task.rewards.some(reward =>
      reward.type === 'encounter' && 
      reward.name === pokemonName
    )
  );
}

// Get tasks with potential shiny encounters
const shinyTasks = data.tasks.filter(task =>
  task.rewards.some(reward =>
    reward.type === 'encounter' && 
    reward.canBeShiny === true
  )
);

// Get unique list of all Pokémon available from research
const allPokemon = [...new Set(
  data.tasks.flatMap(task =>
    task.rewards
      .filter(r => r.type === 'encounter')
      .map(r => r.name)
  )
)];
```

---

## Rocket Lineups Endpoint

**URL:** `https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json`

The rocket lineups endpoint contains data about Team GO Rocket encounters in Pokémon GO. This dataset describes the Pokémon lineups used by Giovanni, the Rocket Leaders (Cliff, Arlo, Sierra), and various Rocket Grunts, along with which Pokémon can be caught after defeating them.

### Usage Example

```javascript
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json')
  .then(response => response.json())
  .then(lineups => {
    console.log(`Loaded ${lineups.length} Team Rocket lineups`);
    const giovanni = lineups.find(l => l.name === 'Giovanni');
  });
```

### Data Structure

The file contains a JSON array of lineup objects, each representing a different Team GO Rocket member.

```json
[
  {
    "name": "Giovanni",
    "title": "Team GO Rocket Boss",
    "type": "",
    "typeImage": "",
    "gender": "Male",
    "firstPokemon": [...],
    "secondPokemon": [...],
    "thirdPokemon": [...]
  }
]
```

### JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Team Rocket Lineups Schema",
  "description": "Schema for Team GO Rocket battle lineups",
  "type": "array",
  "items": {
    "type": "object",
    "required": ["name", "title", "gender", "firstPokemon", "secondPokemon", "thirdPokemon"],
    "properties": {
      "name": {
        "type": "string",
        "description": "Name of the Rocket member (e.g., 'Giovanni', 'Cliff', 'Fire-type Female Grunt')"
      },
      "title": {
        "type": "string",
        "description": "Title/role of the Rocket member",
        "enum": ["Team GO Rocket Boss", "Team GO Rocket Leader", "Team GO Rocket Grunt"]
      },
      "type": {
        "type": "string",
        "description": "Pokémon type specialization for typed grunts"
      },
      "typeImage": {
        "type": "string",
        "description": "URL to type symbol image (for typed grunts)"
      },
      "typeImageWidth": {
        "type": ["integer", "null"],
        "minimum": 1
      },
      "typeImageHeight": {
        "type": ["integer", "null"],
        "minimum": 1
      },
      "gender": {
        "type": "string",
        "description": "Gender of the Rocket member",
        "enum": ["Male", "Female"]
      },
      "firstPokemon": {
        "type": "array",
        "description": "Possible first slot Pokémon (one is randomly selected)",
        "items": {"$ref": "#/definitions/pokemon"}
      },
      "secondPokemon": {
        "type": "array",
        "description": "Possible second slot Pokémon (one is randomly selected)",
        "items": {"$ref": "#/definitions/pokemon"}
      },
      "thirdPokemon": {
        "type": "array",
        "description": "Possible third slot Pokémon (one is randomly selected)",
        "items": {"$ref": "#/definitions/pokemon"}
      }
    }
  },
  "definitions": {
    "pokemon": {
      "type": "object",
      "required": ["name", "image", "types", "isEncounter", "canBeShiny"],
      "properties": {
        "name": {
          "type": "string",
          "description": "Name of the Pokémon (may include form)"
        },
        "image": {
          "type": "string",
          "format": "uri",
          "description": "URL to Pokémon icon"
        },
        "imageWidth": {
          "type": "integer",
          "minimum": 1
        },
        "imageHeight": {
          "type": "integer",
          "minimum": 1
        },
        "types": {
          "type": "array",
          "description": "Pokémon type(s)",
          "items": {"type": "string"}
        },
        "isEncounter": {
          "type": "boolean",
          "description": "Whether this Pokémon can be caught after winning the battle"
        },
        "canBeShiny": {
          "type": "boolean",
          "description": "Whether this Pokémon can be encountered in shiny form"
        }
      }
    }
  }
}
```

### Property Definitions

#### Lineup Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `name` | string | Yes | Identifier name for the Rocket member |
| `title` | string | Yes | Role: Boss, Leader, or Grunt |
| `type` | string | No | Type specialization (for typed grunts: "fire", "water", etc.) |
| `typeImage` | string | No | URL to type badge icon |
| `typeImageWidth` | integer/null | No | Type badge width in pixels |
| `typeImageHeight` | integer/null | No | Type badge height in pixels |
| `gender` | string | Yes | "Male" or "Female" |
| `firstPokemon` | array | Yes | Possible Pokémon in first battle slot |
| `secondPokemon` | array | Yes | Possible Pokémon in second battle slot |
| `thirdPokemon` | array | Yes | Possible Pokémon in third battle slot |

#### Pokémon Properties

| Property | Type | Description |
|----------|------|-------------|
| `name` | string | Pokémon name (includes form if applicable) |
| `image` | string (URI) | URL to Pokémon icon |
| `imageWidth` | integer | Icon width in pixels |
| `imageHeight` | integer | Icon height in pixels |
| `types` | array | Pokémon types (e.g., ["fire", "flying"]) |
| `isEncounter` | boolean | If `true`, this Pokémon can be caught after defeating the Rocket member |
| `canBeShiny` | boolean | If `true`, the encounter can be shiny |

### Understanding Lineups

#### Battle Structure

Each Rocket member has three Pokémon slots. For each slot:
- One Pokémon is randomly selected from the possible options
- The first slot Pokémon determines potential catch encounters
- Only Pokémon with `isEncounter: true` can be caught

#### Rocket Member Types

**Giovanni (Boss):**
- The leader of Team GO Rocket
- Always has Persian as first Pokémon
- Third Pokémon is usually a Legendary Shadow Pokémon
- Requires Super Rocket Radar to find

**Leaders (Cliff, Arlo, Sierra):**
- Named characters with consistent identities
- Require Rocket Radar to find
- Have specific signature Pokémon
- First Pokémon from these battles can be caught

**Grunts:**
- Generic Team GO Rocket members
- Often specialized by type (Fire, Water, Grass, etc.)
- Gender variants may have different lineups
- Most common encounter type

### Encounter Mechanics

After defeating a Rocket member:
1. Only the **first slot Pokémon** can potentially be caught
2. The Pokémon must have `isEncounter: true`
3. A Premier Ball catch sequence begins
4. The Pokémon may be in its shiny form if `canBeShiny: true`

### Rocket Lineups Usage Examples

```javascript
// Find Giovanni's lineup
const giovanni = lineups.find(lineup => lineup.name === 'Giovanni');
console.log('Giovanni\'s legendary:', giovanni.thirdPokemon[0].name);

// Find Leaders
const leaders = lineups.filter(lineup => 
  lineup.title === 'Team GO Rocket Leader'
);

// Get all Fire-type grunts
const fireGrunts = lineups.filter(lineup => lineup.type === 'fire');

// Get all Pokémon that can be caught from Rocket encounters
const catchablePokemon = lineups.flatMap(lineup =>
  lineup.firstPokemon
    .filter(p => p.isEncounter)
    .map(p => p.name)
);

const uniqueCatchable = [...new Set(catchablePokemon)];

// Find lineups where you can potentially get shinies
const shinyLineups = lineups.filter(lineup =>
  lineup.firstPokemon.some(p => p.isEncounter && p.canBeShiny)
);

// Find which Rocket members use a specific Pokémon
function findRocketMembersByPokemon(pokemonName) {
  return lineups.filter(lineup => {
    const allPokemon = [
      ...lineup.firstPokemon,
      ...lineup.secondPokemon,
      ...lineup.thirdPokemon
    ];
    return allPokemon.some(p => p.name === pokemonName);
  });
}
```

---

## Error Handling

Always implement proper error handling when fetching data:

```javascript
async function fetchWithErrorHandling(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch data:', error);
    // Implement fallback logic or retry mechanism
    return null;
  }
}
```

---

## Important Notes

⚠️ **Data Volatility**: All data files are updated automatically every four hours to reflect the current Pokémon GO game state. Do not cache data indefinitely or rely on specific Pokémon availability remaining constant.

🕐 **Timezone Considerations**: Events may use different timezone specifications:
- `"Local Time"` - Times are in the player's local timezone
- `null` - Times are in UTC/GMT
- Specific timezone strings - Times are in the specified timezone

🔄 **Update Frequency**: 
- Events data updates multiple times per day
- Egg pools change with events and seasons
- Research tasks rotate regularly
- Rocket lineups change periodically

📊 **Data Format**: All files use minified JSON format (`.min.json`) for efficient delivery. The data is complete despite being minified.

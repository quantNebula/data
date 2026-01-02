# Pokémon GO Team Rocket Lineups Data Schema

## Overview

The `rocketLineups.min.json` file contains data about Team GO Rocket encounters in Pokémon GO. This dataset describes the Pokémon lineups used by Giovanni, the Rocket Leaders (Cliff, Arlo, Sierra), and various Rocket Grunts, along with which Pokémon can be caught after defeating them.

**Note:** Team Rocket lineups change periodically with events and monthly rotations. This documentation focuses on the data structure rather than specific current lineups.

## Data Access

The data is available via CDN:

```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json
```

### Usage Example

```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json')
  .then(response => response.json())
  .then(lineups => {
    console.log(`Loaded ${lineups.length} Team Rocket lineups`);
    const giovanni = lineups.find(l => l.name === 'Giovanni');
  });
```

## File Structure

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

## JSON Schema

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
        "items": {
          "$ref": "#/definitions/pokemon"
        }
      },
      "secondPokemon": {
        "type": "array",
        "description": "Possible second slot Pokémon (one is randomly selected)",
        "items": {
          "$ref": "#/definitions/pokemon"
        }
      },
      "thirdPokemon": {
        "type": "array",
        "description": "Possible third slot Pokémon (one is randomly selected)",
        "items": {
          "$ref": "#/definitions/pokemon"
        }
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
          "items": {
            "type": "string"
          }
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

## Property Definitions

### Lineup Properties

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

### Pokémon Properties

| Property | Type | Description |
|----------|------|-------------|
| `name` | string | Pokémon name (includes form if applicable) |
| `image` | string (URI) | URL to Pokémon icon |
| `imageWidth` | integer | Icon width in pixels |
| `imageHeight` | integer | Icon height in pixels |
| `types` | array | Pokémon types (e.g., ["fire", "flying"]) |
| `isEncounter` | boolean | If `true`, this Pokémon can be caught after defeating the Rocket member |
| `canBeShiny` | boolean | If `true`, the encounter can be shiny |

## Understanding Lineups

### Battle Structure

Each Rocket member has three Pokémon slots. For each slot:
- One Pokémon is randomly selected from the possible options
- The first slot Pokémon determines potential catch encounters
- Only Pokémon with `isEncounter: true` can be caught

### Rocket Member Types

**Giovanni (Boss)**
- The leader of Team GO Rocket
- Always has Persian as first Pokémon
- Third Pokémon is usually a Legendary Shadow Pokémon
- Requires Super Rocket Radar to find

**Leaders (Cliff, Arlo, Sierra)**
- Named characters with consistent identities
- Require Rocket Radar to find
- Have specific signature Pokémon
- First Pokémon from these battles can be caught

**Grunts**
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

## Usage Examples

### Finding Giovanni's Lineup

```javascript
const giovanni = lineups.find(lineup => lineup.name === 'Giovanni');
console.log('Giovanni\'s legendary:', giovanni.thirdPokemon[0].name);
```

### Finding Leaders

```javascript
const leaders = lineups.filter(lineup => 
  lineup.title === 'Team GO Rocket Leader'
);

leaders.forEach(leader => {
  console.log(`${leader.name}:`, leader.firstPokemon[0].name);
});
```

### Finding Grunts by Type

```javascript
// Get all Fire-type grunts
const fireGrunts = lineups.filter(lineup =>
  lineup.type === 'fire'
);

// Get all typed grunts
const typedGrunts = lineups.filter(lineup =>
  lineup.type && lineup.type !== ''
);
```

### Finding Catchable Pokémon

```javascript
// Get all Pokémon that can be caught from Rocket encounters
const catchablePokemon = lineups.flatMap(lineup =>
  lineup.firstPokemon
    .filter(p => p.isEncounter)
    .map(p => p.name)
);

const uniqueCatchable = [...new Set(catchablePokemon)];
```

### Finding Shiny-Eligible Encounters

```javascript
// Find lineups where you can potentially get shinies
const shinyLineups = lineups.filter(lineup =>
  lineup.firstPokemon.some(p => p.isEncounter && p.canBeShiny)
);

shinyLineups.forEach(lineup => {
  const shinies = lineup.firstPokemon.filter(p => 
    p.isEncounter && p.canBeShiny
  );
  console.log(`${lineup.name}: ${shinies.map(p => p.name).join(', ')}`);
});
```

### Filtering by Gender

```javascript
// Get all female grunts
const femaleGrunts = lineups.filter(lineup =>
  lineup.gender === 'Female' && lineup.title === 'Team GO Rocket Grunt'
);
```

### Building a Counter Guide

```javascript
// Group grunts by their type specialization
const gruntsByType = lineups
  .filter(l => l.type && l.title === 'Team GO Rocket Grunt')
  .reduce((acc, grunt) => {
    if (!acc[grunt.type]) acc[grunt.type] = [];
    acc[grunt.type].push(grunt);
    return acc;
  }, {});

// Get all possible Pokémon types for a specific grunt
function getGruntTypes(gruntName) {
  const grunt = lineups.find(l => l.name === gruntName);
  const allPokemon = [
    ...grunt.firstPokemon,
    ...grunt.secondPokemon,
    ...grunt.thirdPokemon
  ];
  const allTypes = allPokemon.flatMap(p => p.types);
  return [...new Set(allTypes)];
}
```

### Finding Specific Shadow Pokémon

```javascript
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

const machopRockets = findRocketMembersByPokemon('Machop');
```

## Special Cases

### Multiple Possible Pokémon

Each slot typically has 1-3 possible Pokémon. One is randomly chosen when the battle starts. This creates variety and requires players to prepare flexible battle teams.

```javascript
// Cliff's second slot possibilities
{
  "secondPokemon": [
    {"name": "Annihilape", ...},
    {"name": "Honchkrow", ...},
    {"name": "Skarmory", ...}
  ]
}
```

### Non-Catchable Pokémon

Pokémon in second and third slots always have `isEncounter: false`. Some first-slot Pokémon are also not catchable (e.g., Giovanni's Persian).

### Special Grunts

Some grunts have unique mechanics:
- **"Male Grunt"** (Kanto starters) - Uses only starter Pokémon
- **"Female Grunt"** (Snorlax) - Features Snorlax prominently
- **"Decoy Female Grunt"** - Special lineup for particular events
- **Water Male Grunt** - Can have up to 3 Magikarp

### Type Specializations

Typed grunts have quotes related to their type:
- Fire: "Fire burns... catch!"
- Water: "Water flows... splash!"
- Grass: "Grass grows... go!"

The `type` field helps categorize these for strategy guides.

## Important Notes

- **Data volatility**: Rocket lineups change monthly and during special events. Giovanni's legendary changes with each Special Research.
- **Random selection**: Each battle slot randomly selects one Pokémon from its array at battle start.
- **Catch opportunity**: Only first-slot Pokémon with `isEncounter: true` can be caught.
- **Shadow Pokémon**: All caught Pokémon from Rocket battles are Shadow Pokémon and can be purified.
- **Shiny rates**: Shiny chance for Shadow Pokémon is roughly 1/64 when `canBeShiny` is true.
- **CP scaling**: Rocket Pokémon CP scales with trainer level.

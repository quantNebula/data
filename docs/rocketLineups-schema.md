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
        "description": "Pokémon type specialization for typed grunts (empty string for non-typed)"
      },
      "typeImage": {
        "type": "string",
        "description": "URL to type symbol image (empty string for non-typed encounters)"
      },
      "typeImageWidth": {
        "type": ["integer", "null"],
        "minimum": 1,
        "description": "Type badge width in pixels. null for non-typed encounters"
      },
      "typeImageHeight": {
        "type": ["integer", "null"],
        "minimum": 1,
        "description": "Type badge height in pixels. null for non-typed encounters"
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
          "description": "Name of the Pokémon (may include form or regional variant)"
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
          "description": "Whether this Pokémon can be caught if it appears in the battle lineup"
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
| `name` | string | Yes | Identifier name for the Rocket member. Format varies: "Giovanni", "Cliff", "Fire-type Female Grunt", "Male Grunt" |
| `title` | string | Yes | Role: "Team GO Rocket Boss", "Team GO Rocket Leader", or "Team GO Rocket Grunt" |
| `type` | string | No | Type specialization (e.g., "fire", "water", "grass"). Empty string for non-typed encounters (Giovanni, Leaders, special grunts) |
| `typeImage` | string | No | URL to type badge icon. Empty string for non-typed encounters |
| `typeImageWidth` | integer/null | No | Type badge width in pixels. `null` for non-typed encounters (Giovanni, Leaders, special grunts) |
| `typeImageHeight` | integer/null | No | Type badge height in pixels. `null` for non-typed encounters |
| `gender` | string | Yes | "Male" or "Female". Important for differentiating grunt variants |
| `firstPokemon` | array | Yes | Possible Pokémon in first battle slot (one randomly selected per battle) |
| `secondPokemon` | array | Yes | Possible Pokémon in second battle slot (one randomly selected per battle) |
| `thirdPokemon` | array | Yes | Possible Pokémon in third battle slot (one randomly selected per battle) |
### Pokémon Properties
| Property | Type | Description |
|----------|------|-------------|
| `name` | string | Pokémon name. May include regional forms (Alolan, Galarian), form variants (Incarnate), or gender specifications |
| `image` | string (URI) | URL to Pokémon icon |
| `imageWidth` | integer | Icon width in pixels |
| `imageHeight` | integer | Icon height in pixels |
| `types` | array | Pokémon types (e.g., ["fire", "flying"], ["dragon"]) |
| `isEncounter` | boolean | If `true`, this Pokémon can be caught after winning. The catchable Pokémon depends on which slot it occupies during battle |
| `canBeShiny` | boolean | If `true`, the encounter can potentially be shiny. `false` means shiny form not released for Shadow versions or temporarily unavailable |
## Understanding Lineups
### Battle Structure
Each Rocket member has three Pokémon slots. The battle mechanics work as follows:
1. **Random Selection**: For each slot, one Pokémon is randomly selected from the possible options
2. **Encounter Pool**: Pokémon with `isEncounter: true` form the potential catch pool
3. **Catch Mechanics**: After defeating a Rocket member, ONE Pokémon from those with `isEncounter: true` that appeared in the battle can be caught
**Important**: `isEncounter: true` can appear on Pokémon in ANY slot (first, second, or third), not just the first slot. The encounter system determines which catchable Pokémon appears based on the battle lineup.
### Rocket Member Types
**Giovanni (Boss)**
- The leader of Team GO Rocket
- Always has Persian (not catchable) as first Pokémon
- Second slot has strong Normal, Rock, or Ground-type Pokémon
- Third Pokémon is a Legendary Shadow Pokémon (catchable)
- Legendary may include form notation (e.g., "Tornadus (Incarnate)")
- Requires Super Rocket Radar to find
- Legendary changes with Special Research rotations
**Leaders (Cliff, Arlo, Sierra)**
- Named characters with consistent identities
- Each has a signature first Pokémon that can be caught
- Require Rocket Radar to find
- Have specific personality-themed lineups
- Currently:
  - Cliff: Larvitar (Rock/Ground specialist)
  - Arlo: Wobbuffet (Psychic/Steel specialist)
  - Sierra: Feebas (versatile lineup)
**Grunts**
- Generic Team GO Rocket members
- Most are specialized by type (Fire, Water, Grass, etc.)
- Gender variants of the same type have COMPLETELY DIFFERENT lineups
- Can be found at invaded PokéStops
- Most common encounter type
### Gender-Specific Lineups
**Critical**: The same type specialization can have entirely different lineups based on gender:
- **Water-type Female Grunt**: Oshawott, Spheal, Staryu line
- **Water-type Male Grunt**: The infamous "all Magikarp" lineup
When filtering by type, always check gender to understand which lineup you'll face.
### Regional Forms and Variants
Pokémon names may include regional designations and form specifications:
- **Regional Forms**: "Alolan Meowth", "Galarian Slowpoke", "Alolan Ninetales"
- **Form Variants**: "Tornadus (Incarnate)", "Darmanitan (Standard)"
- **Evolution Forms**: Standard naming (no special notation)
These regional and form variants are distinct Pokémon with different types and stats.
### Encounter Mechanics
Understanding encounter availability:
1. **isEncounter Flag**: Indicates which Pokémon CAN be caught if they appear in battle
2. **Multi-Slot Encounters**: Many grunts have `isEncounter: true` on Pokémon in different slots
3. **Battle Dependency**: The actual catchable Pokémon depends on which specific Pokémon appear when you battle
**Examples of Multi-Slot Encounters**:
- Fighting-type Female Grunt: First slot (Machop, Makuhita, Mankey) AND second slot (Hitmonlee, Hitmonchan, Gurdurr) all have `isEncounter: true`
- Bug-type Male Grunt: First slot (Pineco, Grubbin, Scyther) AND second slot (Karrablast, Shelmet, Anorith) have `isEncounter: true`
- Water-type Male Grunt: Both first AND second slot Magikarp have `isEncounter: true`
After defeating the grunt, the game determines which encountered Pokémon becomes available for capture.
### Shiny Availability
The `canBeShiny` field indicates shiny potential for Shadow Pokémon:
- `true`: This Shadow Pokémon can be shiny (approximately 1/64 odds per encounter)
- `false`: Shiny form not available for Shadow version, or temporarily disabled

**Shadow Shiny Mechanics**:

**Encounter Odds**: Shadow Pokémon that can be shiny have approximately **1/64 odds** (~1.56% chance), which is significantly better than wild encounter rates (~1/500) but lower than Community Day rates (~1/25).

**Guaranteed Catch**: Once you defeat a Rocket member, the encounter Pokémon cannot flee, allowing unlimited catch attempts. This is particularly valuable for shiny Shadow Pokémon.

**Availability Limitations**:
- Not all Shadow Pokémon have their shiny forms released
- Even if the regular form has a shiny variant, the Shadow version may not
- Shiny availability can be temporarily disabled and later re-enabled
- First-slot Pokémon may have `canBeShiny: false` even though they're the catchable encounter

**Shadow vs. Regular Shinies**:
- Shadow and regular forms are separate shiny checks (separate Pokédex entries)
- A Shadow shiny remains Shadow even after purification (becomes Purified shiny)
- Purifying a shiny Shadow gives it a unique "Purified" tag but removes the Shadow bonus
- Shadow shinies can be traded (both Pokémon must be registered in both players' Pokédex)

**Strategic Considerations**:
- Shadow shinies are highly valued by collectors
- Purification is permanent - many collectors keep Shadow shinies unpurified
- The Shadow bonus (+20% attack, -20% defense) makes them valuable for raids

**Cross-Reference**: For complete shiny information including release dates, reference `shiny.min.json`. Note that Shadow variants are tracked separately from their regular counterparts.
## Usage Examples
### Finding Giovanni's Lineup
```javascript
const giovanni = lineups.find(lineup => lineup.name === 'Giovanni');
console.log('Giovanni\'s legendary:', giovanni.thirdPokemon[0].name);
console.log('Can catch it?', giovanni.thirdPokemon[0].isEncounter);
console.log('Can be shiny?', giovanni.thirdPokemon[0].canBeShiny);
```
### Finding Leaders
```javascript
const leaders = lineups.filter(lineup => 
  lineup.title === 'Team GO Rocket Leader'
);
leaders.forEach(leader => {
  const catchable = leader.firstPokemon.find(p => p.isEncounter);
  console.log(`${leader.name}: ${catchable.name} (Shiny: ${catchable.canBeShiny})`);
});
```
### Finding Grunts by Type and Gender
```javascript
// Get all Fire-type grunts
const fireGrunts = lineups.filter(lineup =>
  lineup.type === 'fire'
);
// Get Water-type Male grunt specifically
const waterMaleGrunt = lineups.find(lineup =>
  lineup.type === 'water' && lineup.gender === 'Male'
);
// Get all typed grunts grouped by type and gender
const typedGrunts = lineups
  .filter(lineup => lineup.type && lineup.type !== '')
  .reduce((acc, grunt) => {
    const key = `${grunt.type}-${grunt.gender}`;
    acc[key] = grunt;
    return acc;
  }, {});
```
### Finding ALL Catchable Pokémon
```javascript
// Get all Pokémon that can be caught from Rocket encounters (any slot)
const catchablePokemon = lineups.flatMap(lineup => {
  const allSlots = [
    ...lineup.firstPokemon,
    ...lineup.secondPokemon,
    ...lineup.thirdPokemon
  ];
  return allSlots
    .filter(p => p.isEncounter)
    .map(p => ({
      pokemon: p.name,
      source: lineup.name,
      shiny: p.canBeShiny
    }));
});
const uniqueCatchable = [...new Set(catchablePokemon.map(p => p.pokemon))];
console.log(`${uniqueCatchable.length} unique Shadow Pokémon available`);
```
### Finding Shiny-Eligible Encounters
```javascript
// Find all lineups where you can potentially get shinies
const shinyLineups = lineups.filter(lineup => {
  const allPokemon = [
    ...lineup.firstPokemon,
    ...lineup.secondPokemon,
    ...lineup.thirdPokemon
  ];
  return allPokemon.some(p => p.isEncounter && p.canBeShiny);
});
shinyLineups.forEach(lineup => {
  const allPokemon = [
    ...lineup.firstPokemon,
    ...lineup.secondPokemon,
    ...lineup.thirdPokemon
  ];
  const shinies = allPokemon.filter(p => p.isEncounter && p.canBeShiny);
  console.log(`${lineup.name}: ${shinies.map(p => p.name).join(', ')}`);
});
```
### Finding Encounters by Slot
```javascript
// Analyze which slots have catchable Pokémon
function analyzeEncounterSlots(lineup) {
  const slots = {
    first: lineup.firstPokemon.filter(p => p.isEncounter).map(p => p.name),
    second: lineup.secondPokemon.filter(p => p.isEncounter).map(p => p.name),
    third: lineup.thirdPokemon.filter(p => p.isEncounter).map(p => p.name)
  };
  return slots;
}
const waterMale = lineups.find(l => l.name === 'Water-type Male Grunt');
console.log(analyzeEncounterSlots(waterMale));
// Shows Magikarp in first AND second slots
```
### Building a Counter Guide
```javascript
// Group grunts by their type specialization
const gruntsByType = lineups
  .filter(l => l.type && l.title === 'Team GO Rocket Grunt')
  .reduce((acc, grunt) => {
    const key = grunt.type;
    if (!acc[key]) acc[key] = [];
    acc[key].push({
      name: grunt.name,
      gender: grunt.gender
    });
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
console.log('Fire grunt types:', getGruntTypes('Fire-type Female Grunt'));
```
### Finding Specific Shadow Pokémon
```javascript
// Find which Rocket members can have a specific Pokémon
function findRocketMembersByPokemon(pokemonName) {
  return lineups.filter(lineup => {
    const allPokemon = [
      ...lineup.firstPokemon,
      ...lineup.secondPokemon,
      ...lineup.thirdPokemon
    ];
    return allPokemon.some(p => p.name === pokemonName);
  }).map(l => ({
    name: l.name,
    catchable: [...l.firstPokemon, ...l.secondPokemon, ...l.thirdPokemon]
      .find(p => p.name === pokemonName)?.isEncounter || false
  }));
}
const machopSources = findRocketMembersByPokemon('Machop');
console.log('Machop sources:', machopSources);
```
### Finding Regional Form Encounters
```javascript
// Find all Alolan form Shadow Pokémon
const alolanEncounters = lineups.flatMap(lineup => {
  const allPokemon = [
    ...lineup.firstPokemon,
    ...lineup.secondPokemon,
    ...lineup.thirdPokemon
  ];
  return allPokemon
    .filter(p => p.name.includes('Alolan') && p.isEncounter)
    .map(p => ({
      pokemon: p.name,
      source: lineup.name,
      shiny: p.canBeShiny
    }));
});
console.log('Alolan Shadow Pokémon:', 
  [...new Set(alolanEncounters.map(e => e.pokemon))]);
```
## Special Cases
### Unique Grunt Lineups
Some grunts have distinctive mechanics:
**Water-type Male Grunt (The Magikarp Grunt)**
- All three slots contain Magikarp (or Gyarados in third slot)
- **First AND second slot** Magikarp have `isEncounter: true`
- Third slot is either Magikarp (not catchable) or Gyarados (not catchable)
- Most unique lineup in the game
**Male Grunt (Kanto Starters)**
- Uses only Kanto starter evolution lines
- All three starters in first slot (Bulbasaur, Charmander, Squirtle)
- All three have `isEncounter: true` and `canBeShiny: true`
- Second and third slots show evolution stages
**Female Grunt (Snorlax)**
- Features Snorlax prominently (first slot, catchable)
- Snorlax also appears in second and third slots as possible options
- Mixed with other strong Pokémon
**Decoy Female Grunt**
- Special event-specific lineup
- Features Bellsprout as catchable first Pokémon
- Limited availability during certain events
### Multi-Slot Catchable Pokémon
Many grunts have encounter opportunities across multiple slots:
- **Fighting-type Female Grunt**: 6 catchable options across slots 1 and 2
- **Poison-type Female Grunt**: 6 catchable options across slots 1 and 2
- **Ground-type Male Grunt**: 6 catchable options across slots 1 and 2
- **Bug-type Male Grunt**: 6 catchable options across slots 1 and 2
- **Ghost-type Male Grunt**: 6 catchable options across slots 1 and 2
- **Fairy-type Female Grunt**: 6 catchable options across slots 1 and 2
This design gives players a larger pool of possible Shadow Pokémon encounters from a single grunt type.
### Type Specialization Quotes
Typed grunts announce themselves with type-themed dialogue:
- **Fire**: "Do you know how hot Pokémon fire breath can get?"
- **Water**: "These waters are treacherous!"
- **Grass**: "Don't tangle with us!"
- **Electric**: "Get ready to be shocked!"
- **Ice**: "You're gonna be frozen in your tracks!"
- **Fighting**: "This buff physique isn't just for show!"
- **Poison**: "Coiled and ready to strike!"
- **Ground**: "You'll be defeated into the ground!"
- **Flying**: "My Bird Pokémon want to battle with you!"
- **Psychic**: "Are you scared of psychics that use unseen power?"
- **Bug**: "Go, my super bug Pokémon!"
- **Rock**: "Let's rock and roll!"
- **Ghost**: "Ke...ke...ke...ke...ke...ke!"
- **Dragon**: "ROAR! ...How'd that sound?"
- **Dark**: "Wherever there is light, there is also shadow."
- **Steel**: "You're no match for my iron will!"
- **Fairy**: "Check out my cute Pokémon!"
The `type` field corresponds to these specializations and helps build encounter guides.
## Important Notes
- **Data volatility**: Rocket lineups change monthly and during special events. Giovanni's legendary changes with each Special Research rotation
- **Random selection**: Each battle slot randomly selects one Pokémon from its array when battle starts
- **Catch opportunity**: After winning, ONE Pokémon with `isEncounter: true` that appeared in battle becomes available to catch
- **Multi-slot encounters**: Many grunts have catchable Pokémon in multiple slots, expanding the encounter pool
- **Shadow Pokémon**: All caught Pokémon from Rocket battles are Shadow Pokémon and can be purified
- **Shiny rates**: Shiny chance for Shadow Pokémon with `canBeShiny: true` is approximately 1/64
- **CP scaling**: Rocket Pokémon CP scales with trainer level
- **Gender matters**: Same type + different gender = completely different lineup
- **Regional forms**: Alolan, Galarian, and other regional forms are treated as distinct Pokémon
- **Form notation**: Legendary Pokémon may include form specifications like "(Incarnate)"
## Data Quality Considerations
- **Empty strings**: Non-typed encounters (Giovanni, Leaders, special grunts) have `type: ""` and `typeImage: ""`
- **Null dimensions**: Type image dimensions are `null` for non-typed encounters
- **Gender significance**: Always check gender when filtering by type, as it determines the actual lineup
- **Encounter distribution**: `isEncounter: true` appears strategically across multiple slots for many grunts
- **Form completeness**: Regional form names include full designation (e.g., "Alolan", not abbreviated)$$$$
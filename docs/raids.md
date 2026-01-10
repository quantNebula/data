# Pokémon GO Raid Data Schema
## Overview
The `raids.min.json` file contains comprehensive data about current Raid Battles in Pokémon GO. This dataset describes which Pokémon appear in raids across different tiers (1-Star, 3-Star, 5-Star, and Mega Raids), their combat power ranges, type advantages, and shiny availability.

**Note:** Raid bosses rotate frequently with events and game updates. This documentation focuses on the data structure rather than specific current raid lineups.

## Data Access
The data is available via CDN:
```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json
```

### Usage Example
```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json')
  .then(response => response.json())
  .then(raids => {
    console.log(`Loaded ${raids.length} raid bosses`);
    
    // Filter by tier
    const fiveStarRaids = raids.filter(r => r.tier === '5-Star Raids');
    console.log(`${fiveStarRaids.length} legendary raids available`);
  });
```

## File Structure
The file contains a JSON array of raid boss objects, where each object represents a Pokémon available in raids.

```json
[
  {
    "name": "Blacephalon",
    "tier": "5-Star Raids",
    "canBeShiny": true,
    "types": [
      {
        "name": "fire",
        "image": "https://leekduck.com/assets/img/types/fire.png",
        "imageWidth": 32,
        "imageHeight": 32
      }
    ],
    "combatPower": {
      "normal": {
        "min": 1797,
        "max": 1884
      },
      "boosted": {
        "min": 2246,
        "max": 2355
      }
    },
    "boostedWeather": [
      {
        "name": "sunny",
        "image": "https://leekduck.com/assets/img/weather/sunny.png",
        "imageWidth": 32,
        "imageHeight": 32
      }
    ],
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm806.icon.png",
    "imageWidth": 256,
    "imageHeight": 256
  }
]
```

## JSON Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Raid Data Schema",
  "description": "Schema for Pokémon GO raid boss data",
  "type": "array",
  "items": {
    "type": "object",
    "required": [
      "name",
      "tier",
      "canBeShiny",
      "types",
      "combatPower",
      "boostedWeather",
      "image",
      "imageWidth",
      "imageHeight"
    ],
    "properties": {
      "name": {
        "type": "string",
        "description": "The name of the raid boss Pokémon. May include form variations, costume variants, or Shadow designation"
      },
      "tier": {
        "type": "string",
        "description": "The raid difficulty tier",
        "enum": ["1-Star Raids", "3-Star Raids", "5-Star Raids", "Mega Raids"]
      },
      "canBeShiny": {
        "type": "boolean",
        "description": "Whether this Pokémon can be encountered in its shiny form when caught after the raid"
      },
      "types": {
        "type": "array",
        "description": "The Pokémon type(s) - affects damage multipliers and counters",
        "minItems": 1,
        "maxItems": 2,
        "items": {
          "type": "object",
          "required": ["name", "image", "imageWidth", "imageHeight"],
          "properties": {
            "name": {
              "type": "string",
              "description": "Type name in lowercase",
              "enum": [
                "normal", "fire", "water", "electric", "grass", "ice",
                "fighting", "poison", "ground", "flying", "psychic", 
                "bug", "rock", "ghost", "dragon", "dark", "steel", "fairy"
              ]
            },
            "image": {
              "type": "string",
              "format": "uri",
              "description": "URL to the type icon image"
            },
            "imageWidth": {
              "type": "integer",
              "description": "Width of the type icon in pixels",
              "minimum": 1
            },
            "imageHeight": {
              "type": "integer",
              "description": "Height of the type icon in pixels",
              "minimum": 1
            }
          }
        }
      },
      "combatPower": {
        "type": "object",
        "description": "Combat Power (CP) ranges for caught raid boss",
        "required": ["normal", "boosted"],
        "properties": {
          "normal": {
            "type": "object",
            "description": "CP range under normal conditions",
            "required": ["min", "max"],
            "properties": {
              "min": {
                "type": "integer",
                "description": "Minimum CP at level 20 (for 5-Star/Mega) or level 15 (for 1-Star/3-Star)",
                "minimum": 1
              },
              "max": {
                "type": "integer",
                "description": "Maximum CP at level 20 (for 5-Star/Mega) or level 15 (for 1-Star/3-Star)",
                "minimum": 1
              }
            }
          },
          "boosted": {
            "type": "object",
            "description": "CP range when weather-boosted",
            "required": ["min", "max"],
            "properties": {
              "min": {
                "type": "integer",
                "description": "Minimum CP at level 25 (for 5-Star/Mega) or level 20 (for 1-Star/3-Star) with weather boost",
                "minimum": 1
              },
              "max": {
                "type": "integer",
                "description": "Maximum CP at level 25 (for 5-Star/Mega) or level 20 (for 1-Star/3-Star) with weather boost",
                "minimum": 1
              }
            }
          }
        }
      },
      "boostedWeather": {
        "type": "array",
        "description": "Weather conditions that provide a level boost to the caught Pokémon",
        "minItems": 1,
        "items": {
          "type": "object",
          "required": ["name", "image", "imageWidth", "imageHeight"],
          "properties": {
            "name": {
              "type": "string",
              "description": "Weather condition name",
              "enum": [
                "sunny", "rainy", "partly cloudy", "cloudy", 
                "windy", "snow", "fog"
              ]
            },
            "image": {
              "type": "string",
              "format": "uri",
              "description": "URL to the weather icon image"
            },
            "imageWidth": {
              "type": "integer",
              "description": "Width of the weather icon in pixels",
              "minimum": 1
            },
            "imageHeight": {
              "type": "integer",
              "description": "Height of the weather icon in pixels",
              "minimum": 1
            }
          }
        }
      },
      "image": {
        "type": "string",
        "format": "uri",
        "description": "URL to the Pokémon's icon image"
      },
      "imageWidth": {
        "type": "integer",
        "description": "Width of the Pokémon icon in pixels",
        "minimum": 1
      },
      "imageHeight": {
        "type": "integer",
        "description": "Height of the Pokémon icon in pixels",
        "minimum": 1
      }
    }
  }
}
```

## Property Details

### Raid Tiers
Raids in Pokémon GO are categorized into four difficulty tiers:
- **1-Star Raids** - Soloable, easier Pokémon
- **3-Star Raids** - Moderate difficulty, typically require 2-3 players
- **5-Star Raids** - Legendary Pokémon, require groups of players
- **Mega Raids** - Mega-evolved Pokémon, similar difficulty to 5-Star raids

### Combat Power
The CP values indicate what CP range players can expect when catching the raid boss:
- **Normal**: Base level CP (Level 20 for Legendary/Mega, Level 15 for others)
- **Boosted**: Weather-boosted CP (Level 25 for Legendary/Mega, Level 20 for others)

### Weather Boost
When the in-game weather matches a type the Pokémon has, it receives a weather boost:
- The caught Pokémon will be 5 levels higher
- The CP range shifts to the "boosted" values
- The Pokémon will have higher IVs (minimum 4/4/4 instead of 0/0/0)

## Common Use Cases

### Filter by Tier
```javascript
const legendaryRaids = raids.filter(r => r.tier === '5-Star Raids');
```

### Find Shiny-Eligible Bosses
```javascript
const shinyAvailable = raids.filter(r => r.canBeShiny);
```

### Find Raids Boosted in Specific Weather
```javascript
const rainyWeatherRaids = raids.filter(r => 
  r.boostedWeather.some(w => w.name === 'rainy')
);
```

### Group by Tier
```javascript
const raidsByTier = raids.reduce((acc, raid) => {
  if (!acc[raid.tier]) acc[raid.tier] = [];
  acc[raid.tier].push(raid);
  return acc;
}, {});
```

## Notes
- Raid bosses rotate regularly, often aligned with events
- Shadow raid bosses are indicated with "Shadow" prefix in the name
- Costume variants (e.g., "Party Hat Pikachu") are specified in the name
- Mega Evolution forms (e.g., "Mega Swampert") appear in Mega Raids
- The `types` array determines raid boss weaknesses and resistances for battle strategy

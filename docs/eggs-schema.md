# Pokémon GO Egg Data Schema

## Overview

The `eggs.min.json` file contains comprehensive data about the Pokémon egg pool in Pokémon GO. This dataset describes which Pokémon can hatch from different egg types, their rarity, shiny availability, and other relevant properties.

**Note:** The egg pool changes frequently with events and game updates. This documentation focuses on the data structure rather than specific current contents.

## Data Access

The data is available via CDN:

```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json
```

### Usage Example

```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json')
  .then(response => response.json())
  .then(eggs => {
    console.log(`Loaded ${eggs.length} egg entries`);
  });
```

## File Structure

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

## JSON Schema

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

## Property Definitions

### Core Properties

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

When `isAdventureSync` is `true`, the Pokémon **only** hatches from eggs earned through Adventure Sync weekly rewards (walking 25km or 50km in a week), not from regular PokéStop eggs. These eggs use the same distance categories (5 km, 10 km) but have a different pool of Pokémon.

### Combat Power

The `combatPower` object contains `min` and `max` values:

```json
"combatPower": {
  "min": 637,
  "max": 637
}
```

**Note:** For hatched Pokémon, `min` and `max` are typically identical, representing the fixed CP at which the Pokémon hatches. This differs from wild encounters which have variable CP.

### Rarity System

Each Pokémon has both a numeric `rarity` (1-5) and a human-readable `rarityTier`:

| rarity | rarityTier | Description |
|--------|-----------|-------------|
| 1 | Common | Frequently encountered |
| 2 | Uncommon | Moderately rare |
| 3 | Rare | Less common, desirable |
| 4 | Very Rare | Significantly rare, highly sought |
| 5 | Ultra Rare | Extremely rare, special events |

### Regional and Gift Exchange Flags

- **`isRegional`**: Indicates the Pokémon is normally restricted to specific geographic regions (e.g., Kangaskhan in Australia). During events, these may become globally available through eggs.

- **`isGiftExchange`**: Specifically for 7 km eggs, this flag is `true` when the Pokémon can **only** be obtained from eggs received through opening gifts from friends.

## Special Cases

### Pokémon in Multiple Egg Types

Some Pokémon may appear multiple times in the dataset with different configurations:
- Same Pokémon in both regular and Adventure Sync eggs
- Same Pokémon across multiple egg distance types
- Regional variants appearing in both regular 7 km eggs and gift exchange 7 km eggs

When querying the data, be aware that filtering by Pokémon name alone may return multiple entries with different properties.

### Form Variations

The `name` field includes form variations and regional variants:

**Regional Forms:**
- Alolan forms: "Alolan Geodude", "Alolan Diglett"
- Galarian forms: "Galarian Meowth", "Galarian Zigzagoon"
- Hisuian forms: "Hisuian Sneasel", "Hisuian Growlithe"

**Special Forms:**
- Gender variants: "Indeedee (Male)", "Indeedee (Female)"
- Color variants: "Basculin (White Striped)"

### Image URL Patterns

Images are hosted on a CDN and follow consistent URL patterns:

- **Standard Pokémon:** Include Pokémon ID number
- **Regional forms:** Include form designation (e.g., `.fALOLA`, `.fGALARIAN`, `.fHISUIAN`)
- **Gender forms:** Include gender designation (e.g., `.fMALE`, `.fFEMALE`)
- **Color variants:** Include variant designation (e.g., `.fWHITE_STRIPED`)

## Usage Examples

### Filtering by Egg Type

```javascript
// Get all Pokémon from 10 km eggs
const tenKmPokemon = eggs.filter(p => p.eggType === "10 km");

// Get Adventure Sync exclusives
const adventureSyncPokemon = eggs.filter(p => p.isAdventureSync === true);
```

### Finding Shiny-Eligible Pokémon

```javascript
// Get all Pokémon that can be shiny from 5 km eggs
const shiny5kmPokemon = eggs.filter(p => 
  p.eggType === "5 km" && p.canBeShiny === true
);
```

### Filtering by Rarity

```javascript
// Get Ultra Rare Pokémon
const ultraRare = eggs.filter(p => p.rarityTier === "Ultra Rare");

// Get rare or better from 2 km eggs
const rare2km = eggs.filter(p => 
  p.eggType === "2 km" && p.rarity >= 3
);
```

### Regional Variants

```javascript
// Get all Galarian forms
const galarianForms = eggs.filter(p => p.name.startsWith("Galarian"));

// Get all regional-exclusive Pokémon available through eggs
const regionalPokemon = eggs.filter(p => p.isRegional === true);
```

### Gift Exchange Exclusives

```javascript
// Get Pokémon only from friend gifts
const giftExclusives = eggs.filter(p => 
  p.eggType === "7 km" && p.isGiftExchange === true
);
```

### Grouping by Egg Type

```javascript
// Group all Pokémon by egg type
const byEggType = eggs.reduce((acc, pokemon) => {
  const key = pokemon.eggType;
  if (!acc[key]) acc[key] = [];
  acc[key].push(pokemon);
  return acc;
}, {});

console.log(`2 km eggs contain ${byEggType["2 km"].length} Pokémon`);
```

### Finding Duplicate Appearances

```javascript
// Find Pokémon that appear in multiple egg pools
const pokemonCounts = eggs.reduce((acc, p) => {
  acc[p.name] = (acc[p.name] || 0) + 1;
  return acc;
}, {});

const duplicates = Object.entries(pokemonCounts)
  .filter(([name, count]) => count > 1)
  .map(([name]) => name);
```

## Best Practices

### Data Consumption

1. **Treat as read-only**: This data represents a snapshot of the current egg pool and should not be modified directly.

2. **Handle duplicates**: Some Pokémon appear multiple times with different configurations (egg type, Adventure Sync status). Filter appropriately for your use case.

3. **Cache images**: The icon URLs are stable CDN links, but consider caching them locally for better performance.

4. **Rarity awareness**: Use the numeric `rarity` field for sorting/filtering, and `rarityTier` for display purposes.

5. **Adventure Sync distinction**: Always check `isAdventureSync` when filtering by egg type, as these represent separate egg pools.

### Display Considerations

1. **Form names**: The `name` field includes all necessary form information for display.

2. **CP display**: Since `min` and `max` are typically identical for eggs, display as a single value: "CP: 637" rather than "CP: 637-637".

3. **Image dimensions**: Use the provided `imageWidth` and `imageHeight` for proper aspect ratio rendering.

4. **Shiny indicators**: Use `canBeShiny` to show shiny availability indicators in your UI.

## Important Notes

- **Data volatility**: The egg pool changes frequently with in-game events, seasonal rotations, and updates. Always treat this data as a current snapshot that may become outdated.
- **Regional availability**: Pokémon marked as `isRegional: true` may only be available in eggs during special events.
- **Egg acquisition**:
  - 2 km, 5 km, 10 km, 12 km: Obtained from PokéStops and Gyms
  - 7 km: Exclusively from friend gifts
  - 12 km: Exclusively from defeating Team GO Rocket Leaders
  - Adventure Sync eggs: Weekly rewards for walking distance
- **Combat Power**: Values typically represent level 20 hatches, or level 25 when weather-boosted.

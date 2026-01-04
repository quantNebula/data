# Pokémon GO Shiny Pokémon Data Schema
## Overview
The `shiny.min.json` file contains comprehensive data about Shiny Pokémon available in Pokémon GO. This dataset catalogs which Pokémon have shiny variants released in the game, when they were released, and the different forms/costumes available for each shiny.

**Note:** Shiny Pokémon are continuously added to the game. This documentation focuses on the data structure rather than the complete current shiny list.

## Data Access
The data is available via CDN:
```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/shiny.min.json
```

### Usage Example
```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/shiny.min.json')
  .then(response => response.json())
  .then(shinies => {
    console.log(`Loaded ${shinies.length} shiny Pokémon entries`);
    
    // Find a specific Pokémon
    const bulbasaur = shinies.find(s => s.name === 'Bulbasaur');
    console.log(`Bulbasaur shiny released: ${bulbasaur.releasedDate}`);
  });
```

## File Structure
The file contains a JSON array of shiny Pokémon objects. Each object represents a specific Pokémon or form variant with its shiny release information.

```json
[
  {
    "dexNumber": 1,
    "name": "Bulbasaur",
    "releasedDate": "2018/03/25",
    "family": "Bulbasaur",
    "typeCode": null,
    "forms": [
      {
        "name": "f19",
        "imageUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pm0001_00_pgo_fall2019_shiny.png",
        "width": 256,
        "height": 256
      },
      {
        "name": "11",
        "imageUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_001_00_shiny.png",
        "width": 256,
        "height": 256
      }
    ],
    "imageUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_001_00_shiny.png",
    "width": 256,
    "height": 256
  }
]
```

## JSON Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Shiny Pokémon Data Schema",
  "description": "Schema for Pokémon GO shiny Pokémon catalog",
  "type": "array",
  "items": {
    "type": "object",
    "required": [
      "dexNumber",
      "name",
      "releasedDate",
      "family"
    ],
    "properties": {
      "dexNumber": {
        "type": "integer",
        "description": "National Pokédex number",
        "minimum": 1
      },
      "name": {
        "type": "string",
        "description": "Name of the Pokémon. May include regional variants (e.g., 'Hisuian Venusaur', 'Paldean Charizard') or mega forms",
        "minLength": 1
      },
      "releasedDate": {
        "type": "string",
        "description": "Date when the shiny form was released in Pokémon GO",
        "pattern": "^\\d{4}/\\d{2}/\\d{2}$"
      },
      "family": {
        "type": "string",
        "description": "Evolution family identifier, sometimes includes form suffix (e.g., 'Bulbasaur', 'Bulbasaur_11')"
      },
      "typeCode": {
        "type": ["string", "null"],
        "description": "Form type code for regional or special variants (e.g., '_51' for Hisuian, '_52' for Paldean). null for standard forms"
      },
      "forms": {
        "type": "array",
        "description": "Array of different forms/costumes available for this shiny Pokémon",
        "items": {
          "type": "object",
          "required": ["name", "imageUrl", "width", "height"],
          "properties": {
            "name": {
              "type": "string",
              "description": "Form identifier code (e.g., 'f19' for Fall 2019, '11' for costume variant, 'M' for Mega, 'M-X' for Mega X, 'M-Y' for Mega Y, 'G' for Gigantamax)"
            },
            "imageUrl": {
              "type": "string",
              "format": "uri",
              "description": "URL to the shiny form's icon image"
            },
            "width": {
              "type": "integer",
              "description": "Width of the form icon in pixels",
              "minimum": 1
            },
            "height": {
              "type": "integer",
              "description": "Height of the form icon in pixels",
              "minimum": 1
            }
          }
        }
      },
      "imageUrl": {
        "type": "string",
        "format": "uri",
        "description": "URL to the default/primary shiny form's icon image"
      },
      "width": {
        "type": "integer",
        "description": "Width of the default icon in pixels",
        "minimum": 1
      },
      "height": {
        "type": "integer",
        "description": "Height of the default icon in pixels",
        "minimum": 1
      }
    }
  }
}
```

## Property Details

### Dex Number
The National Pokédex number. Note that regional variants and different forms of the same species share the same dex number.

### Name
- Standard forms: Simple Pokémon name (e.g., "Bulbasaur")
- Regional variants: Prefixed with region (e.g., "Hisuian Venusaur", "Paldean Charizard")
- Mega forms: May be listed as separate entries

### Released Date
Date format is `YYYY/MM/DD`, representing when the shiny variant first became available in Pokémon GO. This helps players and collectors track which shinies have been available the longest.

### Family
Used to group Pokémon in the same evolution line. Sometimes includes a form suffix:
- `"Bulbasaur"` - Base family
- `"Bulbasaur_11"` - Specific costume/form variant family

### Type Code
Distinguishes regional and special variants:
- `null` - Standard form
- `"_51"` - Hisuian form
- `"_52"` - Paldean form
- Other codes may indicate additional special forms

### Forms Array
Contains all available costume and form variants. Common form codes include:
- **Numeric codes** (e.g., "11", "14", "05") - Costume variants (party hats, seasonal costumes, etc.)
- **Seasonal codes** (e.g., "f19") - Fall 2019 and other seasonal variants
- **Mega codes** (e.g., "M", "M-X", "M-Y") - Mega Evolution forms
- **"G"** - Gigantamax form

### Default Image
The `imageUrl`, `width`, and `height` at the root level represent the default or most common shiny form, typically the standard costume-less variant.

## Common Use Cases

### Find Shiny by Name
```javascript
const pikachu = shinies.find(s => s.name === 'Pikachu');
```

### Find Recently Released Shinies
```javascript
const recentShinies = shinies.filter(s => {
  const releaseDate = new Date(s.releasedDate.replace(/\//g, '-'));
  const cutoffDate = new Date('2025-01-01');
  return releaseDate >= cutoffDate;
});
```

### Group by Evolution Family
```javascript
const families = shinies.reduce((acc, shiny) => {
  const familyKey = shiny.family.split('_')[0]; // Remove form suffix
  if (!acc[familyKey]) acc[familyKey] = [];
  acc[familyKey].push(shiny);
  return acc;
}, {});
```

### Find Pokémon with Multiple Forms
```javascript
const multiForm = shinies.filter(s => s.forms && s.forms.length > 1);
```

### Find Regional Variants
```javascript
const regionalVariants = shinies.filter(s => s.typeCode !== null);
```

### Check if Shiny is Available
```javascript
function isShinyAvailable(pokemonName) {
  return shinies.some(s => s.name === pokemonName);
}
```

## Notes
- Not all Pokémon have their shiny forms released in Pokémon GO
- Some Pokémon have multiple costumed variants (e.g., Pikachu with various hats)
- Mega Evolution forms may appear as separate entries or within the `forms` array
- Regional variants (Alolan, Galarian, Hisuian, Paldean) are treated as separate entries
- The release date helps determine availability in Lucky Trades and other mechanics
- Images are hosted on the PokeMiners GitHub repository, a reliable source for Pokémon GO assets

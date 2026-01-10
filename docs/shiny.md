## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/shiny.min.json

## JSON Example

```json
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
        }
    ],
    "imageUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_001_00_shiny.png",
    "width": 256,
    "height": 256
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `dexNumber` | integer | National Pokédex number |
| `name` | string | Pokémon name, may include regional variants (e.g., "Hisuian Venusaur") |
| `releasedDate` | string | Date shiny was released (YYYY/MM/DD format) |
| `family` | string | Evolution family identifier, may include form suffix (e.g., "Bulbasaur_11") |
| `typeCode` | string\|null | Form type code (e.g., "_51" for Hisuian, "_52" for Paldean), null for standard forms |
| `forms` | array | Available costume/form variants |
| `forms[].name` | string | Form identifier (e.g., "f19" for Fall 2019, "11" for costume, "M" for Mega) |
| `forms[].imageUrl` | string | Shiny form icon URL |
| `forms[].width` | integer | Icon width in pixels |
| `forms[].height` | integer | Icon height in pixels |
| `imageUrl` | string | Default shiny form icon URL |
| `width` | integer | Default icon width in pixels |
| `height` | integer | Default icon height in pixels |

## Data Structure

The endpoint returns an array of shiny Pokémon objects representing every species and form variant with released shiny versions in Pokémon GO. This is a complete historical catalog, not limited to currently obtainable shinies.

## Pokédex Organization

**National Dex Numbers** (`dexNumber`):
- Based on main series Pokémon games
- Shared across all forms of same species
- Regional variants have same dex number (e.g., all Vulpix forms are #37)
- Useful for sorting and categorization

**Form Variants**:
- Single species may have multiple entries
- Each regional variant is separate entry (Kantonian, Alolan, Galarian, Hisuian, Paldean)
- Mega Evolutions tracked separately
- Costume forms tracked within `forms` array of base entry

## Form Type Codes

`typeCode` distinguishes regional/special variants:
- `null`: Standard/Kantonian form
- `"_51"`: Hisuian forms (Hisui region code)
- `"_52"`: Paldean forms (Paldea region code)  
- `"_61"`: Alolan forms (Alola region code)
- `"_31"`: Galarian forms (Galar region code)

**Examples**:
- Regular Venusaur: `typeCode: null`
- Hisuian Venusaur: `typeCode: "_51"`
- Alolan Vulpix: `typeCode: "_61"`

These codes match asset URL patterns in PokeMiners repository.

## Family Structure

`family` field groups evolution lines:
- Base form name ("Bulbasaur", "Charmander", etc.)
- May include suffix indicating costume/form family ("Bulbasaur_11")
- Evolution lines share same family identifier
- Used to track:
  - Which Pokémon evolve together
  - Costume availability across evolutions
  - Shiny release patterns (often whole family released together)

**Family Suffixes**:
- `_11`: Generic costume family
- `_14`: Specific event costume
- `_f19`: Fall 2019 costume line
- No suffix: Standard evolution family

## Release Date Tracking

`releasedDate` (YYYY/MM/DD format):
- Date shiny variant first became available
- Important for:
  - Historical tracking
  - Determining rarity/availability windows
  - Event documentation
- Does not indicate current availability
- Some early releases had very limited windows

**Notable Release Patterns**:
- Community Days: Entire evolution line released simultaneously
- Legendary releases: Often single species, forms separate
- Event releases: May be temporary, then removed
- Wild releases: Permanently available after release

## Costume and Form Variations

`forms` array tracks special variants:
- Costume Pokémon (event hats, outfits)
- Seasonal forms (Deerling, Sawsbuck)
- Mega Evolution forms
- Event-specific appearances

Each form entry includes:
- `name`: Form identifier code
- `imageUrl`: Specific form's shiny sprite
- `width`, `height`: Image dimensions (always 256×256)

**Form Code Examples**:
- `"f19"`: Fall 2019 costume
- `"11"`: Standard costume variant
- `"14"`: 2014 event costume  
- `"05"`: 2005 celebration costume
- `"M"`: Mega Evolution form
- `"M-X"`: Mega Evolution X form
- `"M-Y"`: Mega Evolution Y form
- `"G"`: Gigantamax form

**Important**: Not all forms available simultaneously. Many costume variants were limited-time.

## Image Assets

**Default Image** (`imageUrl`, `width`, `height`):
- Primary shiny sprite for species
- 256×256px resolution
- From PokeMiners/pogo_assets repository
- Standardized naming: `pokemon_icon_{dex}_{form}_shiny.png`

**Form Images** (in `forms` array):
- Variant-specific sprites
- Same 256×256px resolution  
- May show costumes, Mega forms, etc.
- Allows displaying correct variant for specific occasions

**URL Pattern**:
```
https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_{dex}_{form}_shiny.png
```

Where:
- `{dex}`: Zero-padded dex number (001, 025, 150)
- `{form}`: Form code (00 for standard, 31 for Galarian, 51 for Hisuian, etc.)

## Shiny Availability vs Release

**Important Distinction**:
- `releasedDate`: When shiny FIRST became available
- Current availability may differ:
  - Some shinies were event-exclusive, now unavailable
  - Some had limited windows (Safari Zones, GO Fest)
  - Regional variants may be geographically restricted
  - Costume forms often temporary

**Obtaining Methods** (not in dataset):
- Wild encounters
- Raids (boosted rates)
- Research tasks
- Eggs
- Special Research
- GO Battle League rewards
- Community Day events (highest rates)

## Regional Variant Patterns

**Separate Entries Required**:
- Kantonian Meowth (#52, standard)
- Alolan Meowth (#52, `typeCode: "_61"`)
- Galarian Meowth (#52, `typeCode: "_31"`)

Each has:
- Different release dates
- Different availability methods
- Different shiny sprites
- Same dex number

## Name Formatting

`name` field includes regional prefix:
- "Hisuian Venusaur" (not just "Venusaur")
- "Alolan Vulpix"
- "Galarian Meowth"
- Standard forms have no prefix ("Bulbasaur", not "Kantonian Bulbasaur")

Consistent with in-game display names.

## Mega Evolution Tracking

Mega forms have:
- Same dex number as base form
- Separate entries in array
- Form codes in `forms` array:
  - "M": Single Mega (Mega Venusaur)
  - "M-X": Mega X form (Mega Charizard X)
  - "M-Y": Mega Y form (Mega Charizard Y)
- Release dates when Mega shiny first obtainable

**Mega Shiny Mechanics**:
- Requires base form to be shiny
- Mega Evolving shiny Pokémon shows shiny Mega form
- Shiny status persists through Mega Evolution
- Cannot catch Mega forms directly

## Special Cases

**Spinda**:
- 9 patterns (0-8)
- Each pattern is unique shiny
- Not fully tracked in forms array
- All patterns share same dex number

**Castform**:
- Weather-based forms
- Each form has shiny variant
- Forms: Normal, Sunny, Rainy, Snowy

**Unown**:
- 28 forms (A-Z, !, ?)
- Each letter is separate form
- All have shiny variants
- Tracked separately by form

**Pikachu**:
- Extensive costume variations
- Multiple event forms
- Largest `forms` array in dataset
- Some costumes cannot evolve

**Deerling/Sawsbuck**:
- 4 seasonal forms
- Each season has shiny
- Form depends on hemisphere and season

## Shiny Rate Information (Not in Dataset)

Standard shiny rates (server-side, not in data):
- Wild: ~1/500
- Legendary Raids: ~1/20
- Community Day: ~1/25
- Research: ~1/500  
- Eggs: ~1/60
- GO Battle League: ~1/20

Rates may be boosted during events.

## Important Notes

- Dataset is comprehensive shiny catalog (all-time releases)
- Does NOT indicate current availability
- Does NOT include shiny rates or probabilities  
- Does NOT include obtainability methods
- Historical record; includes removed/unobtainable shinies
- New entries added with each shiny release
- Form codes match PokeMiners asset naming conventions
- Family tracking useful for evolution planning
- Multiple entries per dex number are intentional (regional variants)
- Image URLs use GitHub CDN (jsdelivr.net)
- All images are 256×256px for consistency
- Costume forms may not be currently obtainable
- Release dates in ISO-like format but use slashes (YYYY/MM/DD)

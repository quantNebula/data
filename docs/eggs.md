## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json

## JSON Example

```json
{
    "name": "Bulbasaur",
    "eggType": "1 km",
    "isAdventureSync": false,
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm1.icon.png",
    "imageWidth": 107,
    "imageHeight": 126,
    "canBeShiny": true,
    "shinyAssetUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_001_00_shiny.png",
    "combatPower": {
        "min": 637,
        "max": 637
    },
    "isRegional": false,
    "isGiftExchange": false,
    "rarity": 4,
    "rarityTier": "Very Rare"
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Pokémon name, may include forms ("Galarian Meowth", "Indeedee (Male)") |
| `eggType` | string | Egg distance: "1 km", "2 km", "5 km", "7 km", "10 km", or "12 km" |
| `isAdventureSync` | boolean | True if only from Adventure Sync weekly rewards, not PokéStops |
| `image` | string | URL to Pokémon icon |
| `imageWidth` | integer | Icon width in pixels |
| `imageHeight` | integer | Icon height in pixels |
| `canBeShiny` | boolean | Whether shiny form can hatch |
| `shinyAssetUrl` | string\|null | Shiny sprite URL, null if unavailable |
| `combatPower.min` | integer | Minimum CP (always equals max for eggs) |
| `combatPower.max` | integer | Maximum CP (always equals min for eggs) |
| `isRegional` | boolean | Whether Pokémon is region-locked in wild |
| `isGiftExchange` | boolean | For 7 km eggs: true if only from gifts |
| `rarity` | integer | Numeric rarity 1-5 within egg pool |
| `rarityTier` | string | Text rarity: "Common", "Uncommon", "Rare", "Very Rare", "Ultra Rare" |

## Data Structure

The endpoint returns a flat array of Pokémon objects, where each object represents a species currently available from eggs. Pokémon can appear in multiple entries if they're available from different egg types (e.g., standard 2 km eggs and Adventure Sync 5 km eggs).

## Egg Distance Types

- **1 km**: Starter Pokémon from all generations (Bulbasaur through Quaxly)
- **2 km**: Common baby Pokémon and recent additions
- **5 km**: Standard drops from PokéStops, includes both common and rare species
- **7 km**: Regional variants (Alolan, Galarian, Hisuian) and gift-exclusive Pokémon
- **10 km**: Rare Pokémon including pseudo-legendaries
- **12 km**: Strange Eggs from defeating Team GO Rocket Leaders, contains Dark and Poison types

## Rarity System

The `rarity` field (1-5) and `rarityTier` indicate relative spawn rates within each egg pool:
- **1 (Common)**: ~40-50% hatch rate
- **2 (Uncommon)**: ~25-35% hatch rate
- **3 (Rare)**: ~15-20% hatch rate
- **4 (Very Rare)**: ~5-10% hatch rate
- **5 (Ultra Rare)**: <5% hatch rate

Rarity is independent across egg types (a Common in 10 km eggs is still harder to obtain than a Common in 2 km eggs).

## Adventure Sync Rewards

`isAdventureSync: true` indicates Pokémon exclusively available from weekly Adventure Sync rewards (5 km or 10 km), not from regular PokéStop eggs. These rewards require walking 25 km or 50 km per week respectively.

## Regional and Gift Exclusivity

- `isRegional: true`: Pokémon is region-locked in wild spawns but globally available from eggs
- `isGiftExchange: true`: For 7 km eggs, indicates Pokémon only hatches from gifts from friends (Galarian Corsola, Galarian Slowpoke, etc.)

## Combat Power

All hatched Pokémon are fixed level 20, resulting in identical min/max CP values. This differs from wild catches (level 1-35) and raids (level 20 or 25).

## Shiny Availability

`canBeShiny` indicates if the shiny variant can be obtained from eggs. When true, `shinyAssetUrl` provides the shiny sprite. Some Pokémon have shinies available in other methods but not from eggs.

## Data Updates

Egg pools change:
- During events (temporary additions/removals)
- At season changes (major pool overhauls)
- With new generation releases
- During special occasions (Community Days, raid days)

The dataset always reflects the current active egg pool. Historical data is not maintained.
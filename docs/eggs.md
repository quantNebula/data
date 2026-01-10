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

## Notes

The data is an array of Pokémon objects representing current egg contents across all distance types. Egg pools rotate with events and seasons.
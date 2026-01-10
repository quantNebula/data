## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json

## JSON Example

```json
{
    "name": "Genesect (Burn)",
    "tier": "5-Star Raids",
    "canBeShiny": true,
    "types": [
        {
            "name": "bug",
            "image": "https://leekduck.com/assets/img/types/bug.png",
            "imageWidth": 32,
            "imageHeight": 32
        },
        {
            "name": "steel",
            "image": "https://leekduck.com/assets/img/types/steel.png",
            "imageWidth": 32,
            "imageHeight": 32
        }
    ],
    "combatPower": {
        "normal": { "min": 1833, "max": 1916 },
        "boosted": { "min": 2292, "max": 2395 }
    },
    "boostedWeather": [
        {
            "name": "rainy",
            "image": "https://leekduck.com/assets/img/weather/rainy.png",
            "imageWidth": 32,
            "imageHeight": 32
        },
        {
            "name": "snow",
            "image": "https://leekduck.com/assets/img/weather/snowy.png",
            "imageWidth": 32,
            "imageHeight": 32
        }
    ],
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm649.fBURN.icon.png",
    "imageWidth": 256,
    "imageHeight": 256
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Raid boss Pokémon name, may include forms (e.g., "Genesect (Burn)") |
| `tier` | string | Raid difficulty: "1-Star Raids", "3-Star Raids", "5-Star Raids", or "Mega Raids" |
| `canBeShiny` | boolean | Whether shiny form can be encountered |
| `types` | array | Pokémon types (1-2 elements) |
| `types[].name` | string | Type name (lowercase): "normal", "fire", "water", "electric", "grass", "ice", "fighting", "poison", "ground", "flying", "psychic", "bug", "rock", "ghost", "dragon", "dark", "steel", "fairy" |
| `types[].image` | string | Type icon URL |
| `types[].imageWidth` | integer | Type icon width in pixels |
| `types[].imageHeight` | integer | Type icon height in pixels |
| `combatPower.normal.min` | integer | Minimum CP at base level (20 for 5-Star/Mega, 15 for 1/3-Star) |
| `combatPower.normal.max` | integer | Maximum CP at base level |
| `combatPower.boosted.min` | integer | Minimum CP when weather-boosted (25 for 5-Star/Mega, 20 for 1/3-Star) |
| `combatPower.boosted.max` | integer | Maximum CP when weather-boosted |
| `boostedWeather` | array | Weather conditions that boost this Pokémon |
| `boostedWeather[].name` | string | Weather name: "sunny", "rainy", "partly cloudy", "cloudy", "windy", "snow", or "fog" |
| `boostedWeather[].image` | string | Weather icon URL |
| `boostedWeather[].imageWidth` | integer | Weather icon width in pixels |
| `boostedWeather[].imageHeight` | integer | Weather icon height in pixels |
| `image` | string | Pokémon icon URL |
| `imageWidth` | integer | Pokémon icon width in pixels |
| `imageHeight` | integer | Pokémon icon height in pixels |

## Notes

The data is an array of raid boss objects representing current available raid bosses. Raid bosses rotate regularly with events.

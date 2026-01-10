## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json

## JSON Example

```json
{
    "name": "Giovanni",
    "title": "Team GO Rocket Boss",
    "type": "",
    "typeImage": "",
    "typeImageWidth": null,
    "typeImageHeight": null,
    "gender": "Male",
    "firstPokemon": [
        {
            "name": "Persian",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm53.icon.png",
            "imageWidth": 134,
            "imageHeight": 146,
            "types": ["normal"],
            "isEncounter": false,
            "canBeShiny": false,
            "shinyAssetUrl": null
        }
    ],
    "secondPokemon": [...],
    "thirdPokemon": [...]
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Rocket member name (e.g., "Giovanni", "Cliff", "Fire-type Female Grunt") |
| `title` | string | Role: "Team GO Rocket Boss", "Team GO Rocket Leader", or "Team GO Rocket Grunt" |
| `type` | string | Type specialization (e.g., "fire", "water"), empty string for non-typed encounters |
| `typeImage` | string | Type badge icon URL, empty string for non-typed |
| `typeImageWidth` | integer\|null | Type badge width in pixels, null for non-typed |
| `typeImageHeight` | integer\|null | Type badge height in pixels, null for non-typed |
| `gender` | string | "Male" or "Female" |
| `firstPokemon` | array | Possible first slot Pokémon (one randomly selected per battle) |
| `secondPokemon` | array | Possible second slot Pokémon (one randomly selected per battle) |
| `thirdPokemon` | array | Possible third slot Pokémon (one randomly selected per battle) |
| `*Pokemon[].name` | string | Pokémon name, may include regional forms |
| `*Pokemon[].image` | string | Pokémon icon URL |
| `*Pokemon[].imageWidth` | integer | Icon width in pixels |
| `*Pokemon[].imageHeight` | integer | Icon height in pixels |
| `*Pokemon[].types` | array | Pokémon types |
| `*Pokemon[].isEncounter` | boolean | Whether this Pokémon can be caught after winning |
| `*Pokemon[].canBeShiny` | boolean | Whether shiny form is available for Shadow version |
| `*Pokemon[].shinyAssetUrl` | string\|null | Shiny sprite URL, null if unavailable |

## Notes

The data is an array of Team GO Rocket member lineup objects. Each battle randomly selects one Pokémon from each slot. After defeating a Rocket member, one Pokémon with `isEncounter: true` that appeared in the battle becomes available to catch as a Shadow Pokémon. Gender matters: same type + different gender = completely different lineup.

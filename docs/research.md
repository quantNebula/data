## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json

## JSON Example

```json
{
    "seasonalInfo": {
        "breakthroughPokemon": ["Galarian Mr"],
        "spindaPatterns": [6, 7],
        "season": null
    },
    "tasks": [
        {
            "text": "Catch 7 Pokémon",
            "type": "catch",
            "rewards": [
                {
                    "type": "encounter",
                    "name": "Magikarp",
                    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm129.icon.png",
                    "imageWidth": 112,
                    "imageHeight": 83,
                    "canBeShiny": true,
                    "shinyAssetUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_129_00_shiny.png",
                    "combatPower": { "min": 99, "max": 117 }
                }
            ]
        }
    ]
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `seasonalInfo` | object | Seasonal research information |
| `seasonalInfo.breakthroughPokemon` | array | Pokémon available from 7-day Research Breakthrough (names may be abbreviated) |
| `seasonalInfo.spindaPatterns` | array | Currently available Spinda pattern numbers |
| `seasonalInfo.season` | string\|null | Current season name, null if not applicable |
| `tasks` | array | Field research tasks |
| `tasks[].text` | string | Task requirement description |
| `tasks[].type` | string | Task category: "catch", "throw", "battle", "explore", "training", "buddy", "rocket", "sponsored", or "ar" (may be omitted for special events) |
| `tasks[].rewards` | array | Possible rewards for completing the task |
| `rewards[].type` | string | Reward type: "encounter" (Pokémon) or "item" |
| `rewards[].name` | string | Pokémon or item name |
| `rewards[].image` | string | Reward icon URL |
| `rewards[].imageWidth` | integer | Icon width in pixels |
| `rewards[].imageHeight` | integer | Icon height in pixels |
| `rewards[].canBeShiny` | boolean | Whether Pokémon can be shiny (encounters only) |
| `rewards[].shinyAssetUrl` | string\|null | Shiny sprite URL (encounters only) |
| `rewards[].combatPower` | object | CP range at level 15 (encounters only) |
| `rewards[].combatPower.min` | integer | Minimum CP |
| `rewards[].combatPower.max` | integer | Maximum CP |
| `rewards[].quantity` | integer | Item quantity (items only) |

## Notes

The data is an object containing seasonal information and an array of field research tasks. Tasks rotate monthly and change with events. When multiple rewards exist for a task, typically one is randomly selected upon completion.

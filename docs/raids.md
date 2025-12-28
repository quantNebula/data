
# Endpoints

- Minimized: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json)

**Note:** The endpoint URL assumes the standard Google Cloud Storage public URL format for the bucket `resume_site_goblin` and the `files/` directory structure.

# Example Raid Object

```json
{
    "name": "Alolan Sandshrew",
    "tier": "1-Star Raids",
    "canBeShiny": true,
    "types": [
        {
            "name": "ice",
            "image": "https://leekduck.com/assets/img/types/ice.png"
        },
        {
            "name": "steel",
            "image": "https://leekduck.com/assets/img/types/steel.png"
        }
    ],
    "combatPower": {
        "normal": {
            "min": 688,
            "max": 739
        },
        "boosted": {
            "min": 860,
            "max": 924
        }
    },
    "boostedWeather": [
        {
            "name": "snow",
            "image": "https://leekduck.com/assets/img/weather/snowy.png"
        }
    ],
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm27.fALOLA.icon.png"
}
```

# Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`name`**          | `string`  | The name of the Raid Boss.
| **`tier`**          | `string`  | The difficulty tier of the raid. See [Raid Tiers](#raid-tiers).
| **`canBeShiny`**    | `boolean` | Indicates if the Raid Boss can be encountered in its shiny form.
| **`types`**         | `array`   | An array of objects representing the Pokémon's types.
| **`combatPower`**   | `object`  | Details about the Catch CP range. See [Combat Power Object](#combat-power-object).
| **`boostedWeather`**| `array`   | An array of objects representing the weather conditions that boost this boss.
| **`image`**         | `string`  | URL to the Pokémon's icon image.

## Raid Tiers

The `tier` field identifies the difficulty or category of the raid. Observed values include:

- `1-Star Raids`
- `3-Star Raids`
- `5-Star Raids`
- `Mega Raids`

## Sub-Objects

### Type Object
| Field | Type | Description |
|---|---|---|
| **`name`** | `string` | The name of the type (e.g., "ice", "fire", "steel"). |
| **`image`** | `string` | URL to the type icon image. |

### Combat Power Object
| Field | Type | Description |
|---|---|---|
| **`normal`** | `object` | Object containing `min` and `max` CP for a standard catch (Level 20). |
| **`boosted`** | `object` | Object containing `min` and `max` CP for a weather-boosted catch (Level 25). |

### Boosted Weather Object
| Field | Type | Description |
|---|---|---|
| **`name`** | `string` | The name of the weather condition (e.g., "snow", "partly cloudy", "windy"). |
| **`image`** | `string` | URL to the weather icon image. |

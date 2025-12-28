
# Endpoints

- Minimized: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json)

**Note:** The endpoint URL assumes the standard Google Cloud Storage public URL format for the bucket `resume_site_goblin` and the `files/` directory structure.

# Example Research Object

```json
{
    "seasonalInfo": {
        "breakthroughPokemon": [
            "Galarian Mr. Mime"
        ],
        "spindaPatterns": [
            6,
            7
        ],
        "season": null
    },
    "tasks": [
        {
            "text": "Catch 10 Pokémon",
            "type": "catch",
            "rewards": [
                {
                    "type": "item",
                    "name": "×1500",
                    "image": "https://cdn.leekduck.com/assets/img/items/Stardust.png",
                    "quantity": 1500
                }
            ]
        }
    ]
}
```

# Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`seasonalInfo`**  | `object`  | Information pertinent to the current season's research rotation. See [Seasonal Info](#seasonal-info).
| **`tasks`**         | `array`   | A list of active field research tasks. See [Task Object](#task-object).

## Seasonal Info

| Field | Type | Description |
|---|---|---|
| **`breakthroughPokemon`** | `array` | List of strings representing Pokémon currently available in the weekly Research Breakthrough box. |
| **`spindaPatterns`** | `array` | List of numbers representing available Spinda patterns. |
| **`season`** | `string` | The name of the current season (can be null). |

## Task Object

| Field | Type | Description |
|---|---|---|
| **`text`** | `string` | The text description of the task (e.g., "Catch 5 Fire-type Pokémon"). |
| **`type`** | `string` | The category of the task (e.g., "catch", "throw", "battle", "explore", "buddy"). Optional. |
| **`rewards`** | `array` | A list of possible rewards for completing this task. See [Reward Object](#reward-object). |

## Reward Object

The `rewards` array contains objects that can represent either an item or a Pokémon encounter.

| Field | Type | Description |
|---|---|---|
| **`type`** | `string` | The type of reward: `"item"` or `"encounter"`. |
| **`name`** | `string` | The name of the reward item or Pokémon. |
| **`image`** | `string` | URL to the reward image. |
| **`quantity`** | `number` | (Item only) The quantity of the item rewarded. |
| **`canBeShiny`** | `boolean` | (Encounter only) Indicates if the Pokémon can be shiny. |
| **`combatPower`** | `object` | (Encounter only) The CP range (`min`, `max`) for the encounter. |

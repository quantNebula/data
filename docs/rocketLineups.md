
# Endpoints

- Minimized: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json)

**Note:** The endpoint URL assumes the standard Google Cloud Storage public URL format for the bucket `resume_site_goblin` and the `files/` directory structure.

# Example Rocket Lineup Object

```json
{
    "name": "Cliff",
    "title": "Team GO Rocket Leader",
    "type": "",
    "gender": "Male",
    "firstPokemon": [
        {
            "name": "Larvitar",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm246.icon.png",
            "types": [
                "rock",
                "ground"
            ],
            "isEncounter": true,
            "canBeShiny": true
        }
    ],
    "secondPokemon": [
        {
            "name": "Annihilape",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm979.icon.png",
            "types": [
                "fighting",
                "ghost"
            ],
            "isEncounter": false,
            "canBeShiny": false
        }
    ],
    "thirdPokemon": [
        {
            "name": "Tyranitar",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm248.icon.png",
            "types": [
                "rock",
                "dark"
            ],
            "isEncounter": false,
            "canBeShiny": false
        }
    ]
}
```

# Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`name`**          | `string`  | The name of the Team GO Rocket member.
| **`title`**         | `string`  | The rank or title of the member. See [Rocket Titles](#rocket-titles).
| **`type`**          | `string`  | The type specialty of the member (e.g., "fire", "water"). Empty for Leaders/Boss.
| **`gender`**        | `string`  | The gender of the member ("Male" or "Female").
| **`firstPokemon`**  | `array`   | List of possible Pokémon in the first slot. See [Pokemon Object](#pokemon-object).
| **`secondPokemon`** | `array`   | List of possible Pokémon in the second slot. See [Pokemon Object](#pokemon-object).
| **`thirdPokemon`**  | `array`   | List of possible Pokémon in the third slot. See [Pokemon Object](#pokemon-object).

## Rocket Titles

Observed titles include:

- `Team GO Rocket Boss`
- `Team GO Rocket Leader`
- `Team GO Rocket Grunt`

## Pokemon Object

| Field | Type | Description |
|---|---|---|
| **`name`** | `string` | The name of the Shadow Pokémon. |
| **`image`** | `string` | URL to the Pokémon's icon image. |
| **`types`** | `array` | List of strings representing the Pokémon's types (e.g., `["rock", "ground"]`). |
| **`isEncounter`** | `boolean` | Indicates if this Pokémon can be encountered (rescued) after defeating the member. |
| **`canBeShiny`** | `boolean` | Indicates if the Shadow Pokémon can be shiny. |

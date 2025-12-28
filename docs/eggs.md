
# Endpoints

- Minimized: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json)

**Note:** The endpoint URL assumes the standard Google Cloud Storage public URL format for the bucket `resume_site_goblin` and the `files/` directory structure.

# Example Egg Object

```json
{
    "name": "Bulbasaur",
    "eggType": "1 km",
    "isAdventureSync": false,
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm1.icon.png",
    "canBeShiny": true,
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

# Fields

| Field               | Type      | Description
|-------------------- |---------- |---------------------
| **`name`**          | `string`  | The name of the Pokémon.
| **`eggType`**       | `string`  | The distance required to hatch the egg. See [Egg Types](#egg-types).
| **`isAdventureSync`**| `boolean` | Indicates if this egg is obtained specifically through Adventure Sync rewards.
| **`image`**         | `string`  | URL to the Pokémon's icon image.
| **`canBeShiny`**    | `boolean` | Indicates if the Pokémon can be encountered in its shiny form from this egg.
| **`combatPower`**   | `object`  | An object containing the CP range for the hatched Pokémon (`min` and `max`).
| **`isRegional`**    | `boolean` | Indicates if this Pokémon is exclusive to a specific region.
| **`isGiftExchange`**| `boolean` | Indicates if this egg is obtained by opening Gifts from friends (typically 7km eggs).
| **`rarity`**        | `number`  | A numeric representation of rarity (1 to 5).
| **`rarityTier`**    | `string`  | A text description of the rarity tier. See [Rarity Tiers](#rarity-tiers).

## Egg Types

The `eggType` field represents the walking distance group or category of the egg. Observed values:

- `1 km`
- `2 km`
- `5 km`
- `7 km`
- `10 km`
- `12 km`

## Rarity Tiers

Rarity is expressed both as a number (`rarity`) and a human-readable string (`rarityTier`).

| Rarity Level | Tier Name |
|---|---|
| 1 | Common |
| 2 | Uncommon |
| 3 | Rare |
| 4 | Very Rare |
| 5 | Ultra Rare |

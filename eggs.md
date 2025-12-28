# Eggs Data - eggs.min.json

This file contains an array of all Pokemon currently available from eggs in Pokemon GO, including standard eggs, Adventure Sync rewards, and Gift eggs. Data is automatically scraped and updated.

## Overview

The `eggs.min.json` file provides comprehensive information about egg hatches, including which Pokemon are available from each egg distance, their rarity tiers, CP ranges at hatch, shiny availability, and special conditions like regional exclusivity or Gift egg availability.

## Endpoints 

- **Minimized**: [`GET https://storage.googleapis.com/resume_site_goblin/files/eggs.min.json`](https://storage.googleapis.com/resume_site_goblin/files/eggs.min.json)
- **CDN (jsDelivr)**: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json)

---

## Data Structure

The root of the file is an **array** of Pokemon egg objects.

```json
[
  { /* egg Pokemon object */ },
  { /* egg Pokemon object */ },
  ...
]
```

---

## Example Egg Object

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

---

## Usage Notes

- **Data Updates**: This file is automatically updated via web scraper. Egg pools change with events and seasons, often monthly.
- **Hatch CP**: The CP values represent the hatch level (typically Level 20), not the maximum possible CP for that species.
- **Rarity System**: Higher rarity values (4-5) indicate rarer hatches within that egg tier. Rarity affects hatch probability.
- **Adventure Sync**: Some Pokemon are exclusive to Adventure Sync reward eggs (50km weekly reward). These are marked with `isAdventureSync: true`.
- **Gift Eggs (7km)**: Most 7km eggs come from opening Gifts from friends. These often contain regional Pokemon and baby Pokemon.
- **Regional Exclusives**: Pokemon with `isRegional: true` are typically only available in specific geographic regions, though they may appear in 7km eggs globally during events.
- **Strange Eggs (12km)**: Obtained by defeating Team Rocket Leaders. These eggs contain Dark and Poison-type Pokemon.

---

## Egg Types Explained

### 1 km Eggs
- **Source**: Rare drops from PokéStops and Gyms (usually during events)
- **Content**: Typically event-themed Pokemon or starters
- **Hatch Distance**: 1 kilometer
- **Incubator Use**: Best for limited-use incubators

### 2 km Eggs
- **Source**: Common drops from PokéStops and Gyms
- **Content**: Common Pokemon, some starters, early-evolution Pokemon
- **Hatch Distance**: 2 kilometers
- **Incubator Use**: Good for infinite incubator

### 5 km Eggs
- **Source**: Very common drops from PokéStops and Gyms
- **Content**: Wide variety of Pokemon, moderate rarity
- **Hatch Distance**: 5 kilometers
- **Incubator Use**: Versatile egg tier

### 7 km Eggs
- **Source**: Opening Gifts from friends
- **Content**: Regional Pokemon, Alolan forms, baby Pokemon, Galarian forms
- **Hatch Distance**: 7 kilometers
- **Incubator Use**: Excellent for regional collecting

### 10 km Eggs
- **Source**: Uncommon drops from PokéStops and Gyms
- **Content**: Rare Pokemon, pseudo-legendaries, fossil Pokemon
- **Hatch Distance**: 10 kilometers
- **Incubator Use**: Priority for Super Incubators

### 12 km Eggs (Strange Eggs)
- **Source**: Defeating Team Rocket Leaders (Cliff, Sierra, Arlo, Giovanni)
- **Content**: Dark and Poison-type Pokemon, including rare species
- **Hatch Distance**: 12 kilometers
- **Incubator Use**: High priority for Super Incubators
- **Special Note**: Requires an empty egg slot when defeating a Leader

---

## Rarity Tier System

The rarity system indicates the likelihood of hatching a particular Pokemon from an egg:

| Rarity | Tier Name | Approximate Hatch Rate |
|--------|-----------|----------------------|
| 1 | Common | ~40-50% of hatches |
| 2 | Uncommon | ~25-35% of hatches |
| 3 | Rare | ~10-20% of hatches |
| 4 | Very Rare | ~5-10% of hatches |
| 5 | Ultra Rare | ~1-5% of hatches |

**Note**: Actual hatch rates may vary and are not officially disclosed by Niantic.

---

## Integration Examples

### Filtering by Egg Distance

```javascript
const eggs = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json').then(r => r.json());

const egg2km = eggs.filter(e => e.eggType === '2 km');
const egg7km = eggs.filter(e => e.eggType === '7 km');
const egg10km = eggs.filter(e => e.eggType === '10 km');
const strangeEggs = eggs.filter(e => e.eggType === '12 km');
```

### Finding Shiny-Available Pokemon

```javascript
const shinyAvailable = eggs.filter(e => e.canBeShiny);
console.log(`${shinyAvailable.length} egg Pokemon can be shiny!`);

// Group by egg type
const shinyByEggType = {};
shinyAvailable.forEach(egg => {
  if (!shinyByEggType[egg.eggType]) {
    shinyByEggType[egg.eggType] = [];
  }
  shinyByEggType[egg.eggType].push(egg.name);
});
```

### Finding Regional Exclusives

```javascript
const regionals = eggs.filter(e => e.isRegional);
console.log('Regional Pokemon available in eggs:');
regionals.forEach(egg => {
  console.log(`${egg.name} - ${egg.eggType} (${egg.rarityTier})`);
});
```

### Finding Adventure Sync Rewards

```javascript
const adventureSyncRewards = eggs.filter(e => e.isAdventureSync);
console.log('Adventure Sync reward Pokemon:');
adventureSyncRewards.forEach(egg => {
  console.log(`${egg.name} - ${egg.eggType}`);
});
```

### Grouping by Rarity

```javascript
function groupByRarity(eggs) {
  const groups = {
    'Common': [],
    'Uncommon': [],
    'Rare': [],
    'Very Rare': [],
    'Ultra Rare': []
  };
  
  eggs.forEach(egg => {
    groups[egg.rarityTier].push(egg);
  });
  
  return groups;
}

const egg10km = eggs.filter(e => e.eggType === '10 km');
const grouped = groupByRarity(egg10km);

console.log('10km Egg Rarity Distribution:');
Object.entries(grouped).forEach(([tier, pokemon]) => {
  console.log(`${tier}: ${pokemon.length} Pokemon`);
});
```

### Finding Best Eggs for Shiny Hunting

```javascript
// Find shiny-available Pokemon with highest rarity
const premiumShinyTargets = eggs.filter(e => 
  e.canBeShiny && e.rarity >= 4 && e.eggType === '10 km'
);

premiumShinyTargets.sort((a, b) => b.rarity - a.rarity);
console.log('Premium shiny targets in 10km eggs:');
premiumShinyTargets.forEach(egg => {
  console.log(`${egg.name} - ${egg.rarityTier}`);
});
```

### Calculating Hatch Rewards

```javascript
function estimateHatchRewards(egg) {
  const baseStardust = {
    '2 km': 400,
    '5 km': 800,
    '7 km': 800,
    '10 km': 1600,
    '12 km': 3200
  };
  
  const baseCandy = {
    '2 km': 5,
    '5 km': 10,
    '7 km': 10,
    '10 km': 16,
    '12 km': 32
  };
  
  return {
    pokemon: egg.name,
    cp: `${egg.combatPower.min}-${egg.combatPower.max}`,
    stardust: baseStardust[egg.eggType] || 0,
    candy: `${baseCandy[egg.eggType]}-${baseCandy[egg.eggType] + 5}`,
    shinyChance: egg.canBeShiny ? 'Yes' : 'No',
    rarity: egg.rarityTier
  };
}
```

---

## Egg Management Tips

### Optimizing Incubator Use

1. **Infinite Incubator**: Use for 2km eggs (most common, shortest distance)
2. **Limited-Use Incubators**: Prioritize 10km and 12km eggs
3. **Super Incubators**: Best for 12km eggs (1.5x hatch speed)
4. **Event Incubators**: Save for events with boosted shiny rates or exclusive Pokemon

### Maximizing Egg Slots

- Keep egg slots full to avoid picking up unwanted egg types
- Clear 2km and 5km eggs when hunting for 10km eggs
- During events, keep slots open for event-exclusive egg types

### Adventure Sync Strategy

- Walk 50km per week to earn Adventure Sync rewards
- Ensure egg space is available Monday morning (reward distribution time)
- Adventure Sync eggs can contain exclusive Pokemon not available in regular eggs

---

## Data Source

This data is automatically scraped and updated to keep current with egg pool rotations.

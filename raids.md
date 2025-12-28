# Raids Data - raids.min.json

This file contains an array of current raid boss data for Pokemon GO, including all raid tiers from 1-Star to Mega Raids. Data is automatically scraped and updated.

## Overview

The `raids.min.json` file provides comprehensive information about all currently available raid bosses in Pokemon GO. Each raid boss entry includes details about difficulty tier, CP ranges (both normal and weather-boosted), Pokemon types, weather boost conditions, and shiny availability.

## Endpoints

- **Minimized**: [`GET https://storage.googleapis.com/resume_site_goblin/files/raids.min.json`](https://storage.googleapis.com/resume_site_goblin/files/raids.min.json)
- **CDN (jsDelivr)**: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json)

---

## Data Structure

The root of the file is an **array** of raid boss objects.

```json
[
  { /* raid boss object */ },
  { /* raid boss object */ },
  ...
]
```

---

## Example Raid Object

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

---

## Usage Notes

- **Data Updates**: This file is automatically updated via web scraper. Raid bosses change frequently based on current events and rotations.
- **CP Calculation**: The CP ranges represent catch values (Level 20 for normal, Level 25 for weather-boosted), not the CP during battle.
- **Weather Boost**: When weather conditions match a raid boss's boosted weather types, the catch CP is higher and the boss receives in-battle stat boosts.
- **Image URLs**: All image URLs point to external CDN sources.
- **Shiny Availability**: The `canBeShiny` field indicates whether the shiny form is currently available. This can change with events.
- **Raid Rotations**: Legendary and Mega raids typically rotate on a schedule (often monthly or bi-weekly), while lower tiers may change more or less frequently.

---

## Raid Tier Details

### 1-Star Raids
- **Difficulty**: Easy, soloable by most trainers
- **Typical Catch CP**: 600-1,200
- **Common Pokemon**: Starter Pokemon, regional variants, uncommon species

### 3-Star Raids
- **Difficulty**: Moderate, soloable by high-level trainers with optimal counters
- **Typical Catch CP**: 1,200-2,000
- **Common Pokemon**: Evolved forms, pseudo-legendaries, powerful species

### 5-Star Raids
- **Difficulty**: Hard, typically requires 3-5 trainers
- **Typical Catch CP**: 2,000-2,500
- **Common Pokemon**: Legendary Pokemon, Mythical Pokemon (special events)

### Mega Raids
- **Difficulty**: Hard, typically requires 3-5 trainers
- **Typical Catch CP**: 1,800-2,500 (base form)
- **Common Pokemon**: Mega-evolvable species
- **Note**: Catching awards Mega Energy for that species

---

## Weather Boost Mechanics

Weather boost affects raids in two ways:

1. **In-Battle Boost**: The raid boss receives a stat increase, making it slightly harder
2. **Catch CP Boost**: Successful catches are Level 25 instead of Level 20 (approximately 20% higher CP)

### Weather Types

| Weather | Boosts These Types |
|---------|-------------------|
| Clear/Sunny | Fire, Grass, Ground |
| Partly Cloudy | Normal, Rock |
| Cloudy | Fairy, Fighting, Poison |
| Rainy | Water, Electric, Bug |
| Snow | Ice, Steel |
| Windy | Dragon, Flying, Psychic |
| Fog | Dark, Ghost |

---

## Integration Examples

### Filtering Raids by Tier

```javascript
const raids = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json').then(r => r.json());

const legendaryRaids = raids.filter(r => r.tier === '5-Star Raids');
const megaRaids = raids.filter(r => r.tier === 'Mega Raids');
const easyRaids = raids.filter(r => r.tier === '1-Star Raids');
```

### Finding Shiny-Available Bosses

```javascript
const shinyAvailable = raids.filter(r => r.canBeShiny);
console.log(`${shinyAvailable.length} raid bosses can be shiny!`);
```

### Checking Weather Boost

```javascript
function isWeatherBoosted(raid, currentWeather) {
  return raid.boostedWeather.some(w => w.name === currentWeather);
}

// Example: Check if Zekrom is boosted in windy weather
const zekrom = raids.find(r => r.name === 'Zekrom');
if (isWeatherBoosted(zekrom, 'windy')) {
  console.log('Zekrom is weather boosted! Catch CP:', zekrom.combatPower.boosted);
}
```

### Displaying Raid Boss Info

```javascript
function displayRaid(raid) {
  const typeNames = raid.types.map(t => t.name).join(', ');
  const weatherNames = raid.boostedWeather.map(w => w.name).join(', ');
  const cpRange = `${raid.combatPower.normal.min}-${raid.combatPower.normal.max}`;
  const boostedCpRange = `${raid.combatPower.boosted.min}-${raid.combatPower.boosted.max}`;
  
  return `
    ${raid.name} (${raid.tier})
    Types: ${typeNames}
    Catch CP: ${cpRange} (${boostedCpRange} boosted)
    Weather Boost: ${weatherNames}
    Shiny: ${raid.canBeShiny ? 'Available' : 'Not Available'}
  `;
}
```

---

## Data Source

This data is automatically scraped and updated to keep current with in-game raid rotations.

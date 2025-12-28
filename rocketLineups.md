# Team GO Rocket Lineups Data - rocketLineups.min.json

This file contains an array of Team GO Rocket member battle lineups, including Grunts, Leaders (Cliff, Sierra, Arlo), and Giovanni. Data is automatically scraped and updated.

## Overview

The `rocketLineups.min.json` file provides comprehensive information about Team GO Rocket battles, including each member's possible Pokemon lineup across all three battle slots, Shadow Pokemon that can be caught, and shiny availability.

## Endpoints

- **Minimized**: [`GET https://storage.googleapis.com/resume_site_goblin/files/rocketLineups.min.json`](https://storage.googleapis.com/resume_site_goblin/files/rocketLineups.min.json)
- **CDN (jsDelivr)**: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json)

---

## Data Structure

The root of the file is an **array** of Team GO Rocket member objects.

```json
[
  { /* Rocket member object */ },
  { /* Rocket member object */ },
  ...
]
```

---

## Example Rocket Lineup Object

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

---

## Usage Notes

- **Data Updates**: This file is automatically updated via web scraper. Leader lineups change every few months, while Giovanni's lineup changes with special research releases.
- **Battle Mechanics**: Team GO Rocket members use Shadow Pokemon with boosted attack but reduced defense.
- **Lineup Randomization**: Each slot may have multiple possible Pokemon. The actual Pokemon used is randomly selected at battle start.
- **Encounter Rewards**: Only Pokemon with `isEncounter: true` can be caught after victory. This is typically the first Pokemon in the lineup, except for Leaders/Giovanni where it's usually the first slot.
- **Shiny Shadow Pokemon**: Some Shadow Pokemon can be shiny. These are extremely rare and highly sought after.
- **Strange Eggs**: Defeating Leaders (when inventory space is available) awards a 12km Strange Egg.

---

## Team GO Rocket Structure

### Grunts
- **Encounters**: Found at invaded PokéStops (black)
- **Lineup Pattern**: Usually themed by type (e.g., Fire, Water, Dragon)
- **Difficulty**: Easy to moderate
- **Rewards**: Shadow Pokemon (catchable), 500 Stardust, items

### Leaders (Cliff, Sierra, Arlo)
- **Encounters**: Found at invaded PokéStops after using a Rocket Radar
- **Lineup Pattern**: Each Leader has a fixed set of possible Pokemon per slot
- **Difficulty**: Hard - requires strong counters
- **Rewards**: Shadow Pokemon (catchable), 1000 Stardust, 12km Strange Egg, items
- **Rocket Radar**: Assembled from 6 Mysterious Components (dropped by Grunts)

### Giovanni (Boss)
- **Encounters**: Found using a Super Rocket Radar (from Special Research)
- **Lineup Pattern**: First slot is always Persian (Shadow Persian), middle slot varies, third slot is a Legendary Shadow Pokemon
- **Difficulty**: Very Hard - requires optimal team
- **Rewards**: Legendary Shadow Pokemon (catchable), 5000 Stardust, items
- **Super Rocket Radar**: Obtained only through Special Research quests
- **Decoys**: Many Grunts will disguise as Giovanni when using Super Rocket Radar

---

## Battle Mechanics

### Shadow Pokemon Stats

Shadow Pokemon used by Team GO Rocket have:
- **+20% Attack**: Makes them hit harder
- **-20% Defense**: Makes them take more damage
- **Weather Boost**: Can be weather boosted like regular Pokemon

### Shields

- **Grunts**: 0-1 shields
- **Leaders**: 2 shields (use on first two Charged Attacks)
- **Giovanni**: 2 shields (use on first two Charged Attacks)

**Strategy**: Use fast-charging Charged Moves to burn through shields quickly.

### Type Effectiveness

Type advantages are crucial against Team GO Rocket:
- Super effective moves deal 1.6× damage
- Not very effective moves deal 0.625× damage
- Double weaknesses can make battles much easier

---

## Grunt Type Specializations

Grunts often announce their type specialty with dialogue clues:

| Dialogue Hint | Type | Example Pokemon |
|---------------|------|-----------------|
| "Normal does not mean weak!" | Normal | Rattata, Snorlax |
| "These waters are treacherous!" | Water | Mudkip, Poliwag |
| "Check out my cute Pokémon!" | Fairy | Snubbull, Kirlia |
| "Do you know how hot Pokémon fire breath can get?" | Fire | Charmander, Houndour |
| "Get ready to be shocked!" | Electric | Magnemite, Electabuzz |
| "Are you scared of psychics that use unseen power?" | Psychic | Abra, Drowzee |
| "Ke-ke-ke-ke-ke-ke!" | Ghost | Shuppet, Duskull |
| "ROAR! ...That's how a dragon roars!" | Dragon | Dratini, Bagon |
| "This buff physique isn't just for show!" | Fighting | Machop, Hitmonlee |
| "You're gonna be defeated!" | Mixed lineup | Various types |

---

## Integration Examples

### Finding Leaders

```javascript
const lineups = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json').then(r => r.json());

const leaders = lineups.filter(r => r.title === 'Team GO Rocket Leader');
const giovanni = lineups.find(r => r.title === 'Team GO Rocket Boss');

console.log('Current Leaders:', leaders.map(l => l.name).join(', '));
```

### Finding Shiny-Available Shadow Pokemon

```javascript
const shinyAvailable = lineups.flatMap(member => {
  return [...member.firstPokemon, ...member.secondPokemon, ...member.thirdPokemon]
    .filter(p => p.canBeShiny && p.isEncounter);
});

console.log('Shiny Shadow Pokemon available:');
shinyAvailable.forEach(p => console.log(`- ${p.name}`));
```

### Finding Catchable Pokemon

```javascript
function getCatchablePokemon(member) {
  const catchable = [];
  
  member.firstPokemon.forEach(p => {
    if (p.isEncounter) catchable.push(p);
  });
  member.secondPokemon.forEach(p => {
    if (p.isEncounter) catchable.push(p);
  });
  member.thirdPokemon.forEach(p => {
    if (p.isEncounter) catchable.push(p);
  });
  
  return catchable;
}

const cliff = lineups.find(l => l.name === 'Cliff');
const cliffRewards = getCatchablePokemon(cliff);
console.log('Possible Shadow Pokemon from Cliff:', cliffRewards.map(p => p.name));
```

### Building Counter Teams

```javascript
// Find all Pokemon used by a specific Leader
function getLeaderPokemon(leader) {
  return {
    slot1: leader.firstPokemon.map(p => ({ name: p.name, types: p.types })),
    slot2: leader.secondPokemon.map(p => ({ name: p.name, types: p.types })),
    slot3: leader.thirdPokemon.map(p => ({ name: p.name, types: p.types }))
  };
}

// Type effectiveness reference (simplified)
const typeCounters = {
  'normal': ['fighting'],
  'fire': ['water', 'ground', 'rock'],
  'water': ['electric', 'grass'],
  'electric': ['ground'],
  'grass': ['fire', 'ice', 'flying', 'bug', 'poison'],
  'ice': ['fire', 'fighting', 'rock', 'steel'],
  'fighting': ['flying', 'psychic', 'fairy'],
  'poison': ['ground', 'psychic'],
  'ground': ['water', 'grass', 'ice'],
  'flying': ['electric', 'ice', 'rock'],
  'psychic': ['bug', 'ghost', 'dark'],
  'bug': ['fire', 'flying', 'rock'],
  'rock': ['water', 'grass', 'fighting', 'ground', 'steel'],
  'ghost': ['ghost', 'dark'],
  'dragon': ['ice', 'dragon', 'fairy'],
  'dark': ['fighting', 'bug', 'fairy'],
  'steel': ['fire', 'fighting', 'ground'],
  'fairy': ['poison', 'steel']
};
```

### Finding Grunts by Type

```javascript
const grunts = lineups.filter(r => r.title === 'Team GO Rocket Grunt');

// Group by type specialty
const gruntsByType = {};
grunts.forEach(grunt => {
  const type = grunt.type || 'Mixed';
  if (!gruntsByType[type]) {
    gruntsByType[type] = [];
  }
  gruntsByType[type].push(grunt);
});

console.log('Grunt Types:', Object.keys(gruntsByType).join(', '));
```

### Checking Giovanni's Legendary

```javascript
const giovanni = lineups.find(l => l.name === 'Giovanni');
if (giovanni) {
  const legendary = giovanni.thirdPokemon[0]; // Giovanni's third slot is always the legendary
  console.log(`Current Giovanni reward: ${legendary.name}`);
  console.log(`Types: ${legendary.types.join(', ')}`);
  console.log(`Can be Shiny: ${legendary.canBeShiny ? 'Yes' : 'No'}`);
}
```

---

## Battle Strategy Tips

### General Tips

1. **Use Shields Wisely**: Save your shields for Pokemon weak to opponent's attacks
2. **Burn Their Shields**: Use fast-charging moves to remove opponent shields quickly
3. **Type Advantage**: Always bring counters that are super effective against possible lineups
4. **Fast Move Damage**: Some fast moves deal significant damage - don't neglect them
5. **Switch Strategy**: Switching Pokemon gives a brief window where opponent can't attack

### Leader-Specific Strategy

**Cliff**: 
- Often leads with Rock/Ground types
- Bring Water, Grass, or Ice counters
- Watch for Fighting types in second slot

**Sierra**:
- Variable lineup with diverse types
- Flexible team recommended
- Often includes Ghost or Dark types

**Arlo**:
- Frequently uses Steel and Psychic types
- Fire and Fighting types very effective
- Be prepared for Dragon types

**Giovanni**:
- Always starts with Persian (Normal) - Fighting counters essential
- Middle slot varies - have backup options
- Third slot Legendary - research counters beforehand
- Most challenging battles in the game

---

## Purification

After catching Shadow Pokemon, you can choose to purify them:

### Purification Benefits
- Reduced Stardust and Candy costs for power-ups
- Increases IVs by 2 points each (to a maximum of 15)
- Gains Return (Normal-type Charged Move)
- Cheaper to trade

### Shadow Benefits (Not Purifying)
- +20% Attack boost in battles
- Unique appearance with red eyes and dark aura
- Some Shadow Pokemon are better for raids/PvP
- Rarity and collector value

**Strategy**: Keep Shadow for meta-relevant Pokemon (Mewtwo, Machamp, etc.), purify for Lucky Dex or cost savings.

---

## Data Source

This data is automatically scraped and updated to keep current with lineup rotations.

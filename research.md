# Research Data - research.min.json

This file contains current field research tasks and seasonal research information for Pokemon GO. Data is automatically scraped and updated.

## Overview

The `research.min.json` file provides comprehensive information about all currently available field research tasks that can be obtained by spinning PokéStops. It also includes seasonal information like Research Breakthrough rewards and available Spinda patterns.

## Endpoints

- **Minimized**: [`GET https://storage.googleapis.com/resume_site_goblin/files/research.min.json`](https://storage.googleapis.com/resume_site_goblin/files/research.min.json)
- **CDN (jsDelivr)**: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json)

---

## Data Structure

The root of the file is an **object** with two main keys: `seasonalInfo` and `tasks`.

```json
{
  "seasonalInfo": { /* seasonal data */ },
  "tasks": [ /* array of task objects */ ]
}
```

---

## Example Research Object

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

---

## Usage Notes

- **Data Updates**: This file is automatically updated via web scraper. Field research tasks change monthly, typically at the start of each new event or season.
- **Task Availability**: Not all tasks appear at every PokéStop. Task distribution is randomized, and some tasks are rarer than others.
- **Research Breakthrough**: Completing 7 days of field research (one stamp per day) awards a Research Breakthrough encounter with one of the listed Pokemon.
- **Spinda Patterns**: Spinda has multiple form patterns. The `spindaPatterns` array indicates which patterns are currently available from field research.
- **Task Types**: The `type` field categorizes tasks by activity (catch, throw, battle, etc.), which can help organize them by difficulty or play style.
- **Reward Variation**: Some tasks may have multiple possible rewards, though the data typically shows the most common or guaranteed reward.

---

## Task Type Categories

Field research tasks are categorized by the type of activity required:

| Type | Description | Examples |
|------|-------------|----------|
| `catch` | Catching Pokemon | "Catch 10 Pokémon", "Catch 5 Fire-type Pokémon" |
| `throw` | Making throws | "Make 3 Great Throws", "Make 5 Curveball Throws in a row" |
| `battle` | Battling | "Win a raid", "Win 5 raids", "Battle in GO Battle League" |
| `explore` | Walking/spinning | "Spin 3 PokéStops or Gyms", "Walk 2 km" |
| `buddy` | Buddy activities | "Earn 3 hearts with your buddy", "Earn 5 hearts with your buddy" |
| `power` | Power-ups | "Power up Pokémon 5 times" |
| `evolve` | Evolution | "Evolve a Pokémon" |
| `photo` | Snapshots | "Take a snapshot of a wild Pokémon" |
| `hatch` | Hatching eggs | "Hatch an Egg", "Hatch 2 Eggs" |

---

## Research Breakthrough

Trainers can earn one field research stamp per day by completing any field research task. After collecting 7 stamps (across 7 different days), a Research Breakthrough is awarded.

### Breakthrough Rewards

The `breakthroughPokemon` array lists Pokemon that can be encountered from Research Breakthroughs. The encounter is guaranteed to be one of the listed Pokemon.

**Additional Breakthrough Rewards** (not shown in this data):
- 2000 XP
- 3000 Stardust
- Multiple item bundles (Rare Candy, Silver Pinap Berries, etc.)

---

## Spinda Patterns

Spinda is a special Pokemon with multiple cosmetic patterns (spot arrangements on its body). Each pattern is identified by a number. The `spindaPatterns` array indicates which pattern(s) are currently available from field research tasks.

**Note**: Typically, tasks like "Make 5 Great Curveball Throws in a row" reward Spinda encounters.

---

## Integration Examples

### Filtering Tasks by Type

```javascript
const research = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json').then(r => r.json());

const catchTasks = research.tasks.filter(t => t.type === 'catch');
const throwTasks = research.tasks.filter(t => t.type === 'throw');
const battleTasks = research.tasks.filter(t => t.type === 'battle');
```

### Finding Pokemon Encounter Rewards

```javascript
const encounterTasks = research.tasks.filter(task => 
  task.rewards.some(reward => reward.type === 'encounter')
);

encounterTasks.forEach(task => {
  const pokemon = task.rewards.find(r => r.type === 'encounter');
  console.log(`${task.text} -> ${pokemon.name} ${pokemon.canBeShiny ? '✨' : ''}`);
});
```

### Finding Shiny-Available Encounters

```javascript
const shinyAvailable = research.tasks.filter(task =>
  task.rewards.some(r => r.type === 'encounter' && r.canBeShiny)
);

console.log(`${shinyAvailable.length} tasks can reward shiny Pokemon!`);
```

### Grouping Tasks by Reward Type

```javascript
function groupTasksByReward(tasks) {
  const groups = {
    encounters: [],
    stardust: [],
    items: [],
    other: []
  };
  
  tasks.forEach(task => {
    const firstReward = task.rewards[0];
    if (firstReward.type === 'encounter') {
      groups.encounters.push(task);
    } else if (firstReward.name.includes('Stardust')) {
      groups.stardust.push(task);
    } else if (firstReward.type === 'item') {
      groups.items.push(task);
    } else {
      groups.other.push(task);
    }
  });
  
  return groups;
}
```

### Displaying Current Breakthrough Pokemon

```javascript
const { seasonalInfo } = research;
console.log('Current Research Breakthrough rewards:');
seasonalInfo.breakthroughPokemon.forEach(pokemon => {
  console.log(`- ${pokemon}`);
});

if (seasonalInfo.spindaPatterns.length > 0) {
  console.log(`\nSpinda patterns available: ${seasonalInfo.spindaPatterns.join(', ')}`);
}
```

---

## Common Rewards

### Item Rewards

Common item rewards from field research include:
- **Stardust**: Ranging from 500 to 3000+
- **Poke Balls**: Great Balls, Ultra Balls
- **Berries**: Razz Berry, Pinap Berry, Nanab Berry, Silver Pinap Berry, Golden Razz Berry
- **Rare Items**: Rare Candy, TMs (Fast and Charged), Star Pieces
- **Evolution Items**: Sinnoh Stone, Unova Stone, etc.

### Pokemon Encounter Rewards

Encounter rewards vary by season and events, but commonly include:
- **Starter Pokemon**: Including their regional variants
- **Rare Species**: Pokemon that are uncommon in the wild
- **Event-Related Pokemon**: Featured during active events
- **Shiny-Available Species**: Many encounter rewards can be shiny

---

## Data Source

This data is automatically scraped and updated to keep current with monthly research rotations.

# Pokémon GO Field Research Data Schema

## Overview

The `research.min.json` file contains data about current Field Research tasks in Pokémon GO. This dataset describes available research tasks, their requirements, and possible rewards including Pokémon encounters, items, and Stardust.

**Note:** Field Research tasks change frequently with events and monthly rotations. This documentation focuses on the data structure rather than specific current contents.

## Data Access

The data is available via CDN:

```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json
```

### Usage Example

```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json')
  .then(response => response.json())
  .then(data => {
    console.log(`Loaded ${data.tasks.length} field research tasks`);
  });
```

## File Structure

The file contains a JSON object with two main sections: seasonal information and an array of research tasks.

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
      "rewards": [...]
    }
  ]
}
```

## JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Field Research Data Schema",
  "description": "Schema for Pokémon GO field research tasks",
  "type": "object",
  "required": ["seasonalInfo", "tasks"],
  "properties": {
    "seasonalInfo": {
      "type": "object",
      "description": "Seasonal metadata about research rewards",
      "properties": {
        "breakthroughPokemon": {
          "type": "array",
          "description": "List of Pokémon that can be encountered from Research Breakthrough rewards",
          "items": {"type": "string"}
        },
        "spindaPatterns": {
          "type": "array",
          "description": "Currently available Spinda pattern numbers",
          "items": {"type": "integer"}
        },
        "season": {
          "type": ["string", "null"],
          "description": "Current season name"
        }
      }
    },
    "tasks": {
      "type": "array",
      "description": "Array of field research task objects",
      "items": {
        "type": "object",
        "required": ["text", "rewards"],
        "properties": {
          "text": {
            "type": "string",
            "description": "The task description as shown to players"
          },
          "type": {
            "type": "string",
            "description": "Category of task",
            "enum": ["catch", "throw", "battle", "explore", "training", "buddy", "rocket", "sponsored", "ar"]
          },
          "rewards": {
            "type": "array",
            "description": "Array of possible rewards for this task",
            "items": {
              "type": "object",
              "required": ["type"],
              "properties": {
                "type": {
                  "type": "string",
                  "description": "Type of reward",
                  "enum": ["encounter", "item"]
                },
                "name": {
                  "type": "string",
                  "description": "Name of the Pokémon or item (may include quantity for items)"
                },
                "image": {
                  "type": "string",
                  "format": "uri",
                  "description": "URL to reward icon"
                },
                "imageWidth": {
                  "type": "integer",
                  "minimum": 1
                },
                "imageHeight": {
                  "type": "integer",
                  "minimum": 1
                },
                "canBeShiny": {
                  "type": "boolean",
                  "description": "Whether this Pokémon can be shiny (encounters only)"
                },
                "combatPower": {
                  "type": "object",
                  "description": "CP range for encounter rewards",
                  "properties": {
                    "min": {"type": "integer", "minimum": 0},
                    "max": {"type": "integer", "minimum": 0}
                  }
                },
                "quantity": {
                  "type": "integer",
                  "description": "Quantity of item reward",
                  "minimum": 1
                }
              }
            }
          }
        }
      }
    }
  }
}
```

## Property Definitions

### Seasonal Info

| Property | Type | Description |
|----------|------|-------------|
| `breakthroughPokemon` | array | Names of Pokémon available from 7-day Research Breakthrough |
| `spindaPatterns` | array | Pattern numbers for Spinda encounters this period |
| `season` | string/null | Current game season name |

### Task Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `text` | string | Yes | The task requirement text shown to players |
| `type` | string | No | Task category for filtering purposes |
| `rewards` | array | Yes | Array of possible rewards (one is randomly selected) |

### Task Types

- **`catch`** - Catching Pokémon (specific types, quantities, weather boost)
- **`throw`** - Throwing Pokéballs (Nice, Great, Excellent, Curveball)
- **`battle`** - Raids, Trainer Battles, Team GO Rocket battles
- **`explore`** - Walking, spinning PokéStops, hatching eggs
- **`training`** - Powering up, evolving Pokémon, earning XP/Stardust
- **`buddy`** - Buddy-related activities (hearts, treats, walking)
- **`rocket`** - Team GO Rocket specific tasks
- **`sponsored`** - Tasks from sponsored locations
- **`ar`** - AR scanning tasks

### Reward Properties

| Property | Type | Description |
|----------|------|-------------|
| `type` | string | `"encounter"` for Pokémon or `"item"` for items |
| `name` | string | Pokémon name or item name (may include quantity prefix like "×3") |
| `image` | string (URI) | URL to the icon image |
| `imageWidth` | integer | Icon width in pixels |
| `imageHeight` | integer | Icon height in pixels |
| `canBeShiny` | boolean | Whether Pokémon can be shiny (encounters only) |
| `combatPower` | object | CP range (encounters only) |
| `quantity` | integer | Item quantity (items only) |

## Understanding Rewards

### Multiple Reward Possibilities

Most tasks have multiple possible rewards listed. **Only one reward is given** when completing the task - the game randomly selects from the array. This is common for encounter rewards where several Pokémon share the same task.

```javascript
// This task can reward ANY ONE of these Pokémon
{
  "text": "Make 3 Great Throws",
  "rewards": [
    {"type": "encounter", "name": "Omanyte", ...},
    {"type": "encounter", "name": "Kabuto", ...},
    {"type": "encounter", "name": "Clamperl", ...}
  ]
}
```

### Guaranteed vs Random

Some tasks offer item rewards where the exact reward is guaranteed:

```javascript
// You always get these specific items
{
  "text": "Catch 10 Pokémon",
  "rewards": [
    {"type": "item", "name": "×5", "quantity": 5, ...}, // Poké Balls
    {"type": "item", "name": "×200", "quantity": 200, ...} // Stardust
  ]
}
```

## Usage Examples

### Filtering by Task Type

```javascript
// Get all catch-related tasks
const catchTasks = data.tasks.filter(task => task.type === 'catch');

// Get all throw-related tasks
const throwTasks = data.tasks.filter(task => task.type === 'throw');
```

### Finding Specific Pokémon Encounters

```javascript
// Find tasks that reward a specific Pokémon
function findTasksForPokemon(pokemonName) {
  return data.tasks.filter(task =>
    task.rewards.some(reward =>
      reward.type === 'encounter' && 
      reward.name === pokemonName
    )
  );
}

const gibleTasks = findTasksForPokemon('Gible');
```

### Finding Shiny-Eligible Encounters

```javascript
// Get tasks with potential shiny encounters
const shinyTasks = data.tasks.filter(task =>
  task.rewards.some(reward =>
    reward.type === 'encounter' && 
    reward.canBeShiny === true
  )
);
```

### Finding Spinda Tasks

```javascript
// Find current Spinda pattern tasks
const spindaTasks = data.tasks.filter(task =>
  task.rewards.some(reward =>
    reward.name && reward.name.startsWith('Spinda')
  )
);

console.log('Current Spinda patterns:', data.seasonalInfo.spindaPatterns);
```

### Grouping Tasks by Category

```javascript
// Group all tasks by type
const tasksByType = data.tasks.reduce((acc, task) => {
  const type = task.type || 'other';
  if (!acc[type]) acc[type] = [];
  acc[type].push(task);
  return acc;
}, {});
```

### Extracting All Encounter Rewards

```javascript
// Get unique list of all Pokémon available from research
const allPokemon = [...new Set(
  data.tasks.flatMap(task =>
    task.rewards
      .filter(r => r.type === 'encounter')
      .map(r => r.name)
  )
)];
```

## Special Cases

### Reward Arrays

Tasks can have multiple rewards in different patterns:

1. **Multiple possible encounters** (random selection) - Several Pokémon, one is chosen
2. **Encounter + Items** - Mix of Pokémon and items as alternatives
3. **Multiple items** - Stack of different items, all awarded together
4. **Single reward** - Only one option available

### Item Name Format

Item rewards may have quantity in the `name` field (e.g., "×200", "×5") in addition to the `quantity` property. Both should be used for complete information.

### Spinda Patterns

Spinda encounters include pattern numbers in the name (e.g., "Spinda 6", "Spinda 7"). The `seasonalInfo.spindaPatterns` array indicates which patterns are currently available.

### Research Breakthrough

The `breakthroughPokemon` array lists Pokémon available from 7-day streak Research Breakthrough rewards, which are separate from individual task rewards.

## Important Notes

- **Data volatility**: Field Research tasks rotate monthly and change during events. Data represents current availability.
- **Random rewards**: When multiple rewards exist for a task, only one is randomly awarded upon completion.
- **Stacking**: Some tasks appear to list multiple items - these may all be awarded together, or be separate possible outcomes depending on the task.
- **CP values**: Combat Power ranges for encounters are based on trainer level and may vary.
- **Task removal**: Tasks can be discarded and replaced by spinning PokéStops until the desired task is found.

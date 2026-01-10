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
                "shinyAssetUrl": {
                  "type": ["string", "null"],
                  "format": "uri",
                  "description": "URL to the shiny variant icon image (encounters only). null if shiny form is not available or not yet released"
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
| `breakthroughPokemon` | array | Names of Pokémon available from 7-day Research Breakthrough. Names may be abbreviated (e.g., "Galarian Mr" for "Galarian Mr. Mime") |
| `spindaPatterns` | array | Pattern numbers for Spinda encounters this period |
| `season` | string/null | Current game season name. `null` indicates no active seasonal designation or data not currently available |
### Task Properties
| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `text` | string | Yes | The task requirement text shown to players |
| `type` | string | No | Task category for filtering purposes. May be omitted for special event tasks or uncategorized tasks |
| `rewards` | array | Yes | Array of possible rewards (see reward structure details below) |
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
| `name` | string | Pokémon name (including regional forms) or item name (may include quantity prefix like "×3") |
| `image` | string (URI) | URL to the icon image |
| `imageWidth` | integer | Icon width in pixels |
| `imageHeight` | integer | Icon height in pixels |
| `canBeShiny` | boolean | Whether Pokémon can be shiny (encounters only). `false` indicates the shiny variant is not yet released in the game |
| `shinyAssetUrl` | string/null | URL to the shiny variant icon image (encounters only). `null` if shiny form is not available or not yet released |
| `combatPower` | object | CP range for encounters at level 15 (standard research encounter level). Actual CP may vary based on trainer level and weather boost |
| `quantity` | integer | Item quantity (items only) |

### Shiny Encounters from Research
Field Research encounters can yield shiny Pokémon with the following mechanics:

**Shiny Eligibility**: The `canBeShiny` flag indicates whether a specific encounter reward can be shiny:
- `true`: Shiny form is available (standard ~1/500 odds for most species)
- `false`: Shiny form either not released yet or not available through this specific encounter

**Odds and Rates**:
- Most research encounter shinies follow standard wild encounter rates (~1/500)
- Some featured event research may have boosted rates
- Legendary Pokémon from Research Breakthrough historically had higher rates (~1/20)

**Research Breakthrough Shinies**:
The `seasonalInfo.breakthroughPokemon` lists Pokémon available from completing 7 days of research. These encounters:
- Are typically shiny-eligible (check `canBeShiny` in encounter data)
- Often feature legendary or mythical Pokémon
- May rotate monthly or per season
- Have guaranteed minimum IVs (10/10/10)

**Special Cases**:
- **Spinda**: Each pattern number is a separate shiny check. Current patterns listed in `seasonalInfo.spindaPatterns`
- **Legendary Encounters**: Research Breakthrough legendaries historically have boosted shiny rates
- **Event-Exclusive Research**: Special or Timed Research during events may feature boosted shiny rates for specific encounters

**Cross-Reference**: For comprehensive shiny availability and release dates, reference `shiny.min.json` which catalogs all released shiny forms in the game.
## Understanding Rewards
### Reward Structure: Random Selection vs. Guaranteed Bundles
The rewards array can represent different scenarios:
#### Random Selection (ONE Reward from List)
When a task offers multiple Pokémon encounters or a mix of encounters and items, typically **only ONE** reward is randomly selected upon completion.
```javascript
// This task rewards ONE of these options
{
  "text": "Make 3 Great Throws",
  "rewards": [
    {"type": "encounter", "name": "Omanyte", ...},
    {"type": "encounter", "name": "Kabuto", ...},
    {"type": "encounter", "name": "Clamperl", ...},
    {"type": "item", "name": "×200", ...} // Or Stardust instead
  ]
}
```
#### Guaranteed Bundles (ALL Items Awarded)
Some tasks award multiple items together as a bundle. The game logic determines whether rewards are alternatives or bundles.
```javascript
// This task may award ALL of these items together
{
  "text": "Make 3 Great Throws in a row",
  "rewards": [
    {"type": "item", "name": "×10", "quantity": 10, ...}, // Poké Balls
    {"type": "item", "name": "×5", "quantity": 5, ...},   // Ultra Balls
    {"type": "item", "name": "×9", "quantity": 9, ...},   // Razz Berries
    {"type": "item", "name": "×3", "quantity": 3, ...},   // Pinap Berries
    {"type": "item", "name": "×1", "quantity": 1, ...},   // Rare Candy
    {"type": "item", "name": "×1000", "quantity": 1000, ...} // Stardust
  ]
}
```
**Important:** Without game knowledge, it's difficult to determine from the data alone whether rewards are alternatives or bundles. Generally:
- Multiple Pokémon encounters = Random selection (one awarded)
- Mixed encounters and items = Random selection (one awarded)
- Multiple different item types = May be a bundle (check in-game)
### Regional Forms and Variants
Pokémon names may include regional form designations and other variants:
- **Regional Forms**: "Alolan Meowth", "Galarian Slowpoke", "Hisuian Growlithe", "Paldean Wooper"
- **Gender Variants**: "Frillish (Female)"
- **Pattern Variants**: "Spinda 6", "Spinda 7"
```javascript
// Example of regional form variants in same task
{
  "text": "Spin 5 PokéStops or Gyms",
  "rewards": [
    {"type": "encounter", "name": "Growlithe", ...},
    {"type": "encounter", "name": "Hisuian Growlithe", ...},
    {"type": "encounter", "name": "Slowpoke", ...},
    {"type": "encounter", "name": "Galarian Slowpoke", ...}
  ]
}
```
### Special Event Tasks
Some tasks are tied to specific events or dates and may have unique characteristics:
- Tasks without a `type` field are often special event-related
- Reward quantities may reflect event dates (e.g., "×2026" Stardust for New Year 2026)
- Event tasks are temporary and rotate with game events
```javascript
// Special event task example
{
  "text": "Catch 26 Pokémon",
  "rewards": [
    {"type": "item", "name": "×2026", "quantity": 2026, ...} // Event-themed reward
  ]
}
```
### Spinda Patterns
Spinda encounters include pattern numbers in the name (e.g., "Spinda 6", "Spinda 7"). The `seasonalInfo.spindaPatterns` array indicates which patterns are currently available. Spinda patterns rotate periodically.
```javascript
// Finding current Spinda tasks
const spindaTasks = data.tasks.filter(task =>
  task.rewards.some(reward =>
    reward.name && reward.name.startsWith('Spinda')
  )
);
console.log('Current Spinda patterns:', data.seasonalInfo.spindaPatterns);
// Output: Current Spinda patterns: [6, 7]
```
### Research Breakthrough
The `breakthroughPokemon` array lists Pokémon available from 7-day streak Research Breakthrough rewards, which are separate from individual task rewards.
## Usage Examples
### Filtering by Task Type
```javascript
// Get all catch-related tasks
const catchTasks = data.tasks.filter(task => task.type === 'catch');
// Get all throw-related tasks
const throwTasks = data.tasks.filter(task => task.type === 'throw');
// Get tasks without a type (often event-specific)
const specialTasks = data.tasks.filter(task => !task.type);
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
// Find tasks for any regional form
function findTasksForPokemonFamily(baseName) {
  return data.tasks.filter(task =>
    task.rewards.some(reward =>
      reward.type === 'encounter' && 
      reward.name.includes(baseName)
    )
  );
}
const meowthTasks = findTasksForPokemonFamily('Meowth');
// Returns tasks with Meowth, Alolan Meowth, and Galarian Meowth
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
// Get unique shiny-capable Pokémon
const shinyPokemon = [...new Set(
  data.tasks.flatMap(task =>
    task.rewards
      .filter(r => r.type === 'encounter' && r.canBeShiny === true)
      .map(r => r.name)
  )
)].sort();
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
console.log('Spinda task:', spindaTasks[0]?.text);
```
### Grouping Tasks by Category
```javascript
// Group all tasks by type
const tasksByType = data.tasks.reduce((acc, task) => {
  const type = task.type || 'event';
  if (!acc[type]) acc[type] = [];
  acc[type].push(task);
  return acc;
}, {});
console.log('Tasks by category:', Object.keys(tasksByType));
console.log('Number of catch tasks:', tasksByType.catch?.length || 0);
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
)].sort();
console.log(`${allPokemon.length} unique Pokémon available from field research`);
```
### Finding High-Value Rewards
```javascript
// Find tasks offering Rare Candy
const rareCandyTasks = data.tasks.filter(task =>
  task.rewards.some(reward =>
    reward.type === 'item' && 
    reward.name && reward.image.includes('Rare%20Candy')
  )
);
// Find tasks offering rare Pokémon (pseudo-legendary starters)
const rarePokemon = ['Larvitar', 'Beldum', 'Gible', 'Axew', 'Jangmo-o'];
const rarePokemonTasks = data.tasks.filter(task =>
  task.rewards.some(reward =>
    reward.type === 'encounter' && 
    rarePokemon.includes(reward.name)
  )
);
```
### Analyzing Reward Complexity
```javascript
// Find tasks with multiple encounter options
const multiEncounterTasks = data.tasks.filter(task => {
  const encounters = task.rewards.filter(r => r.type === 'encounter');
  return encounters.length > 1;
});
// Find tasks with item bundles
const itemBundleTasks = data.tasks.filter(task => {
  const items = task.rewards.filter(r => r.type === 'item');
  return items.length > 3; // Likely a bundle if 4+ items
});
```
## Important Notes
- **Data volatility**: Field Research tasks rotate monthly and change during events. The data represents current availability and may change without notice.
- **Random rewards**: When multiple rewards exist for a task, the game determines whether one is randomly selected or all are awarded. This logic is not encoded in the data.
- **CP values**: Combat Power ranges represent level 15 encounters (standard research level). Actual CP varies by trainer level and weather conditions.
- **Task removal**: Tasks can be discarded and replaced by spinning PokéStops until the desired task is found.
- **Shiny availability**: `canBeShiny: false` indicates the shiny variant hasn't been released in the game yet, not that you caught it already.
- **Regional forms**: Pay attention to form designations (Alolan, Galarian, Hisuian, Paldean) as they are distinct Pokémon with different stats and types.
- **Event tasks**: Tasks without a `type` field or with unusual reward quantities are often tied to limited-time events.
## Data Quality Considerations
- **Name abbreviations**: Pokémon names in `breakthroughPokemon` may be abbreviated (e.g., "Galarian Mr" instead of "Galarian Mr. Mime")
- **Missing types**: Not all tasks include a `type` field, particularly event-specific tasks
- **Reward interpretation**: Without additional game context, determining whether multiple item rewards are alternatives or bundles requires observation
- **Image URLs**: All image URLs point to external CDN resources which may change independently of this data
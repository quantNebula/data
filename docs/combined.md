# Pokémon GO Data Repository

This repository provides structured JSON data for Pokémon GO game mechanics, updated regularly to reflect current game state.

## Available Data Files

| File | Description |
|------|-------------|
| https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json | Egg pool data including distance tiers, Pokémon availability, and rarity |
| https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json | Event schedules, bonuses, featured Pokémon, and special mechanics |
| https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json | Field research tasks, rewards, and seasonal breakthrough encounters |
| https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json | Team GO Rocket battle lineups for Giovanni, Leaders, and Grunts |

## Quick Start

### Accessing the Data

All data files are available via CDN:

```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/{filename}
```

### Basic Usage

```javascript
// Fetch egg pool data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json')
  .then(response => response.json())
  .then(data => {
    console.log('Egg data loaded:', data);
  });

// Fetch current events
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json')
  .then(response => response.json())
  .then(events => {
    const activeEvents = events.filter(e => 
      new Date(e.start) <= new Date() && new Date(e.end) >= new Date()
    );
    console.log('Active events:', activeEvents);
  });

// Fetch field research tasks
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json')
  .then(response => response.json())
  .then(data => {
    console.log('Current research season:', data.seasonalInfo.season);
    console.log('Available tasks:', data.tasks.length);
  });

// Fetch Team Rocket lineups
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json')
  .then(response => response.json())
  .then(lineups => {
    const giovanni = lineups.find(l => l.name === 'Giovanni');
    console.log('Giovanni lineup:', giovanni);
  });
```

### Parallel Data Loading

Load multiple data sources simultaneously for better performance:

```javascript
async function loadAllData() {
  const [eggs, events, research, rocketLineups] = await Promise.all([
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json').then(r => r.json()),
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json').then(r => r.json()),
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/research.min.json').then(r => r.json()),
    fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json').then(r => r.json())
  ]);

  return { eggs, events, research, rocketLineups };
}

loadAllData().then(data => {
  console.log('All data loaded:', data);
});
```

## Data Structure Overview

### Eggs Data Structure
```json
{
  "1km": [...],
  "2km": [...],
  "5km": [...],
  "7km": [...],
  "10km": [...],
  "12km": [...],
  "adventureSync5km": [...],
  "adventureSync10km": [...]
}
```

### Events Data Structure
```json
[
  {
    "eventID": "string",
    "name": "string",
    "eventType": "string",
    "start": "ISO 8601 date-time",
    "end": "ISO 8601 date-time",
    "timezone": "string|null",
    "extraData": {...}
  }
]
```

### Research Data Structure
```json
{
  "seasonalInfo": {
    "breakthroughPokemon": [...],
    "spindaPatterns": [...],
    "season": "string"
  },
  "tasks": [...]
}
```

### Rocket Lineups Data Structure
```json
[
  {
    "name": "string",
    "title": "string",
    "type": "string",
    "firstPokemon": [...],
    "secondPokemon": [...],
    "thirdPokemon": [...]
  }
]
```

## Common Use Cases

### Finding Active Events

```javascript
function getActiveEvents(events) {
  const now = new Date();
  return events.filter(event => {
    const start = new Date(event.start);
    const end = new Date(event.end);
    return now >= start && now <= end;
  });
}
```

### Checking Egg Pool for Specific Pokémon

```javascript
function findPokemonInEggs(eggData, pokemonName) {
  const results = [];
  for (const [distance, pokemon] of Object.entries(eggData)) {
    const found = pokemon.find(p => p.name === pokemonName);
    if (found) {
      results.push({ distance, ...found });
    }
  }
  return results;
}
```

### Finding Research Tasks by Type

```javascript
function getTasksByType(researchData, taskType) {
  return researchData.tasks.filter(task => task.type === taskType);
}
```

### Checking Catchable Shadow Pokémon

```javascript
function getCatchableShadowPokemon(rocketLineups) {
  return rocketLineups.flatMap(lineup => 
    lineup.firstPokemon
      .filter(p => p.canBeCaught)
      .map(p => ({ pokemon: p.name, leader: lineup.name }))
  );
}
```

## Data Schemas

Each data file has comprehensive JSON Schema documentation available in the `docs/` directory:

- **[Eggs Schema Documentation](docs/eggs-schema.md)** - Detailed egg pool structure, rarity tiers, and form variations
- **[Events Schema Documentation](docs/events-schema.md)** - Event types, timezone handling, and extraData structures
- **[Research Schema Documentation](docs/research-schema.md)** - Task types, reward structures, and seasonal information
- **[Rocket Lineups Schema Documentation](docs/rocketLineups-schema.md)** - Lineup structure, encounter mechanics, and type specializations

All schemas follow JSON Schema Draft 07 specification and include:
- Complete property definitions
- Type validations and constraints
- Comprehensive usage examples
- Special cases and edge conditions

## Important Notes

⚠️ **Data Volatility**: All data files are updated frequently to reflect current Pokémon GO game state. Do not cache data indefinitely or rely on specific Pokémon availability remaining constant.

🕐 **Timezone Considerations**: Events may use different timezone specifications:
- `"Local Time"` - Times are in the player's local timezone
- `null` - Times are in UTC/GMT
- Specific timezone strings - Times are in the specified timezone

🔄 **Update Frequency**: 
- Events data updates multiple times per day
- Egg pools change with events and seasons
- Research tasks rotate regularly
- Rocket lineups change periodically

📊 **Data Format**: All files use minified JSON format (`.min.json`) for efficient delivery. The data is complete despite being minified.

## Error Handling

Always implement proper error handling when fetching data:

```javascript
async function fetchWithErrorHandling(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch data:', error);
    // Implement fallback logic or retry mechanism
    return null;
  }
}
```

## CORS Support

The jsDelivr CDN supports CORS, allowing you to fetch data from any domain in browser environments.

## Rate Limiting

jsDelivr CDN has generous rate limits suitable for most applications. For high-traffic applications, consider implementing:
- Local caching with reasonable TTL (time-to-live)
- Request throttling
- Fallback mechanisms

## Contributing

This is a data repository. For issues or corrections, please contact the maintainer.

## License

Data is provided for informational and educational purposes. Pokémon and Pokémon GO are trademarks of Nintendo, Creatures Inc., and GAME FREAK Inc.

## Support

For detailed information about specific data structures, refer to the individual schema documentation files in the `docs/` directory.

---

**Last Updated**: Automatically updated with each data refresh  
**Repository**: https://github.com/quantNebula/data

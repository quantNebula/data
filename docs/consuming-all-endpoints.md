# Consuming All Pokémon GO Data Endpoints

## Overview
This guide explains how to consume all available data endpoints simultaneously and understand the relationships between different data sources. By combining these endpoints, you can build comprehensive applications that provide a complete view of the current Pokémon GO game state.

> **Note:** All data is available as **both individual endpoint files AND a combined aggregated file**. The `combined.min.json` file is automatically generated from the individual files and validated to ensure all data sources are included. See [Combined File Architecture](combined-architecture.md) for technical details.

## Available Endpoints

All data is available via CDN at `https://cdn.jsdelivr.net/gh/quantNebula/data@master/{filename}`:

| Endpoint | Description | Update Frequency |
|----------|-------------|------------------|
| `events.min.json` | Game events, schedules, and event-specific features | Frequently (daily/weekly) |
| `raids.min.json` | Current raid bosses across all tiers | Event-based (weekly/bi-weekly) |
| `eggs.min.json` | Pokémon available from eggs | Event-based (weekly/monthly) |
| `research.min.json` | Field research tasks and rewards | Monthly + event updates |
| `shiny.min.json` | Shiny Pokémon availability and release dates | As new shinies are released |
| `rocketLineups.min.json` | Team GO Rocket leader lineups | Monthly updates |
| `combined.min.json` | **All endpoints combined into one validated object** | Real-time (same as individual files) |

## Two Ways to Consume the Data

You can consume the data in two ways:
1. **Combined Endpoint** - Single file with all data (recommended for most use cases)
2. **Individual Endpoints** - Separate files for each data type (better for targeted updates)

### Option 1: Using the Combined Endpoint (Recommended)

The easiest and most reliable way to consume all data simultaneously is using the `combined.min.json` endpoint. This file is **automatically generated from the individual files** and **validated** to ensure all data sources are present.

```javascript
// Fetch all data at once
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
  .then(response => response.json())
  .then(data => {
    console.log('Events:', data.events.length);
    console.log('Raids:', data.raids.length);
    console.log('Eggs:', data.eggs.length);
    console.log('Research tasks:', data.research.tasks.length);
    console.log('Shiny Pokémon:', data.shiny.length);
    console.log('Rocket lineups:', data.rocketLineups.length);
  });
```

### Guaranteed Structure

The `combined.min.json` file **always includes all required data sources**:

```json
{
  "events": [...],           // Always present
  "raids": [...],            // Always present
  "eggs": [...],             // Always present
  "research": {              // Always present
    "seasonalInfo": {...},
    "tasks": [...]
  },
  "shiny": [...],           // Always present
  "rocketLineups": [...]    // Always present
}
```

### Benefits of Using Combined Endpoint

✅ **Single Request** - One fetch instead of six  
✅ **Validated** - Automatically validated during generation  
✅ **Comprehensive** - Guaranteed to include all data sources  
✅ **Atomic** - All data from the same generation cycle  
✅ **Efficient** - Reduced bandwidth and latency  

### Option 2: Fetching Individual Endpoints

For more granular control, when you only need specific endpoints, or when you want to update specific data independently, use the individual endpoint files.

**Use individual endpoints when:**
- You only need specific data (e.g., just events)
- You want to cache different endpoints separately
- You're implementing selective updates
- You want finer control over data freshness

**Fetch all individual endpoints in parallel:**

```javascript
async function fetchAllData() {
  const baseUrl = 'https://cdn.jsdelivr.net/gh/quantNebula/data@master';
  
  const [events, raids, eggs, research, shiny, rocketLineups] = await Promise.all([
    fetch(`${baseUrl}/events.min.json`).then(r => r.json()),
    fetch(`${baseUrl}/raids.min.json`).then(r => r.json()),
    fetch(`${baseUrl}/eggs.min.json`).then(r => r.json()),
    fetch(`${baseUrl}/research.min.json`).then(r => r.json()),
    fetch(`${baseUrl}/shiny.min.json`).then(r => r.json()),
    fetch(`${baseUrl}/rocketLineups.min.json`).then(r => r.json())
  ]);
  
  return { events, raids, eggs, research, shiny, rocketLineups };
}

fetchAllData().then(data => {
  console.log('All data loaded:', data);
});
```

## Data Relationships and Cross-Referencing

Understanding how different endpoints relate to each other is key to building comprehensive applications:

### 1. Events → Other Endpoints

Events often influence what appears in raids, eggs, and research. Current events can:
- Change raid bosses
- Modify egg pools
- Add special field research tasks
- Increase shiny encounter rates

**Example: Finding event-influenced content**
```javascript
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
  .then(response => response.json())
  .then(data => {
    // Get current events
    const now = new Date();
    const currentEvents = data.events.filter(event => {
      const start = new Date(event.start);
      const end = new Date(event.end);
      return start <= now && end >= now && event.isCurrent;
    });
    
    console.log('Current events:', currentEvents.map(e => e.name));
    
    // Events may reference Pokémon that appear in raids, research, etc.
    // Check event extraData for specific spawn/raid/research information
    currentEvents.forEach(event => {
      if (event.extraData?.event) {
        console.log(`${event.name} features:`, event.extraData.event.features);
        console.log(`${event.name} bonuses:`, event.extraData.event.bonuses);
      }
    });
  });
```

### 2. Pokémon Name Cross-Reference

Many endpoints reference Pokémon by name, allowing you to cross-reference:
- **Events** → List Pokémon in spawn pools (in extraData)
- **Raids** → Pokémon available as raid bosses
- **Eggs** → Pokémon available from hatching
- **Research** → Pokémon available as encounter rewards
- **Shiny** → Pokémon with shiny forms available
- **Rocket Lineups** → Pokémon used by Team GO Rocket

**Example: Finding all ways to obtain a specific Pokémon**
```javascript
async function findPokemonAvailability(pokemonName) {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  const availability = {
    inRaids: data.raids.find(r => r.name === pokemonName),
    inEggs: data.eggs.find(e => e.name === pokemonName),
    inResearch: data.research.tasks.filter(t => 
      t.rewards.some(reward => 
        reward.type === 'encounter' && reward.name === pokemonName
      )
    ),
    hasShiny: data.shiny.find(s => s.name === pokemonName),
    inRocketLineups: data.rocketLineups.filter(leader =>
      leader.firstPokemon.some(p => p.name === pokemonName) ||
      leader.secondPokemon.some(p => p.name === pokemonName) ||
      leader.thirdPokemon.some(p => p.name === pokemonName)
    )
  };
  
  return availability;
}

// Example usage
findPokemonAvailability('Corphish').then(availability => {
  console.log('Corphish availability:', availability);
  
  if (availability.inRaids) {
    console.log(`- Available in ${availability.inRaids.tier}`);
  }
  if (availability.inEggs) {
    console.log(`- Hatches from ${availability.inEggs.eggType} eggs`);
  }
  if (availability.inResearch.length > 0) {
    console.log(`- Reward from ${availability.inResearch.length} research task(s)`);
  }
  if (availability.hasShiny) {
    console.log('- Shiny form available!');
  }
});
```

### 3. Shiny Availability Cross-Reference

The `canBeShiny` field appears across multiple endpoints:

**Example: Finding all current shiny-eligible Pokémon**
```javascript
async function getAllShinyOpportunities() {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  const shinyOpportunities = new Map();
  
  // From raids
  data.raids.filter(r => r.canBeShiny).forEach(raid => {
    shinyOpportunities.set(raid.name, {
      ...shinyOpportunities.get(raid.name),
      raids: true,
      tier: raid.tier
    });
  });
  
  // From eggs
  data.eggs.filter(e => e.canBeShiny).forEach(egg => {
    shinyOpportunities.set(egg.name, {
      ...shinyOpportunities.get(egg.name),
      eggs: true,
      eggType: egg.eggType
    });
  });
  
  // From research
  data.research.tasks.forEach(task => {
    task.rewards
      .filter(r => r.type === 'encounter' && r.canBeShiny)
      .forEach(reward => {
        shinyOpportunities.set(reward.name, {
          ...shinyOpportunities.get(reward.name),
          research: true
        });
      });
  });
  
  return shinyOpportunities;
}

getAllShinyOpportunities().then(opportunities => {
  console.log('Shiny opportunities:', opportunities);
});
```

### 4. Event Timeline Relationships

Events often overlap or occur in succession, and understanding the timeline helps:

**Example: Building an event calendar**
```javascript
async function buildEventCalendar() {
  const events = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json')
    .then(r => r.json());
  
  // Sort by start date
  const sortedEvents = events.sort((a, b) => 
    new Date(a.start) - new Date(b.start)
  );
  
  // Group by status
  const now = new Date();
  const calendar = {
    past: [],
    current: [],
    upcoming: []
  };
  
  sortedEvents.forEach(event => {
    const start = new Date(event.start);
    const end = new Date(event.end);
    
    if (end < now) {
      calendar.past.push(event);
    } else if (start <= now && end >= now) {
      calendar.current.push(event);
    } else {
      calendar.upcoming.push(event);
    }
  });
  
  return calendar;
}

buildEventCalendar().then(calendar => {
  console.log('Current events:', calendar.current.length);
  console.log('Upcoming events:', calendar.upcoming.length);
  
  // Find overlapping events
  calendar.current.forEach((event, i) => {
    calendar.current.slice(i + 1).forEach(otherEvent => {
      console.log(`${event.name} overlaps with ${otherEvent.name}`);
    });
  });
});
```

### 5. Combat Power (CP) Relationships

CP values appear in eggs, raids, and research rewards, useful for planning:

**Example: Finding high-CP encounter opportunities**
```javascript
async function findHighCPEncounters(minCP = 1000) {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  const highCPSources = [];
  
  // From raids (normal CP)
  data.raids
    .filter(r => r.combatPower.normal.max >= minCP)
    .forEach(raid => {
      highCPSources.push({
        name: raid.name,
        source: 'raid',
        tier: raid.tier,
        cpRange: `${raid.combatPower.normal.min}-${raid.combatPower.normal.max}`,
        boostedRange: `${raid.combatPower.boosted.min}-${raid.combatPower.boosted.max}`
      });
    });
  
  // From research
  data.research.tasks.forEach(task => {
    task.rewards
      .filter(r => r.type === 'encounter' && r.combatPower?.max >= minCP)
      .forEach(reward => {
        highCPSources.push({
          name: reward.name,
          source: 'research',
          task: task.text,
          cpRange: `${reward.combatPower.min}-${reward.combatPower.max}`
        });
      });
  });
  
  return highCPSources;
}

findHighCPEncounters(1500).then(sources => {
  console.log('High CP encounter opportunities:', sources);
});
```

## Practical Use Cases

### Use Case 1: Event Dashboard

Display current events with related content:

```javascript
async function buildEventDashboard() {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  const currentEvents = data.events.filter(e => e.isCurrent);
  
  return currentEvents.map(event => ({
    event: event,
    relatedRaids: data.raids.filter(r => 
      event.extraData?.event?.customSections?.raids
    ),
    relatedResearch: data.research.tasks.filter(t =>
      // Research tasks related to the event
      t.rewards.some(reward => 
        event.name.toLowerCase().includes(reward.name?.toLowerCase())
      )
    )
  }));
}
```

### Use Case 2: Pokédex Completion Tracker

Track progress toward completing your Pokédex:

```javascript
async function buildCompletionTracker(caughtPokemon) {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  // Get all available Pokémon
  const available = new Set();
  
  data.raids.forEach(r => available.add(r.name));
  data.eggs.forEach(e => available.add(e.name));
  data.research.tasks.forEach(t => {
    t.rewards
      .filter(r => r.type === 'encounter')
      .forEach(r => available.add(r.name));
  });
  
  // Find missing Pokémon
  const missing = Array.from(available).filter(name => 
    !caughtPokemon.includes(name)
  );
  
  // Find ways to obtain missing Pokémon
  return missing.map(name => ({
    name,
    sources: {
      raid: data.raids.find(r => r.name === name),
      egg: data.eggs.find(e => e.name === name),
      research: data.research.tasks.filter(t =>
        t.rewards.some(r => r.type === 'encounter' && r.name === name)
      )
    }
  }));
}
```

### Use Case 3: Raid Battle Planner

Plan raid battles with type effectiveness:

```javascript
async function planRaidBattles() {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  // Group raids by tier
  const raidsByTier = {};
  data.raids.forEach(raid => {
    if (!raidsByTier[raid.tier]) {
      raidsByTier[raid.tier] = [];
    }
    raidsByTier[raid.tier].push({
      name: raid.name,
      types: raid.types,
      cpRange: raid.combatPower.normal,
      canBeShiny: raid.canBeShiny,
      boostedWeather: raid.boostedWeather
    });
  });
  
  return raidsByTier;
}
```

### Use Case 4: Shiny Hunter's Guide

Find all opportunities to hunt for shinies:

```javascript
async function buildShinyHuntersGuide() {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  // Get shiny-eligible Pokémon
  const shinyPokemon = data.shiny.map(s => s.name);
  
  // Find current opportunities
  const opportunities = shinyPokemon.map(name => {
    const sources = [];
    
    const raid = data.raids.find(r => r.name === name && r.canBeShiny);
    if (raid) sources.push({ type: 'raid', tier: raid.tier });
    
    const egg = data.eggs.find(e => e.name === name && e.canBeShiny);
    if (egg) sources.push({ type: 'egg', eggType: egg.eggType });
    
    const research = data.research.tasks.filter(t =>
      t.rewards.some(r => 
        r.type === 'encounter' && r.name === name && r.canBeShiny
      )
    );
    if (research.length > 0) sources.push({ type: 'research', count: research.length });
    
    return { name, sources };
  }).filter(p => p.sources.length > 0);
  
  return opportunities;
}
```

### Use Case 5: Team GO Rocket Battle Prep

Prepare for Team GO Rocket battles:

```javascript
async function prepareForRocketBattles() {
  const data = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
    .then(r => r.json());
  
  return data.rocketLineups.map(leader => ({
    name: leader.name,
    title: leader.title,
    possibleTeams: [
      // First slot (always the same)
      leader.firstPokemon.map(p => ({
        name: p.name,
        types: p.types,
        isEncounter: p.isEncounter
      })),
      // Second slot (random selection)
      leader.secondPokemon.map(p => ({
        name: p.name,
        types: p.types,
        isEncounter: p.isEncounter
      })),
      // Third slot (random selection, may be the reward)
      leader.thirdPokemon.map(p => ({
        name: p.name,
        types: p.types,
        isEncounter: p.isEncounter,
        canBeShiny: p.canBeShiny
      }))
    ]
  }));
}
```

## Data Freshness and Caching

The data is updated frequently as game events change. Recommended caching strategies:

```javascript
class PoGoDataCache {
  constructor(cacheDuration = 3600000) { // 1 hour default
    this.cacheDuration = cacheDuration;
    this.cache = null;
    this.lastFetch = null;
  }
  
  async getData() {
    const now = Date.now();
    
    // Return cached data if still fresh
    if (this.cache && this.lastFetch && (now - this.lastFetch) < this.cacheDuration) {
      return this.cache;
    }
    
    // Fetch fresh data
    this.cache = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
      .then(r => r.json());
    this.lastFetch = now;
    
    return this.cache;
  }
  
  invalidateCache() {
    this.cache = null;
    this.lastFetch = null;
  }
}

// Usage
const dataCache = new PoGoDataCache(3600000); // 1 hour cache

// First call fetches data
dataCache.getData().then(data => console.log('Data loaded'));

// Subsequent calls within 1 hour use cached data
dataCache.getData().then(data => console.log('Cached data'));

// Force refresh
dataCache.invalidateCache();
dataCache.getData().then(data => console.log('Fresh data'));
```

## Error Handling

Always implement proper error handling when fetching data:

```javascript
async function fetchDataSafely() {
  try {
    const response = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json');
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    const data = await response.json();
    return { success: true, data };
  } catch (error) {
    console.error('Error fetching data:', error);
    return { success: false, error: error.message };
  }
}

fetchDataSafely().then(result => {
  if (result.success) {
    console.log('Data loaded successfully');
  } else {
    console.error('Failed to load data:', result.error);
    // Fallback to cached data or show error message
  }
});
```

## TypeScript Type Definitions

For TypeScript projects, consider defining types based on the schemas documented in individual endpoint documentation files:

```typescript
interface CombinedData {
  events: Event[];
  raids: Raid[];
  eggs: Egg[];
  research: Research;
  shiny: Shiny[];
  rocketLineups: RocketLeader[];
}

interface Event {
  eventID: string;
  name: string;
  eventType: string;
  heading: string;
  image: string;
  imageWidth: number;
  imageHeight: number;
  start: string;
  end: string;
  timezone: string;
  status?: string;
  isCurrent?: boolean;
  extraData?: any;
}

// Define other interfaces based on individual endpoint documentation
// See eggs.md, raids.md, research.md, etc. for detailed schemas
```

## Performance Considerations

1. **Use combined.min.json when possible**: Reduces HTTP requests from 6 to 1
2. **Implement caching**: Data doesn't change more than once per hour typically
3. **Filter data client-side**: All endpoints return complete datasets; filter for your needs
4. **Consider lazy loading**: Load individual endpoints only when needed
5. **Use HTTP/2**: Most modern browsers support it for better performance

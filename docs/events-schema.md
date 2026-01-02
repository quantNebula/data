# Pokémon GO Events Data Schema

## Overview

The `events.min.json` file contains comprehensive data about ongoing and upcoming events in Pokémon GO. This dataset describes event schedules, bonuses, featured Pokémon, raids, research tasks, and special mechanics.

**Note:** Events are time-sensitive and constantly updated. This documentation focuses on the data structure rather than specific current events.

## Data Access

The data is available via CDN:

```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json
```

### Usage Example

```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json')
  .then(response => response.json())
  .then(events => {
    console.log(`Loaded ${events.length} events`);
    const currentEvents = events.filter(e => 
      new Date(e.start) <= new Date() && new Date(e.end) >= new Date()
    );
  });
```

## File Structure

The file contains a JSON array of event objects, each with core properties and optional `extraData` specific to the event type.

```json
[
  {
    "eventID": "unique-event-id",
    "name": "Event Name",
    "eventType": "event",
    "heading": "Event",
    "image": "https://...",
    "imageWidth": 1280,
    "imageHeight": 720,
    "start": "2026-01-06T10:00:00.000",
    "end": "2026-01-11T20:00:00.000",
    "timezone": "Local Time",
    "extraData": {...}
  }
]
```

## JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Events Data Schema",
  "description": "Schema for Pokémon GO event data",
  "type": "array",
  "items": {
    "type": "object",
    "required": ["eventID", "name", "eventType", "heading", "image", "start", "end"],
    "properties": {
      "eventID": {
        "type": "string",
        "description": "Unique identifier for the event"
      },
      "name": {
        "type": "string",
        "description": "Display name of the event"
      },
      "eventType": {
        "type": "string",
        "description": "Category of event",
        "enum": [
          "event",
          "community-day",
          "raid-battles",
          "raid-day",
          "raid-hour",
          "pokemon-spotlight-hour",
          "go-battle-league",
          "max-mondays",
          "max-battles",
          "team-go-rocket",
          "pokemon-go-tour",
          "season",
          "go-pass",
          "research"
        ]
      },
      "heading": {
        "type": "string",
        "description": "Category label for display"
      },
      "image": {
        "type": "string",
        "format": "uri",
        "description": "Event banner image URL"
      },
      "imageWidth": {
        "type": "integer",
        "minimum": 1
      },
      "imageHeight": {
        "type": "integer",
        "minimum": 1
      },
      "start": {
        "type": "string",
        "format": "date-time",
        "description": "Event start date/time in ISO 8601 format"
      },
      "end": {
        "type": "string",
        "format": "date-time",
        "description": "Event end date/time in ISO 8601 format"
      },
      "timezone": {
        "type": ["string", "null"],
        "description": "Timezone specification ('Local Time', null for UTC, or specific timezone)"
      },
      "extraData": {
        "type": ["object", "null"],
        "description": "Event-type specific data structure"
      }
    }
  }
}
```

## Property Definitions

### Core Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `eventID` | string | Yes | Unique identifier (kebab-case, often includes date) |
| `name` | string | Yes | Display name of the event |
| `eventType` | string | Yes | Event category for filtering and display |
| `heading` | string | Yes | Category label shown in UI |
| `image` | string (URI) | Yes | Event banner/promotional image URL |
| `imageWidth` | integer | Yes | Banner image width in pixels |
| `imageHeight` | integer | Yes | Banner image height in pixels |
| `start` | string (ISO 8601) | Yes | Event start timestamp |
| `end` | string (ISO 8601) | Yes | Event end timestamp |
| `timezone` | string/null | No | Timezone context for start/end times |
| `extraData` | object/null | No | Event-specific detailed information |

### Event Types

- **`event`** - General in-game events with spawns, bonuses, and features
- **`community-day`** - Monthly Community Day events
- **`raid-battles`** - Specific Pokémon appearing in raids
- **`raid-day`** - Special raid event days
- **`raid-hour`** - Weekly raid hour events
- **`pokemon-spotlight-hour`** - Weekly spotlight hour featuring one Pokémon
- **`go-battle-league`** - PvP season/league rotation information
- **`max-mondays`** - Weekly Max Battle featured Pokémon
- **`max-battles`** - Dynamax/Gigantamax battle events
- **`team-go-rocket`** - Team GO Rocket takeover events
- **`pokemon-go-tour`** - Large-scale tour events (in-person and global)
- **`season`** - Seasonal game-wide changes
- **`go-pass`** - GO Pass progression track period
- **`research`** - Special or Masterwork Research availability

### Timezone Handling

The `timezone` field indicates how to interpret start/end times:

- **`"Local Time"`** - Times are in the player's local timezone
- **`null`** - Times are in UTC/GMT (often used for global simultaneous starts)
- **Specific timezone string** - Times are in the specified timezone

```javascript
// Check if event is currently active (accounting for local time)
function isEventActive(event) {
  const now = new Date();
  const start = new Date(event.start);
  const end = new Date(event.end);
  
  if (event.timezone === "Local Time") {
    // Times are already in local timezone
    return now >= start && now <= end;
  }
  // Handle UTC or specific timezones
  return now >= start && now <= end;
}
```

## Extra Data Structures

The `extraData` object varies by event type. Common structures include:

### Community Day Extra Data

```json
"extraData": {
  "communityday": {
    "spawns": [...],
    "bonuses": [...],
    "shinies": [...],
    "specialresearch": [...],
    "featuredAttack": "...",
    "ticketedResearch": {...}
  }
}
```

### General Event Extra Data

```json
"extraData": {
  "event": {
    "bonuses": [...],
    "features": [...],
    "shinies": [...],
    "customSections": {
      "spawns": {...},
      "raids": {...},
      "research": {...}
    }
  }
}
```

### Raid Battles Extra Data

```json
"extraData": {
  "raidbattles": {
    "bosses": [...],
    "shinies": [...],
    "tiers": {...},
    "featuredAttacks": [...]
  }
}
```

## Usage Examples

### Finding Current Events

```javascript
// Get all currently active events
function getCurrentEvents(events) {
  const now = new Date();
  return events.filter(event => {
    const start = new Date(event.start);
    const end = new Date(event.end);
    return now >= start && now <= end;
  });
}

const currentEvents = getCurrentEvents(eventsData);
```

### Filtering by Event Type

```javascript
// Get all Community Days
const communityDays = events.filter(e => e.eventType === 'community-day');

// Get all active raid bosses
const raidEvents = events.filter(e => 
  e.eventType === 'raid-battles' &&
  new Date(e.start) <= new Date() &&
  new Date(e.end) >= new Date()
);
```

### Finding Upcoming Events

```javascript
// Get events starting in the next 7 days
const nextWeek = new Date();
nextWeek.setDate(nextWeek.getDate() + 7);

const upcomingEvents = events.filter(event => {
  const start = new Date(event.start);
  return start > new Date() && start <= nextWeek;
}).sort((a, b) => new Date(a.start) - new Date(b.start));
```

### Extracting Event Bonuses

```javascript
// Get all active bonuses from general events
const activeBonuses = events
  .filter(e => e.eventType === 'event' && isEventActive(e))
  .flatMap(e => e.extraData?.event?.bonuses || []);
```

### Finding Spotlight Hour Schedule

```javascript
// Get upcoming Spotlight Hours
const spotlightHours = events
  .filter(e => e.eventType === 'pokemon-spotlight-hour')
  .map(e => ({
    date: new Date(e.start),
    pokemon: e.extraData?.spotlight?.name,
    bonus: e.extraData?.spotlight?.bonus
  }))
  .sort((a, b) => a.date - b.date);
```

### Building an Event Calendar

```javascript
// Group events by date
function groupEventsByDate(events) {
  return events.reduce((acc, event) => {
    const date = new Date(event.start).toISOString().split('T')[0];
    if (!acc[date]) acc[date] = [];
    acc[date].push(event);
    return acc;
  }, {});
}

const calendar = groupEventsByDate(events);
```

### Finding GO Battle League Rotation

```javascript
// Get current and next GBL rotation
const gblEvents = events
  .filter(e => e.eventType === 'go-battle-league')
  .sort((a, b) => new Date(a.start) - new Date(b.start));

const currentGBL = gblEvents.find(e => isEventActive(e));
const nextGBL = gblEvents.find(e => new Date(e.start) > new Date());
```

### Extracting Shiny Debuts

```javascript
// Find all Pokémon getting shiny forms
const shinyDebuts = events
  .flatMap(e => e.extraData?.event?.shinies || [])
  .map(s => s.name)
  .filter((name, index, arr) => arr.indexOf(name) === index);
```

## Important Notes

- **Data volatility**: Events are constantly added, updated, and removed. Always check timestamps for currency.
- **Overlapping events**: Multiple events often run simultaneously (e.g., a general event + raid boss + Spotlight Hour).
- **Time zones**: Pay careful attention to the `timezone` field - "Local Time" means times vary by player location.
- **ExtraData variation**: The `extraData` structure varies significantly by event type. Always check for null/undefined.
- **Past events**: The data may include recently completed events for historical reference.
- **Nested data**: Event details like spawns, raids, and research are often deeply nested in `extraData.event.customSections`.
- **Event duration**: Events range from 1-hour (Raid Hour) to multi-month (Season) in length.

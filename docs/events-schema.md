# Pokémon GO Events Data Schema
## Overview
The `events.min.json` file contains comprehensive data about ongoing and upcoming events in Pokémon GO. This dataset serves as a complete event calendar for the game, covering everything from weekly recurring features to large-scale multi-day events.
**Important:** This data represents events in various states of detail:
- **Placeholder events** - Future events with basic information and default images
- **Fully detailed events** - Events with custom images and complete information
- **Past events** - Recently completed events retained for historical reference

**Critical Note:** The `extraData` field is designed to contain comprehensive event details. Many events have `extraData: null` when they are placeholder entries or when detailed information is not yet available. Fully detailed events typically have `extraData` populated with event-specific information such as bonuses, spawns, featured Pokémon, and more.

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
    
    // Filter to current events
    const now = new Date();
    const currentEvents = events.filter(e => {
      const start = new Date(e.start);
      const end = new Date(e.end);
      return start <= now && end >= now;
    });
    
    console.log(`${currentEvents.length} events active now`);
  });
```
## File Structure
The file contains a JSON array of event objects. Each event has core properties that describe its schedule, category, and visual representation.
```json
[
  {
    "eventID": "january-communityday2026",
    "name": "Grookey Community Day",
    "eventType": "community-day",
    "heading": "Community Day",
    "image": "https://cdn.leekduck.com/assets/img/events/.../image.jpg",
    "imageWidth": 1206,
    "imageHeight": 679,
    "start": "2026-01-18T14:00:00.000Z",
    "end": "2026-01-18T17:00:00.000Z",
    "timezone": "Local Time",
    "extraData": null
  }
]
```
## JSON Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Events Data Schema",
  "description": "Schema for Pokémon GO event calendar data",
  "type": "array",
  "items": {
    "type": "object",
    "required": [
      "eventID",
      "name",
      "eventType",
      "heading",
      "image",
      "imageWidth",
      "imageHeight",
      "start",
      "end",
      "timezone",
      "extraData"
    ],
    "properties": {
      "eventID": {
        "type": "string",
        "description": "Unique identifier for the event",
        "pattern": "^[a-z0-9-_]+$"
      },
      "name": {
        "type": "string",
        "description": "Display name of the event",
        "minLength": 1
      },
      "eventType": {
        "type": "string",
        "description": "Event category identifier",
        "minLength": 1
      },
      "heading": {
        "type": "string",
        "description": "Human-readable category label for display",
        "minLength": 1
      },
      "image": {
        "type": "string",
        "format": "uri",
        "description": "Event banner/promotional image URL"
      },
      "imageWidth": {
        "type": "integer",
        "minimum": 1,
        "description": "Banner image width in pixels"
      },
      "imageHeight": {
        "type": "integer",
        "minimum": 1,
        "description": "Banner image height in pixels"
      },
      "start": {
        "type": "string",
        "pattern": "^\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}\\.\\d{3}Z$",
        "description": "Event start timestamp in ISO 8601 format with Z (UTC) indicator"
      },
      "end": {
        "type": "string",
        "pattern": "^\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}\\.\\d{3}Z$",
        "description": "Event end timestamp in ISO 8601 format with Z (UTC) indicator"
      },
      "timezone": {
        "type": ["string", "null"],
        "description": "Timezone context for interpreting timestamps"
      },
      "extraData": {
        "type": ["object", "null"],
        "description": "Event-specific detailed data (frequently null)"
      }
    }
  }
}
```
## Property Definitions
### Core Properties
| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `eventID` | string | Yes | Unique identifier using consistent naming patterns based on event type |
| `name` | string | Yes | Display name shown to players, may include Pokémon names and forms |
| `eventType` | string | Yes | Event category identifier (e.g., "community-day", "event", "raid-battles") |
| `heading` | string | Yes | Display-friendly category label |
| `image` | string (URI) | Yes | Banner image URL - default images indicate placeholder status |
| `imageWidth` | integer | Yes | Banner width in pixels |
| `imageHeight` | integer | Yes | Banner height in pixels |
| `start` | string | Yes | ISO 8601 timestamp with Z (UTC) indicator |
| `end` | string | Yes | ISO 8601 timestamp with Z (UTC) indicator |
| `timezone` | string/null | Yes | Determines how to interpret timestamps |
| `extraData` | object/null | Yes | Detailed event information (frequently null for future events) |
## Event ID Patterns
The `eventID` field follows consistent naming conventions based on event type:
| Pattern | Example | Event Type |
|---------|---------|-----------|
| `{month-name}-{type}{YYYY}` | `january-communityday2026` | Monthly features |
| `{type}{YYYYMMDD}` | `raidhour20260107` | Weekly recurring events |
| `descriptive-name-date` | `kyurem-fusion-raid-day` | Special one-time events |
| `gbl-{season}_{leagues}` | `gbl-precious-paths_great-league_ultra-league_master-league-split-2` | GO Battle League rotations |
| `{pokemon}-in-{location}-{month}-{year}` | `blacephalon-in-raids-january-2026` | Raid rotations |
**Important:** The eventID is the true unique identifier. Multiple events can share identical names (e.g., weekly Raid Hours with the same bosses) but will always have different eventIDs incorporating dates.
## Event Type Classification

### Event Type Values
The `eventType` field identifies the category of event. Values are lowercase with hyphens:
```json
"eventType": "community-day"
"eventType": "event"
"eventType": "raid-battles"
```

**When filtering by event type**, use direct string comparison:
```javascript
// Filter by exact type
const communityDays = events.filter(e => 
  e.eventType === 'community-day'
);

// Use prefix matching for related types
const raidEvents = events.filter(e => 
  e.eventType.startsWith('raid-')
);
```

### Event Types
These are the event types found in the data:
| Event Type | Description | Typical Duration |
|------------|-------------|------------------|
| `event` | General in-game events with spawns, bonuses, features | 3-7 days |
| `community-day` | Monthly featured Pokémon with bonuses and exclusive move | 3 hours |
| `community-day-classic` | Repeat of past Community Day | 3 hours |
| `raid-battles` | Specific Pokémon appearing in raid tiers | 5-14 days |
| `raid-day` | Special 3-hour raid event with increased spawns | 3 hours |
| `raid-hour` | Weekly coordinated raid hour (Wednesdays) | 1 hour |
| `pokemon-spotlight-hour` | Weekly featured Pokémon with bonus (Tuesdays) | 1 hour |
| `pokestop-showcase` | Competition to display Pokémon at PokéStops | 1-2 days |
| `go-battle-league` | PvP league rotation and cup availability | ~7 days |
| `max-mondays` | Weekly featured Dynamax Pokémon (Mondays) | 1 hour |
| `max-battles` | Multi-day Dynamax/Gigantamax events | 1-2 days |
| `team-go-rocket` | Team GO Rocket takeover events | 2-3 days |
| `pokemon-go-tour` | Large-scale ticketed events (in-person and global) | 2-3 days |
| `season` | 3-month seasonal game-wide changes | ~3 months |
| `go-pass` | Monthly subscription reward track period | ~1 month |
| `research` | Special or Masterwork Research availability window | Varies |
## Heading Property
The `heading` field is the human-readable version of the event type:
| eventType | heading |
|-----------|---------|
| `community-day` | `Community Day` |
| `raid-battles` | `Raid Battles` |
| `pokemon-spotlight-hour` | `Pokémon Spotlight Hour` |
| `go-battle-league` | `GO Battle League` |
**Purpose:** Use `heading` for display in user interfaces, as it:
- Uses proper capitalization and spacing
- Provides consistent formatting
## Event Naming Conventions
Event names follow patterns based on their type:
### Pokémon-Focused Events
Names include Pokémon species and forms:
- `"Piplup Community Day Classic"`
- `"Grookey Community Day"`
- `"Genesect (Burn Drive), Genesect (Chill Drive) Raid Hour"`
- `"Thundurus (Incarnate Forme) in 5-star Raid Battles"`
**Form Designations:**
- Parentheses indicate forms: `(Incarnate Forme)`, `(Burn Drive)`
- Multiple Pokémon are comma-separated
- "Forme" vs "Form" spelling varies (typically "Forme" for official names)
### Themed Events
General events have descriptive names:
- `"Pinch Perfect"`
- `"High Zaptitude"`
- `"Into the Depths"`
- `"Precious Pals"`
### GO Battle League
Follows the pattern: `"{Leagues/Cups} | {Season Name}"`:
- `"Great League, Ultra League, and Master League | Precious Paths"`
- `"Ultra League and Sunshine Cup: Great League Edition | Precious Paths"`
- `"Master Premier and Retro Cup: Great League Edition | Precious Paths"`
**Pattern Details:**
- Available leagues/cups listed with commas and "and" before the last item
- `|` separator between leagues and season name
- Cup names may include edition specifiers (": Great League Edition")
### Recurring Weekly Events
- **Raid Hour:** `"{Boss Pokémon} Raid Hour"`
- **Spotlight Hour:** `"{Featured Pokémon} Spotlight Hour"`
- **Max Monday:** `"Dynamax {Pokémon} during Max Monday"`
## Timestamp and Timezone Handling
### Timestamp Format
Timestamps use **ISO 8601 format WITH timezone indicators**:
```json
"start": "2026-01-04T14:00:00.000Z"
"end": "2026-01-04T17:00:00.000Z"
```
**Format breakdown:**
- `YYYY-MM-DD` - Date
- `T` - Separator
- `HH:mm:ss.SSS` - Time with milliseconds (always `.000`)
- `Z` - UTC/GMT timezone indicator (always present)
### Timezone Field Interpretation
The `timezone` field is **critical** for correctly interpreting timestamps:
| timezone Value | Interpretation | Use Case | Example Events |
|----------------|----------------|----------|----------------|
| `"Local Time"` | Event starts at the specified time in each player's local timezone | Recurring daily/weekly events, most general events | Community Day at 14:00 everywhere, Raid Hour at 18:00 everywhere |
| `null` | Event starts at the specified time in UTC (same moment globally) | Global simultaneous starts, league rotations, international GO Tour events | GO Battle League rotations, in-person GO Tour hours |
**Examples:**
```javascript
// Community Day: 14:00-17:00 Local Time
{
  "start": "2026-01-18T14:00:00.000Z",
  "end": "2026-01-18T17:00:00.000Z",
  "timezone": "Local Time"
}
// Starts at 14:00 in New York, 14:00 in London, 14:00 in Tokyo (different moments)
// GO Battle League: 21:00 UTC
{
  "start": "2025-12-30T21:00:00.000Z",
  "end": "2026-01-06T21:00:00.000Z",
  "timezone": null
}
// Starts at the same moment worldwide (21:00 UTC = 16:00 EST, 02:00 JST next day)
```
### Working with Timestamps
```javascript
// Helper function to parse event timestamps correctly
function getEventDate(timestamp, timezone) {
  if (timezone === "Local Time") {
    // Parse as local time (strip Z suffix for local interpretation)
    return new Date(timestamp.replace('Z', ''));
  } else {
    // Parse as UTC (timestamp already has Z suffix)
    return new Date(timestamp);
  }
}
// Check if event is currently active
function isEventActive(event) {
  const now = new Date();
  const start = getEventDate(event.start, event.timezone);
  const end = getEventDate(event.end, event.timezone);
  return now >= start && now <= end;
}
```
### Timezone Patterns by Event Type
| Event Type | timezone | Reasoning |
|------------|----------|-----------|
| Weekly recurring (Raid Hour, Spotlight Hour, Max Monday) | `"Local Time"` | Consistent local time experience worldwide |
| Community Day | `"Local Time"` | Same local hours globally |
| General events | `"Local Time"` | Regional rolling start |
| GO Battle League | `null` | Global simultaneous rotation |
| GO Tour (global) | `"Local Time"` | Same hours locally |
| GO Tour (in-person venue) | `null` | Specific venue timezone in UTC |
## Image URLs and Placeholder Detection
### Image Patterns
Images are hosted on the LeekDuck CDN with two distinct patterns:
#### Custom Article Images (Fully Detailed Events)
```
https://cdn.leekduck.com/assets/img/events/article-images/{YYYY}/{YYYY-MM-DD-event-id}/image-name.jpg
```
Example:
```
https://cdn.leekduck.com/assets/img/events/article-images/2026/2026-01-18-january-communityday2026/grookey-community-day-january-2026.jpg
```
**Characteristics:**
- Includes date folder structure
- Event-specific image name
- Indicates fully announced event with complete details
#### Default Placeholder Images
```
https://cdn.leekduck.com/assets/img/events/{default-image-name}.jpg
```
**Common default images:**
| File Name | Event Type | Usage |
|-----------|-----------|--------|
| `events-default-img.jpg` | Generic events | General placeholder for unannounced events |
| `cd-default.jpg` | Community Day | Future Community Day without featured Pokémon revealed |
| `pokemonspotlighthour.jpg` | Spotlight Hour | Standard Spotlight Hour template |
| `raidhour.jpg` | Raid Hour | Standard Raid Hour template |
| `mega-default.jpg` | Mega Raids | Mega Raid placeholder |
| `max-battles-kanto.jpg` | Max Battles | Dynamax/Gigantamax template |
**Identifying placeholder events:**
```javascript
// Check if event is a placeholder
function isPlaceholder(event) {
  return event.extraData === null && (
    event.image.includes('default') ||
    event.image.endsWith('pokemonspotlighthour.jpg') ||
    event.image.endsWith('raidhour.jpg') ||
    event.image.endsWith('cd-default.jpg')
  );
}
// Get all fully detailed events
const detailedEvents = events.filter(e => 
  e.image.includes('article-images') && 
  e.extraData !== null // Note: Currently all are null
);
```
### Image Dimensions
Images have varied dimensions with no strict standard:
| Aspect Ratio | Dimensions | Frequency |
|--------------|------------|-----------|
| 16:9 | 1280 × 720 | Most common |
| 2:1 | 1024 × 512 | Common for defaults |
| 16:9 | 1920 × 1080 | Occasional high-res |
| Various | Varied | Event-specific |
**Usage recommendation:** Always use the provided `imageWidth` and `imageHeight` for proper aspect ratio rendering.
## Event Duration Patterns
Events follow **highly consistent** duration patterns based on type:
### One-Hour Events (Local Time)
| Event Type | Time (Local) | Day of Week |
|------------|--------------|-------------|
| Raid Hour | 18:00 - 19:00 | Wednesday |
| Spotlight Hour | 18:00 - 19:00 | Tuesday |
| Max Monday | 18:00 - 19:00 | Monday |
```javascript
// All occur at exactly 18:00-19:00 local time
{
  "start": "2026-01-06T18:00:00.000",
  "end": "2026-01-06T19:00:00.000",
  "timezone": "Local Time"
}
```
### Three-Hour Events (Local Time)
| Event Type | Time (Local) | Frequency |
|------------|--------------|-----------|
| Community Day | 14:00 - 17:00 | Monthly (usually 3rd weekend) |
| Community Day Classic | 14:00 - 17:00 | Occasional |
| Raid Day | 14:00 - 17:00 | Occasional special events |
| Shadow Raid Day | 14:00 - 17:00 | Occasional |
| Max Battle Day | 14:00 - 17:00 | Occasional |
### Multi-Day Events
| Event Type | Typical Duration |
|------------|------------------|
| General events | 3-7 days |
| Raid boss rotations | 5-14 days (often ~11 days) |
| GO Battle League rotation | ~7 days |
| Team GO Rocket takeover | 2-3 days |
| Dynamax Weekend | 1-2 days |
| GO Tour (in-person) | 2-3 days |
| GO Tour (global) | 2 days |
### Extended Periods
| Event Type | Typical Duration |
|------------|------------------|
| GO Pass | ~1 month |
| Season | ~3 months |
| Masterwork Research | Until completion |
**Using duration to identify event types:**
```javascript
// Calculate event duration in hours
function getEventDuration(event) {
  const start = new Date(event.start);
  const end = new Date(event.end);
  return (end - start) / (1000 * 60 * 60); // milliseconds to hours
}
// Identify likely event type by duration
function guessEventTypeByDuration(event) {
  const hours = getEventDuration(event);
  
  if (hours === 1) return 'One-hour weekly event (Raid/Spotlight/Max Monday)';
  if (hours === 3) return 'Three-hour event (Community Day/Raid Day)';
  if (hours >= 24 * 85 && hours <= 24 * 95) return 'Season (~3 months)';
  if (hours >= 24 * 27 && hours <= 24 * 35) return 'GO Pass (~1 month)';
  if (hours >= 24 * 5 && hours <= 24 * 8) return 'GO Battle League rotation (~7 days)';
  
  return 'Variable duration event';
}
```
## Extra Data Structure
The `extraData` field is designed to contain comprehensive event details but is **frequently null**, especially for:
- Future events not yet fully announced
- Placeholder entries for scheduled events
- Events using default banner images

### Designed Structure (When Present)
When `extraData` is populated, it contains event-specific information such as:
**General Events:**
```json
"extraData": {
  "event": {
    "bonuses": ["2× Catch XP", "2× Catch Stardust"],
    "features": ["Increased spawns", "Special Field Research"],
    "shinies": [{"name": "Pokémon", "debut": true}],
    "customSections": {
      "spawns": {"wild": [...], "incense": [...], "lures": [...]},
      "raids": {"1star": [...], "3star": [...], "5star": [...]},
      "research": {"field": [...], "timed": [...]}
    }
  }
}
```
**Community Day:**
```json
"extraData": {
  "communityday": {
    "spawns": ["Featured Pokémon"],
    "bonuses": ["3× Catch XP", "2-hour Lure Duration"],
    "shinies": ["Featured Pokémon"],
    "featuredAttack": "Exclusive Move Name",
    "specialresearch": ["Research Name"],
    "ticketedResearch": {"name": "...", "price": "..."}
  }
}
```
**Raid Battles:**
```json
"extraData": {
  "raidbattles": {
    "bosses": ["Pokémon names"],
    "shinies": ["Shiny-capable Pokémon"],
    "tiers": {"1": [...], "3": [...], "5": [...]},
    "featuredAttacks": ["Special moves"]
  }
}
```

### Shiny Releases in Events
Many events feature shiny Pokémon releases and increased shiny rates:

**New Shiny Debuts**: Events often introduce shiny forms for Pokémon that previously didn't have them available. The `extraData` may list newly released shinies.

**Increased Shiny Rates**: During certain events (especially Community Days), featured Pokémon have significantly boosted shiny encounter rates:
- **Community Day**: ~1/25 odds for featured Pokémon (compared to ~1/500 standard)
- **Raid Day**: ~1/10 odds for featured raid bosses
- **General Events**: May feature boosted rates for specific spawns

**Shiny Data in extraData**:
```javascript
// Community Day shinies (evolution line)
"shinies": [
  {"name": "Piplup", "image": "..._shiny.png", "canBeShiny": true},
  {"name": "Prinplup", "image": "..._shiny.png", "canBeShiny": true},
  {"name": "Empoleon", "image": "..._shiny.png", "canBeShiny": true}
]

// Raid event shinies
"shinies": ["Genesect", "Blacephalon"]  // May be simple string array
```

**Cross-Reference with Shiny Data**: For complete shiny availability, cross-reference event data with `shiny.min.json` which contains the authoritative list of all released shiny forms with their release dates.
### Safe Access Pattern
**Always check for null** when accessing extraData:
```javascript
// Safely access nested extraData
const bonuses = event.extraData?.event?.bonuses || [];
const hasDetails = event.extraData !== null;
// Identify events with full details
const fullyDetailed = events.filter(e => e.extraData !== null);
// Identify placeholder events
const placeholders = events.filter(e => 
  e.extraData === null && 
  (e.image.includes('default') || 
   e.image.includes('pokemonspotlighthour') ||
   e.image.includes('raidhour'))
);
```
## Overlapping Events
**Multiple events commonly run simultaneously.** This is normal game behavior, not a data error.
### Common Overlap Scenarios
**1. General Event + Raid Rotation + Weekly Features:**
```javascript
// Example: January 6-11, 2026
{
  "name": "Pinch Perfect",           // General event
  "start": "2026-01-06T10:00:00.000",
  "end": "2026-01-11T20:00:00.000"
},
{
  "name": "Genesect (Burn Drive), Genesect (Chill Drive) in 5-star Raid Battles",
  "start": "2026-01-05T10:00:00.000",
  "end": "2026-01-16T10:00:00.000"   // Overlaps with event
},
{
  "name": "Barboach Spotlight Hour",  // Weekly feature during event
  "start": "2026-01-06T18:00:00.000",
  "end": "2026-01-06T19:00:00.000"
}
```
**2. Season + GO Pass + Multiple Active Events:**
Seasons and GO Pass run for extended periods, overlapping with everything.
**3. Event Takeovers (Team GO Rocket):**
```javascript
{
  "name": "Precious Pals",
  "start": "2026-01-20T10:00:00.000",
  "end": "2026-01-25T20:00:00.000"
},
{
  "name": "Precious Pals: Taken Over",  // Takeover within main event
  "start": "2026-01-23T10:00:00.000",
  "end": "2026-01-25T20:00:00.000"
}
```
### Finding Overlapping Events
```javascript
// Get all events active at a specific time
function getEventsAtTime(events, timestamp) {
  const checkTime = new Date(timestamp);
  
  return events.filter(event => {
    const start = getEventDate(event.start, event.timezone);
    const end = getEventDate(event.end, event.timezone);
    return checkTime >= start && checkTime <= end;
  });
}
// Find currently active events
const now = new Date();
const activeNow = getEventsAtTime(events, now);
console.log(`${activeNow.length} events currently active`);
// Group by event type
const activeByType = activeNow.reduce((acc, e) => {
  const type = e.eventType;
  if (!acc[type]) acc[type] = [];
  acc[type].push(e);
  return acc;
}, {});
```
## Duplicate Events and Series
### Recurring Weekly Events
Events like Raid Hour, Spotlight Hour, and Max Monday **recur weekly** with:
- Identical or similar names
- Different eventIDs (incorporating dates)
- Same time slots
- Different featured Pokémon
**Example - Multiple Raid Hours:**
```json
{
  "eventID": "raidhour20260107",
  "name": "Genesect (Burn Drive), Genesect (Chill Drive) Raid Hour",
  "start": "2026-01-07T18:00:00.000"
},
{
  "eventID": "raidhour20260114",
  "name": "Genesect (Burn Drive), Genesect (Chill Drive) Raid Hour",  // Same name
  "start": "2026-01-14T18:00:00.000"  // Different date
}
```
### Event Series
Some events are part of named series:
**Max Mondays Series:**
```javascript
const maxMondays = events.filter(e => 
  e.name.includes('Max Monday')
).sort((a, b) => new Date(a.start) - new Date(b.start));
// Different Pokémon each week:
// "Dynamax Drampa during Max Monday"
// "Dynamax Caterpie during Max Monday"
// "Dynamax Beldum during Max Monday"
```
**GO Battle League Rotations (Precious Paths Season):**
```javascript
const gblSeason = events.filter(e => 
  e.name.includes('Precious Paths') &&
  e.eventType.startsWith('go-battle-league')
);
// Multiple rotations within same season
// Different league/cup combinations each week
```
**GO Tour Multi-Location:**
```javascript
// Same event, different locations and times
{
  "eventID": "pokemon-go-tour-kalos-tainan-2026",
  "name": "Pokémon GO Tour: Kalos - Tainan 2026",
  "start": "2026-02-20T01:00:00.000Z"  // UTC for in-person event
},
{
  "eventID": "pokemon-go-tour-kalos-los-angeles-2026",
  "name": "Pokémon GO Tour: Kalos - Los Angeles 2026",
  "start": "2026-02-20T17:00:00.000Z"  // Different time
},
{
  "eventID": "pokemon-go-tour-kalos-global-2026",
  "name": "Pokémon GO Tour: Kalos - Global 2026",
  "start": "2026-02-28T10:00:00.000",  // Local Time for global
  "timezone": "Local Time"
}
```
## Usage Examples
### Finding Current Events
```javascript
// Get all currently active events
function getCurrentEvents(events) {
  const now = new Date();
  
  return events.filter(event => {
    const start = event.timezone === "Local Time" 
      ? new Date(event.start.replace('Z', ''))
      : new Date(event.start);
    
    const end = event.timezone === "Local Time"
      ? new Date(event.end.replace('Z', ''))
      : new Date(event.end);
    
    return now >= start && now <= end;
  });
}
const currentEvents = getCurrentEvents(events);
console.log('Currently active:', currentEvents.map(e => e.name));
```
### Filtering by Event Type
```javascript
// Get all Community Days
const communityDays = events.filter(e => 
  e.eventType === 'community-day'
);
// Get all raid-related events
const raidEvents = events.filter(e => {
  return e.eventType === 'raid-battles' || 
         e.eventType === 'raid-hour' || 
         e.eventType === 'raid-day';
});
// Group by event type
const byType = events.reduce((acc, event) => {
  const type = event.eventType;
  if (!acc[type]) acc[type] = [];
  acc[type].push(event);
  return acc;
}, {});
console.log('Event types:', Object.keys(byType));
```
### Finding Upcoming Events
```javascript
// Get events starting in the next N days
function getUpcomingEvents(events, days = 7) {
  const now = new Date();
  const future = new Date();
  future.setDate(future.getDate() + days);
  
  return events
    .filter(e => {
      const start = e.timezone === "Local Time"
        ? new Date(e.start.replace('Z', ''))
        : new Date(e.start);
      return start > now && start <= future;
    })
    .sort((a, b) => new Date(a.start) - new Date(b.start));
}
const nextWeek = getUpcomingEvents(events, 7);
console.log('Coming up:', nextWeek.map(e => ({
  name: e.name,
  start: e.start
})));
```
### Building an Event Calendar
```javascript
// Group events by date
function groupByDate(events) {
  const grouped = {};
  
  events.forEach(event => {
    const date = event.start.split('T')[0]; // YYYY-MM-DD
    if (!grouped[date]) {
      grouped[date] = [];
    }
    grouped[date].push(event);
  });
  
  // Sort events within each day
  Object.keys(grouped).forEach(date => {
    grouped[date].sort((a, b) => a.start.localeCompare(b.start));
  });
  
  return grouped;
}
const calendar = groupByDate(events);
// Display calendar
Object.entries(calendar)
  .sort(([dateA], [dateB]) => dateA.localeCompare(dateB))
  .forEach(([date, dayEvents]) => {
    console.log(`\n${date}:`);
    dayEvents.forEach(e => {
      const time = e.start.split('T')[1].substring(0, 5);
      console.log(`  ${time} - ${e.name} (${e.heading})`);
    });
  });
```
### Finding Event Series
```javascript
// Get all events in a recurring series
function getEventSeries(events, pattern) {
  return events
    .filter(e => e.name.includes(pattern))
    .sort((a, b) => new Date(a.start) - new Date(b.start));
}
// Get all Spotlight Hours
const spotlightHours = getEventSeries(events, 'Spotlight Hour');
console.log('Upcoming Spotlight Hours:');
spotlightHours.forEach(e => {
  const pokemon = e.name.replace(' Spotlight Hour', '');
  console.log(`  ${e.start.split('T')[0]} - ${pokemon}`);
});
// Get all Max Mondays
const maxMondays = getEventSeries(events, 'Max Monday');
// Get all events for a specific season
function getSeasonEvents(events, seasonName) {
  return events.filter(e => 
    e.name.includes(seasonName) ||
    e.eventType.includes(seasonName.toLowerCase().replace(/\s+/g, '-'))
  );
}
const preciousPathsEvents = getSeasonEvents(events, 'Precious Paths');
```
### GO Battle League Analysis
```javascript
// Parse GO Battle League event details
function parseGBLEvent(event) {
  // Example: "Great League and Ultra Premier | Precious Paths"
  const parts = event.name.split(' | ');
  const leagues = parts[0];
  const season = parts[1] || null;
  
  // Parse individual leagues/cups
  const leagueList = leagues
    .split(/,| and /)
    .map(s => s.trim())
    .filter(s => s.length > 0);
  
  return {
    event: event,
    leagues: leagueList,
    season: season,
    start: new Date(event.start), // GBL uses UTC (timestamp has Z)
    end: new Date(event.end)
  };
}
// Get current GBL rotation
const gblEvents = events
  .filter(e => e.eventType === 'go-battle-league')
  .map(parseGBLEvent);
const now = new Date();
const currentGBL = gblEvents.find(e => e.start <= now && e.end >= now);
if (currentGBL) {
  console.log('Current GBL rotation:');
  console.log('  Leagues:', currentGBL.leagues.join(', '));
  console.log('  Season:', currentGBL.season);
  console.log('  Until:', currentGBL.end.toLocaleDateString());
}
```
### Identifying Placeholder Events
```javascript
// Find events that are placeholders (minimal details)
function isPlaceholder(event) {
  const defaultImages = [
    'default',
    'pokemonspotlighthour.jpg',
    'raidhour.jpg',
    'cd-default.jpg',
    'mega-default.jpg'
  ];
  
  const hasDefaultImage = defaultImages.some(img => 
    event.image.includes(img)
  );
  
  return event.extraData === null && hasDefaultImage;
}
const placeholders = events.filter(isPlaceholder);
const detailed = events.filter(e => !isPlaceholder(e));
console.log(`${placeholders.length} placeholder events`);
console.log(`${detailed.length} detailed events`);
// Group placeholders by type
const placeholdersByType = placeholders.reduce((acc, e) => {
  const type = e.eventType;
  acc[type] = (acc[type] || 0) + 1;
  return acc;
}, {});
console.log('Placeholder breakdown:', placeholdersByType);
```
### Finding Raid Boss Rotations
```javascript
// Get current raid bosses
const raidBosses = events.filter(e => {
  return e.eventType === 'raid-battles' || e.eventType === 'mega-raids';
});
const now = new Date();
const activeRaids = raidBosses.filter(e => {
  const start = new Date(e.start);
  const end = new Date(e.end);
  return start <= now && end <= now;
});
console.log('Current raid bosses:');
activeRaids.forEach(raid => {
  const tierMatch = raid.name.match(/(\d+)-star|Mega/);
  const tier = tierMatch ? tierMatch[0] : 'Unknown';
  console.log(`  ${tier}: ${raid.name}`);
});
```
### Event Duration Analysis
```javascript
// Calculate and categorize by duration
function analyzeEventDurations(events) {
  const durations = events.map(event => {
    const start = new Date(event.start);
    const end = new Date(event.end);
    const hours = (end - start) / (1000 * 60 * 60);
    
    return {
      name: event.name,
      type: event.eventType,
      hours: hours,
      category: categorizeDuration(hours)
    };
  });
  
  return durations;
}
function categorizeDuration(hours) {
  if (hours === 1) return '1-hour (weekly)';
  if (hours === 3) return '3-hour (special)';
  if (hours >= 24 && hours < 24 * 3) return '1-3 days';
  if (hours >= 24 * 3 && hours < 24 * 10) return '3-10 days';
  if (hours >= 24 * 25 && hours <= 24 * 35) return '~1 month';
  if (hours >= 24 * 85 && hours <= 24 * 95) return '~3 months';
  return 'Other';
}
const analysis = analyzeEventDurations(events);
// Group by category
const byCat = analysis.reduce((acc, e) => {
  if (!acc[e.category]) acc[e.category] = [];
  acc[e.category].push(e);
  return acc;
}, {});
console.log('Events by duration:');
Object.entries(byCat).forEach(([cat, evts]) => {
  console.log(`  ${cat}: ${evts.length} events`);
});
```
## Best Practices
### Data Consumption
1. **Filter by eventType using direct string comparison**. The eventType values are lowercase with hyphens (e.g., "community-day", "raid-battles").
2. **Treat extraData as frequently null**. Many events have `extraData: null`. Build your application to handle this gracefully.
3. **Use eventID as the unique identifier**, not event name. Multiple events can share names (e.g., weekly Raid Hours).
4. **Handle timezone carefully**. The `timezone` field determines whether timestamps are local or UTC. Always check this before parsing dates.
5. **Expect overlapping events**. Multiple events running simultaneously is normal, not a data error.
6. **Cache image URLs** but be prepared for them to change. All images are hosted on an external CDN.
7. **Default images indicate placeholders**. Events with default images typically lack full details and may have limited information until closer to the event date.
### Display Considerations
1. **Use `heading` for display** rather than parsing `eventType`. It provides properly formatted category labels.
2. **Show timezone context** to users. Display "Local Time" or "UTC" based on the `timezone` field so players understand when events start.
3. **Group overlapping events** intelligently in your UI. Show long-running events (seasons, GO Pass) separately from daily/weekly features.
4. **Indicate placeholder status** with visual cues (e.g., "Details coming soon" badge) for events with default images and null extraData.
5. **Format event names** appropriately:
   - For GO Battle League: Split on `|` to separate leagues from season
   - For recurring events: Extract featured Pokémon names
   - For series: Show series name and instance number
6. **Calculate and display duration** to help users understand event types at a glance.
### Query Optimization
1. **Pre-filter by date ranges** to reduce dataset size:
```javascript
const upcomingEvents = events.filter(e => 
  new Date(e.end) >= new Date()
);
```
2. **Build type indexes** for frequent filtering:
```javascript
const typeIndex = events.reduce((acc, e) => {
  const type = e.eventType;
  if (!acc[type]) acc[type] = [];
  acc[type].push(e);
  return acc;
}, {});
// Quick access
const allCommunityDays = typeIndex['community-day'] || [];
```
3. **Cache parsed dates** if performing multiple time-based queries:
```javascript
const eventsWithDates = events.map(e => ({
  ...e,
  startDate: e.timezone === "Local Time" 
    ? new Date(e.start.replace('Z', ''))
    : new Date(e.start),
  endDate: e.timezone === "Local Time"
    ? new Date(e.end.replace('Z', ''))
    : new Date(e.end)
}));
```
## Important Notes
- **Data volatility**: Events are constantly added, updated, and removed. The dataset represents a snapshot that can become outdated quickly. Always fetch fresh data regularly.
- **Placeholder prevalence**: Many future events appear as placeholders with default images and null extraData. Expect limited information until events are fully announced.
- **Timezone complexity**: Events use either local time (same hour everywhere) or UTC (same moment everywhere). Misinterpreting this can cause display errors spanning multiple timezones.
- **Event overlap is normal**: Multiple events running simultaneously is intentional game design (e.g., general event + raid rotation + weekly features + season + GO Pass).
- **Recurring events have separate entries**: Each instance of Raid Hour, Spotlight Hour, or Max Monday is a distinct entry with a unique eventID.
- **GO Tour complexity**: Large events like GO Tour have multiple entries for different locations, times, and ticket types (general admission vs. Mega Night).
- **Duration patterns are reliable**: Event durations are remarkably consistent and can be used to infer event types when other information is unclear.
- **Image dimensions vary**: No single standard aspect ratio exists. Always use the provided dimensions for proper rendering.
- **Past events may be present**: Recently completed events may remain in the dataset briefly before being archived.
- **Event naming patterns**: Names follow predictable patterns by type, making parsing and extraction feasible for automation.
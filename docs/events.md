## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json

## JSON Example

```json
{
    "eventID": "january-communityday2026",
    "name": "Grookey Community Day",
    "eventType": "community-day",
    "heading": "Community Day",
    "image": "https://cdn.leekduck.com/assets/img/events/article-images/2026/.../grookey-community-day-january-2026.jpg",
    "imageWidth": 1206,
    "imageHeight": 679,
    "start": "2026-01-18T14:00:00.000",
    "end": "2026-01-18T17:00:00.000",
    "timezone": "Local Time",
    "status": "upcoming",
    "isCurrent": false,
    "extraData": { ... }
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `eventID` | string | Unique event identifier (kebab-case) |
| `name` | string | Display name |
| `eventType` | string | Event category: "community-day", "raid-day", "spotlight-hour", "event", "season", etc. |
| `heading` | string | Human-readable category label |
| `image` | string | Event banner URL |
| `imageWidth` | integer | Banner width in pixels |
| `imageHeight` | integer | Banner height in pixels |
| `start` | string | Start time (ISO 8601 format) |
| `end` | string | End time (ISO 8601 format) |
| `timezone` | string\|null | "Local Time" for local events, null for UTC |
| `status` | string | "current" or "upcoming" |
| `isCurrent` | boolean | True if event is currently active |
| `extraData` | object\|null | Event-specific details (structure varies by eventType) |

## Data Structure

The endpoint returns an array of event objects sorted chronologically. Events can overlap, and multiple events are often active simultaneously (e.g., a season + weekly event + spotlight hour).

## Event Status and Current Events

- `status`: "upcoming" (starts in future) or "current" (currently active)
- `isCurrent`: Boolean duplicate of status check, provided for convenience
- Events transition from "upcoming" to "current" automatically at start time
- Past events are removed from the dataset

## Timezone Handling

**Local Time Events** (`timezone: "Local Time"`):
- Start/end times apply to player's local timezone
- Times are in ISO 8601 format without timezone offset
- Used for most events (Community Days, Spotlight Hours, local events)

**UTC Events** (`timezone: null`):
- Times are in ISO 8601 format with explicit UTC timezone
- Applied globally at the exact same moment worldwide
- Used for GO Battle League, season transitions, some raid releases

## Event Types and ExtraData Structures

### community-day
`extraData.communityday` contains:
- `spawns`: Featured Pokémon appearing more frequently
- `bonuses`: Active bonuses (Stardust multipliers, Candy bonuses, XP boosts)
- `bonusDisclaimers`: Time restrictions and exceptions for bonuses
- `shinies`: Available shiny Pokémon (featured + evolutions)
- `specialresearch`: Paid research storyline (steps, tasks, rewards)
- `featuredAttack`: Exclusive move learned during evolution window
- `photobomb`: GO Snapshot encounters
- `pokestopShowcases`: Active showcases during event
- `fieldResearchTasks`: Event-exclusive field research
- `lureModuleBonus`: Extended lure duration details
- `ticketedResearch`: Paid ticket price and description

### raid-day
`extraData.raidday` contains:
- `featured`: Raid bosses appearing in raids
- `featuredAttacks`: Exclusive moves available
- `raids`: Breakdown by tier (fiveStar, mega, other)
- `shinies`: Shiny forms available
- `bonuses`: Event-specific bonuses (extra passes, XP/Stardust multipliers)
- `ticketBonuses`: Bonuses exclusive to ticket holders
- `ticketPrice`: Cost if ticketed event
- `alternationPattern`: Description of rotating raid bosses
- `specialMechanics`: Unique battle mechanics or features

### raid-hour
`extraData.raidhour` contains:
- `featured`: Single raid boss object with details
- `canBeShiny`: Whether the featured boss can be shiny

### pokemon-spotlight-hour
`extraData.spotlight` contains:
- `name`: Featured Pokémon name
- `canBeShiny`: Shiny availability
- `image`: Pokémon icon URL
- `bonus`: Active bonus during hour (e.g., "2× Evolution XP", "2× Transfer Candy")
- `list`: Array of Pokémon (usually single entry matching featured)

### go-battle-league
`extraData` is typically null; details in event name/heading
League rotations and cups described in event name

### max-mondays
`extraData.maxmondays` contains:
- `featured`: Dynamax Pokémon available
- `bonus`: Event date/description

### season
`extraData.season` contains extensive seasonal data:
- `name`: Season name
- `bonuses`: Season-long active bonuses
- `spawns`: Biome/habitat spawn changes
- `eggs`: Complete egg pool by distance (2km, 5km, 7km, 10km, 12km, route, adventure)
- `researchBreakthrough`: 7-day streak reward Pokémon
- `specialResearch`: Season-specific storyline
- `masterworkResearch`: High-tier paid research
- `communityDays`: Scheduled Community Days
- `features`: New features, mechanics, debuts
- `goBattleLeague`: Season league schedule
- `goPass`: GO Pass details if active
- `pokemonDebuts`: New Pokémon released
- `maxPokemonDebuts`: New Dynamax/Gigantamax forms

### go-pass
`extraData` typically null; pass details in related season event

### pokemon-go-tour
`extraData.gotour` contains:
- `eventInfo`: Location, dates, pricing
- `exclusiveBonuses`: In-person attendee bonuses
- `ticketAddOns`: Additional purchasable items
- `whatsNew`: Debut features
- `habitats`: Rotating spawn habitats with schedules
- `incenseEncounters`: Incense-exclusive spawns
- `eggs`: Event egg pool by distance
- `raids`: Raid lineup by tier
- `research`: Field, special, timed, and masterwork research
- `shinyDebuts`: Pokémon with shiny forms debuting
- `shinies`: All available shinies
- `sales`: Merchandise and ticket information
- `costumedPokemon`: Special costume variants

### team-go-rocket
`extraData` typically null; Rocket invasion details separate

### pokestop-showcase
`extraData.pokestopshowcase` contains:
- `featured`: Pokémon eligible for showcases
- `description`: Rules and reward details

### event (generic)
`extraData.event` contains flexible structure:
- `bonuses`: Active bonuses
- `bonusDisclaimers`: Restrictions and notes
- `features`: Event highlights
- `shinies`: Available shinies
- `customSections`: Nested objects with:
  - `spawns`: Wild spawn information (paragraphs, lists, pokemon arrays)
  - `raids`: Raid boss information
  - `eggs`: Egg pool changes
  - `research`: Field/timed/paid research
  - `sales`: In-game shop items
  
## Important Notes

- Events are removed from dataset once they end (no historical archive)
- Event times should be converted to user's timezone for "Local Time" events
- `extraData` structure varies significantly by `eventType`; consuming applications must handle missing fields gracefully
- Multiple events with same `eventType` can run simultaneously (e.g., multiple raid-hour entries for different weeks)
- Image dimensions provided for responsive layout optimization
- Event IDs use kebab-case and are globally unique

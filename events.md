# Events Data - events.min.json

This file contains an array of current and upcoming Pokemon GO events, including general events, Community Days, Raid Hours, Spotlight Hours, seasonal events, and more. Data is automatically scraped and updated.

## Overview

The `events.min.json` file provides comprehensive event data for Pokemon GO, including event timing, featured content, bonuses, and detailed information specific to each event type. Each event has a unique structure based on its type, allowing clients to parse and display event-specific information appropriately.

## Endpoints

- **Minimized**: [`GET https://storage.googleapis.com/resume_site_goblin/files/events.min.json`](https://storage.googleapis.com/resume_site_goblin/files/events.min.json)
- **CDN (jsDelivr)**: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json)

---

## Data Structure

The root of the file is an **array** of event objects.

```json
[
  { /* event object */ },
  { /* event object */ },
  ...
]
```

---

## Example Event Object

```json
{
    "eventID": "winter-weekend-2025",
    "name": "Winter Weekend",
    "eventType": "event",
    "heading": "Event",
    "image": "https://cdn.leekduck.com/assets/img/events/article-images/2025/2025-12-27-winter-weekend-2025/winter-weekend-eevee.jpg",
    "start": "2025-12-27T10:00:00.000",
    "end": "2025-12-28T20:00:00.000",
    "timezone": "Local Time",
    "extraData": {
        "event": {
            "bonuses": [
                {
                    "text": "There's no daily limit for how many GO Points you can earn!",
                    "image": "https://cdn.leekduck.com/assets/img/events/bonuses/none.png"
                }
            ],
            "features": [],
            "shinies": [],
            "customSections": {
                "research": {
                    "paragraphs": [
                        "Event-exclusive Timed Research",
                        "There will be a free Timed Research opportunity available.",
                        "Timed Research rewards include the following."
                    ],
                    "lists": [
                        [
                            "XP",
                            "Stardust",
                            "Encounters with Eevee wearing a holiday hat*"
                        ]
                    ],
                    "pokemon": []
                }
            }
        }
    }
}
```

# Fields

| Field           | Type     | Description
|---------------- |--------- |---------------------
| **`eventID`**   | `string` | The unique identifier for the event.
| **`name`**      | `string` | The name of the event.
| **`eventType`** | `string` | The type of the event. This determines the structure of `extraData`. See [List of Event Types](#list-of-event-types).
| **`heading`**   | `string` | The display heading for the event.
| **`image`**     | `string` | URL to the header/thumbnail image for the event.
| **`start`**     | `string` | The start date of the event in ISO 8601 format. See [Note for Start/End dates](#note-for-startend-dates).
| **`end`**       | `string` | The end date of the event in ISO 8601 format. See [Note for Start/End dates](#note-for-startend-dates).
| **`timezone`**  | `string` | The timezone context (e.g., "Local Time").
| **`extraData`** | `object` | Detailed information specific to the `eventType`. Can be `null`. See [Extra Data](#extra-data).

## List of Event Types

| Event Type | Description |
|---|---|
| `event` | General event type. |
| `pokestop-showcase` | PokéStop Showcases. |
| `max-mondays` | Dynamax events on Mondays. |
| `go-pass` | Seasonal or monthly pass progression. |
| `timed-research` | Limited-time research events. |
| `go-battle-league` | GBL updates and cups. |
| `pokemon-spotlight-hour` | Weekly spotlight hours. |
| `raid-battles` | Raid boss rotations. |
| `raid-hour` | Weekly raid hours. |
| `community-day` | Community Day events. |
| `raid-day` | Special Raid Day events. |
| `team-go-rocket` | Team GO Rocket takeovers. |
| `max-battles` | Max Battle events. |
| `pokemon-go-tour` | Major GO Tour events. |
| `research` | Masterwork or paid research. |
| `season` | Seasonal overviews. |

## Note on Start/End Dates

The `start` and `end` fields are DateTime strings encoded in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format (`YYYY-MM-DDTHH:MM:SS.sss`).

### Timezone Handling

Most Pokemon GO events occur based on the user's local timezone:
- When `timezone` is `"Local Time"`: The event starts/ends at the specified time in each player's local timezone
- When `timezone` is `null`: The times are in UTC and the event starts/ends at the same moment globally

**Example**: An event with `"start": "2025-12-27T10:00:00.000"` and `"timezone": "Local Time"` begins at 10:00 AM in New York, 10:00 AM in London, 10:00 AM in Tokyo, etc. - each at different moments in absolute time.

## Extra Data

For some event types, `extraData` contains specific structures. The key inside `extraData` usually matches the `eventType` (with dashes removed or generic naming).

### Event (`event`)
Generic events often include bonuses, spawns, and shinies.

```json
"extraData": {
    "event": {
        "bonuses": [ { "text": "...", "image": "..." } ],
        "bonusDisclaimers": [],
        "features": [],
        "shinies": [],
        "customSections": {
            "spawns": { "paragraphs": [], "lists": [], "pokemon": [] },
            "eggs": { ... },
            "raids": { ... },
            "research": { ... }
        }
    }
}
```

### Community Day (`communityday`)

```json
"extraData": {
    "communityday": {
        "spawns": [ { "name": "Piplup", "image": "...", "canBeShiny": true } ],
        "bonuses": [ { "text": "Increased Spawns", "image": "..." } ],
        "bonusDisclaimers": [],
        "shinies": [ { "name": "Piplup", "image": "...", "canBeShiny": false } ],
        "specialresearch": [
            {
                "step": 1,
                "name": "Community Day Classic: Piplup (1/3)",
                "tasks": [
                    {
                        "text": "Catch 3 Pokémon",
                        "reward": { "text": " Piplup ", "image": "..." }
                    }
                ],
                "rewards": [ { "text": "Piplup", "image": "..." } ]
            }
        ]
    }
}
```

### Raid Battles (`raidbattles`)

Used for specific raid events or rotations (e.g., Mega Raids, 5-Star rotations).

```json
"extraData": {
    "raidbattles": {
        "bosses": [
            { "name": "Mega Glalie", "image": "...", "canBeShiny": true }
        ],
        "shinies": [
            { "name": "Glalie", "image": "...", "canBeShiny": false }
        ],
        "tiers": {
            "mega": [],
            "fiveStar": [],
            "threeStar": [],
            "oneStar": []
        },
        "alternationPattern": "",
        "featuredAttacks": []
    }
}
```

### Pokémon Spotlight Hour (`spotlight`)

```json
"extraData": {
    "spotlight": {
        "name": "Delibird",
        "canBeShiny": true,
        "image": "...",
        "bonus": "2× Catch Stardust",
        "list": [
            { "name": "Delibird", "canBeShiny": true, "image": "..." }
        ]
    }
}
```

### Max Mondays (`maxmondays`)

```json
"extraData": {
    "maxmondays": {
        "featured": {
            "name": "Omanyte",
            "image": "...",
            "canBeShiny": true
        },
        "bonus": "December 29, 2025"
    }
}
```

### PokéStop Showcase (`pokestopshowcase`)

```json
"extraData": {
    "pokestopshowcase": {
        "featured": [
            { "name": "Eevee", "image": "...", "canBeShiny": false }
        ],
        "description": "There will be PokéStop Showcases featuring Eevee. Top the leaderboard to win rewards including Premium Items!  Eevee"
    }
}
```

### Timed Research (`timedresearch`)

```json
"extraData": {
    "timedresearch": {
        "name": "Best Friends Forever Timed Research",
        "description": "Event-exclusive Timed Research",
        "isPaid": false,
        "price": null,
        "tasks": [
            {
                "step": 1,
                "name": "Best Friends Forever Timed Research (1/4)",
                "tasks": [ { "text": "Make a new friend", "reward": { ... } } ],
                "rewards": [ { "text": "×10", "image": "..." } ]
            }
        ],
        "availability": {
            "start": "Monday, December 15, at 10:00 a.m.",
            "end": "Monday, December 29, 2025, at 8:00 p.m."
        }
    }
}
```

### Raid Hour (`raidhour`)

```json
"extraData": {
    "raidhour": {
        "featured": {
            "name": "Blacephalon Raid Hour",
            "image": "",
            "canBeShiny": false
        }
    }
}
```

### Raid Day (`raidday`)

```json
"extraData": {
    "raidday": {
        "featured": [
            { "name": "Reshiram", "image": "...", "canBeShiny": true }
        ],
        "bonuses": [ { "text": "...", "image": "..." } ],
        "shinies": [ { "name": "Reshiram", "image": "...", "canBeShiny": false } ]
    }
}
```

### Research / Masterwork (`research`)

```json
"extraData": {
    "research": {
        "name": "Masterwork Research: A Precious Catch",
        "researchType": "masterwork",
        "isPaid": true,
        "price": 7.99,
        "description": "Ticket-exclusive Masterwork Research",
        "tasks": [
            {
                "step": 1,
                "name": "Masterwork Research: A Precious Catch (1/?)",
                "tasks": [],
                "rewards": []
            }
        ]
    }
}
```

### GO Tour (`gotour`)

```json
"extraData": {
    "gotour": {
        "eventInfo": { "name": "...", "location": "...", "ticketPrice": null },
        "habitats": {
            "General": { "info": "...", "spawns": [] }
        },
        "eggs": { "2km": [], "5km": [], "7km": [], "10km": [] },
        "raids": { "oneStar": [], "threeStar": [], "fiveStar": [], "mega": [] },
        "shinies": [],
        "sales": []
    }
}
```

### Season (`season`)

```json
"extraData": {
    "season": {
        "name": "Precious Paths",
        "eggs": {
            "2km": [],
            "5km": [],
            "route": [],
            "adventure": []
        },
        "specialResearch": [ "..." ],
        "features": [ "..." ]
    }
}
```

---

## Usage Notes

- **Data Updates**: This file is automatically updated via web scraper. Event counts and specific events will change as Niantic announces new content.
- **Ordering**: Events are typically ordered chronologically by start date.
- **Image URLs**: All image URLs point to external CDN sources.
- **Shiny Information**: The `canBeShiny` field appears in various sub-objects to indicate shiny availability.
- **Null Values**: The `extraData` field can be `null` for simple events with no additional structured data.
- **Event Types**: Different event types have completely different `extraData` structures. Always check the `eventType` field before parsing `extraData`.

---

## Common Sub-Object Structures

### Bonus Object

Used in various event types to describe event bonuses.

| Field | Type | Description |
|-------|------|-------------|
| `text` | `string` | Human-readable description of the bonus. |
| `image` | `string` | URL to an icon representing the bonus. |

### Pokemon Object

Used in spawns, shinies, and featured lists.

| Field | Type | Description |
|-------|------|-------------|
| `name` | `string` | The name of the Pokémon. |
| `image` | `string` | URL to the Pokémon's icon image. |
| `canBeShiny` | `boolean` | Whether this Pokémon can be shiny. |

### Task Object (in research)

Used in special research and timed research structures.

| Field | Type | Description |
|-------|------|-------------|
| `step` | `number` | The step number in the research. |
| `name` | `string` | The name of this research step. |
| `tasks` | `array` | Array of task objects with `text` and `reward` fields. |
| `rewards` | `array` | Array of reward objects with `text` and `image` fields. |

---

## Integration Examples

### Parsing Events by Type

```javascript
const events = await fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json').then(r => r.json());

// Filter by event type
const communityDays = events.filter(e => e.eventType === 'community-day');
const spotlightHours = events.filter(e => e.eventType === 'pokemon-spotlight-hour');
const activeEvents = events.filter(e => {
  const now = new Date();
  const start = new Date(e.start);
  const end = new Date(e.end);
  return now >= start && now <= end;
});
```

### Displaying Event Countdown

```javascript
function getEventStatus(event) {
  const now = new Date();
  const start = new Date(event.start);
  const end = new Date(event.end);
  
  if (now < start) {
    return `Starts in ${formatDuration(start - now)}`;
  } else if (now >= start && now <= end) {
    return `Ends in ${formatDuration(end - now)}`;
  } else {
    return 'Ended';
  }
}
```

---

## Data Source

This data is automatically scraped and updated to keep current with in-game events.

# Pokémon GO Combined Data Schema
## Overview
The `combined.min.json` file is a comprehensive aggregation that merges detailed event information with other game data. This dataset serves as a single-source file containing events with their full `extraData` details, providing a complete picture of ongoing and upcoming game features.

**Important:** This file combines data from multiple sources:
- **Events** - Complete event details with populated `extraData` objects
- Other game data may be included depending on the combination strategy

This differs from `events.min.json` where `extraData` is often `null` for placeholder events.

## Data Access
The data is available via CDN:
```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json
```

### Usage Example
```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/combined.min.json')
  .then(response => response.json())
  .then(data => {
    console.log(`Loaded ${data.events.length} events with details`);
    
    // Access Community Day details
    const communityDays = data.events.filter(e => 
      e.eventType.includes('community-day')
    );
    
    // Access full event details
    communityDays.forEach(cd => {
      if (cd.extraData?.communityday) {
        console.log(`${cd.name}: ${cd.extraData.communityday.spawns.length} featured spawns`);
      }
    });
  });
```

## File Structure
The file contains a JSON object with an `events` array. Each event includes complete details in its `extraData` property.

```json
{
  "events": [
    {
      "eventID": "january-communitydayclassic2026",
      "name": "Piplup Community Day Classic",
      "eventType": "community-day",
      "heading": "Community Day",
      "image": "https://cdn.leekduck.com/assets/img/events/article-images/2026/...",
      "imageWidth": 1280,
      "imageHeight": 720,
      "start": "2026-02-24T21:00:00.000Z",
      "end": "2026-03-03T21:00:00.000Z",
      "timezone": "Local Time",
      "extraData": {
        "communityday": {
          "spawns": [...],
          "bonuses": [...],
          "bonusDisclaimers": [...],
          "shinies": [...],
          "specialresearch": [...],
          "featuredAttack": null,
          "photobomb": null,
          "pokestopShowcases": [],
          "fieldResearchTasks": [],
          "lureModuleBonus": null,
          "ticketedResearch": {...}
        }
      }
    }
  ]
}
```

## JSON Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Combined Data Schema",
  "description": "Schema for combined Pokémon GO game data with detailed event information",
  "type": "object",
  "required": ["events"],
  "properties": {
    "events": {
      "type": "array",
      "description": "Array of events with complete extraData details",
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
            "description": "Display name of the event"
          },
          "eventType": {
            "type": "string",
            "description": "Event category type (without skeleton-loading suffix in combined data)",
            "examples": ["community-day", "event", "raid-day", "spotlight-hour"]
          },
          "heading": {
            "type": "string",
            "description": "Human-readable category label"
          },
          "image": {
            "type": "string",
            "format": "uri",
            "description": "Event banner/promotional image URL"
          },
          "imageWidth": {
            "type": "integer",
            "description": "Image width in pixels",
            "minimum": 1
          },
          "imageHeight": {
            "type": "integer",
            "description": "Image height in pixels",
            "minimum": 1
          },
          "start": {
            "type": "string",
            "description": "ISO 8601 datetime when event begins",
            "format": "date-time"
          },
          "end": {
            "type": "string",
            "description": "ISO 8601 datetime when event ends",
            "format": "date-time"
          },
          "timezone": {
            "type": "string",
            "description": "Timezone specification",
            "examples": ["Local Time", "PST", "UTC"]
          },
          "extraData": {
            "type": ["object", "null"],
            "description": "Detailed event-specific data. Structure varies by event type. In combined.min.json, this is typically populated unlike in events.min.json",
            "properties": {
              "communityday": {
                "$ref": "#/definitions/communityDayData"
              },
              "event": {
                "$ref": "#/definitions/eventData"
              },
              "raidday": {
                "$ref": "#/definitions/raidDayData"
              },
              "spotlighthour": {
                "$ref": "#/definitions/spotlightHourData"
              }
            }
          }
        }
      }
    }
  },
  "definitions": {
    "communityDayData": {
      "type": "object",
      "description": "Detailed Community Day event data",
      "properties": {
        "spawns": {
          "type": "array",
          "description": "Featured Pokémon spawning during the event",
          "items": {
            "type": "object",
            "properties": {
              "name": {"type": "string"},
              "image": {"type": "string", "format": "uri"},
              "imageWidth": {"type": "integer"},
              "imageHeight": {"type": "integer"},
              "canBeShiny": {"type": "boolean"}
            }
          }
        },
        "bonuses": {
          "type": "array",
          "description": "Event bonuses active during Community Day",
          "items": {
            "type": "object",
            "properties": {
              "text": {"type": "string"},
              "image": {"type": "string", "format": "uri"},
              "imageWidth": {"type": "integer"},
              "imageHeight": {"type": "integer"}
            }
          }
        },
        "bonusDisclaimers": {
          "type": "array",
          "description": "Disclaimers or additional details about bonuses",
          "items": {"type": "string"}
        },
        "shinies": {
          "type": "array",
          "description": "Shiny forms available during the event",
          "items": {
            "type": "object",
            "properties": {
              "name": {"type": "string"},
              "image": {"type": "string", "format": "uri"},
              "imageWidth": {"type": "integer"},
              "imageHeight": {"type": "integer"},
              "canBeShiny": {"type": "boolean"}
            }
          }
        },
        "specialresearch": {
          "type": "array",
          "description": "Special Research tasks available"
        },
        "featuredAttack": {
          "type": ["string", "null"],
          "description": "Exclusive move available during evolution window"
        },
        "photobomb": {
          "type": ["object", "null"],
          "description": "Pokémon that can photobomb snapshots during the event"
        },
        "pokestopShowcases": {
          "type": "array",
          "description": "PokéStop Showcase competitions"
        },
        "fieldResearchTasks": {
          "type": "array",
          "description": "Event-specific field research tasks"
        },
        "lureModuleBonus": {
          "type": ["string", "null"],
          "description": "Special lure module effects during the event"
        },
        "ticketedResearch": {
          "type": ["object", "null"],
          "description": "Paid research ticket details",
          "properties": {
            "price": {"type": "number"},
            "description": {"type": "string"}
          }
        }
      }
    },
    "eventData": {
      "type": "object",
      "description": "Detailed general event data",
      "properties": {
        "bonuses": {
          "type": "array",
          "description": "Event bonuses",
          "items": {
            "type": "object",
            "properties": {
              "text": {"type": "string"},
              "image": {"type": "string", "format": "uri"},
              "imageWidth": {"type": "integer"},
              "imageHeight": {"type": "integer"}
            }
          }
        },
        "bonusDisclaimers": {
          "type": "array",
          "items": {"type": "string"}
        },
        "features": {
          "type": "array",
          "description": "Event features and highlights (may contain HTML)",
          "items": {"type": "string"}
        },
        "shinies": {
          "type": "array",
          "description": "New or featured shiny releases"
        },
        "customSections": {
          "type": "object",
          "description": "Custom sections like spawns, raids, eggs with HTML content",
          "additionalProperties": {
            "type": "object",
            "properties": {
              "paragraphs": {
                "type": "array",
                "items": {"type": "string"}
              },
              "lists": {
                "type": "array",
                "items": {
                  "type": "array",
                  "items": {"type": "string"}
                }
              }
            }
          }
        }
      }
    },
    "raidDayData": {
      "type": "object",
      "description": "Detailed Raid Day event data"
    },
    "spotlightHourData": {
      "type": "object",
      "description": "Detailed Spotlight Hour event data"
    }
  }
}
```

## Property Details

### Top-Level Structure
The combined data uses an object wrapper with an `events` array, allowing for potential future expansion with additional top-level properties for other data types.

### Event Types
Common `eventType` values include:
- `"community-day"` - Monthly Community Day events
- `"event"` - General game events
- `"raid-day"` - Special raid-focused events
- `"spotlight-hour"` - Weekly Spotlight Hour events
- `"go-battle-league"` - PvP season events
- `"season"` - Season change events

Note: In `combined.min.json`, the `eventType` typically does not include the " skeleton-loading" suffix present in `events.min.json`.

### Extra Data Structure
The `extraData` object structure varies by event type:

#### Community Day Events
- **spawns**: Featured Pokémon appearing frequently
- **bonuses**: XP, Stardust, Candy multipliers
- **shinies**: Shiny forms available (usually evolution line)
- **featuredAttack**: Exclusive move if evolved during the event window
- **ticketedResearch**: Optional paid research details

#### General Events
- **bonuses**: Active gameplay bonuses
- **features**: Event highlights (may include HTML formatting)
- **customSections**: Structured data about spawns, raids, eggs with rich HTML content
  - **paragraphs**: Descriptive text blocks
  - **lists**: Arrays of HTML-formatted list items (often Pokémon with images)

### Custom Sections HTML
The `customSections` often contain HTML markup for rich display:
```javascript
{
  "spawns": {
    "paragraphs": ["May encounter event-themed Pokémon..."],
    "lists": [[
      "<div class=\"pkmn-list-img grass\"><img src=\"...\"></div>...",
      "<div class=\"pkmn-list-img fire\"><img src=\"...\"></div>..."
    ]]
  }
}
```

## Common Use Cases

### Get All Active Events
```javascript
const now = new Date();
const activeEvents = data.events.filter(e => {
  const start = new Date(e.start);
  const end = new Date(e.end);
  return start <= now && end >= now;
});
```

### Find Community Days with Exclusive Moves
```javascript
const cdsWithMoves = data.events.filter(e => 
  e.extraData?.communityday?.featuredAttack
);
```

### Extract Event Bonuses
```javascript
const allBonuses = data.events
  .flatMap(e => e.extraData?.communityday?.bonuses || e.extraData?.event?.bonuses || []);
```

### Find Events with Ticketed Research
```javascript
const ticketedEvents = data.events.filter(e => 
  e.extraData?.communityday?.ticketedResearch?.price > 0
);
```

### Get Upcoming Events by Type
```javascript
const now = new Date();
const upcomingRaidDays = data.events.filter(e => 
  e.eventType === 'raid-day' && new Date(e.start) > now
);
```

## Differences from events.min.json

| Feature | events.min.json | combined.min.json |
|---------|----------------|-------------------|
| **Structure** | Array of events | Object with `events` array |
| **extraData** | Often `null` (placeholders) | Usually populated with details |
| **eventType** | Includes " skeleton-loading" suffix | Clean type name |
| **Purpose** | Event calendar/schedule | Detailed event information |
| **Update frequency** | More frequent (includes placeholders) | Less frequent (requires detail scraping) |

## Notes
- This file is generated by combining base event data with detailed scraped information
- The `extraData` population makes this file significantly larger than `events.min.json`
- Custom sections may contain HTML markup intended for web display
- Some events may still have `null` extraData if details haven't been scraped yet
- The file structure allows for future expansion to include other combined data types beyond events
- HTML content in features and customSections should be sanitized before display in untrusted contexts

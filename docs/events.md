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

## Notes

The data is an array of event objects. The `extraData` field structure changes based on `eventType`. Common eventTypes include: community-day, raid-day, raid-hour, spotlight-hour, go-battle-league, max-mondays, season, go-pass, pokemon-go-tour, team-go-rocket, pokestop-showcase.

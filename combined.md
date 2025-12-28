# Endpoints

- Minimized: [`GET https://storage.googleapis.com/resume_site_goblin/files/combined.min.json`](https://storage.googleapis.com/resume_site_goblin/files/combined.min.json)

**Note:** The endpoint URL assumes the standard Google Cloud Storage public URL format for the bucket `resume_site_goblin` and the `files/` directory structure.

# Example Combined Object

```json
{
    "events": [
        {
            "eventID": "winter-weekend-2025",
            "name": "Winter Weekend",
            "eventType": "event",
            "...": "..."
        }
    ],
    "raids": [
        {
            "name": "Alolan Sandshrew",
            "tier": "1-Star Raids",
            "...": "..."
        }
    ],
    "research": {
        "seasonalInfo": { ... },
        "tasks": [ ... ]
    },
    "eggs": [
        {
            "name": "Bulbasaur",
            "eggType": "1 km",
            "...": "..."
        }
    ],
    "rocketLineups": [
        {
            "name": "Cliff",
            "title": "Team GO Rocket Leader",
            "...": "..."
        }
    ],
    "event-links": {
        "winter-weekend-2025": "https://leekduck.com/events/winter-weekend-2025/",
        "...": "..."
    }
}
```

# Description

This file aggregates the data from all other individual JSON files into a single response object. This allows clients to fetch all data in one request.

# Root Keys

| Key | Type | Schema Reference | Description |
|---|---|---|---|
| **`events`** | `array` | [events.md](events.md) | List of all current and upcoming events. |
| **`raids`** | `array` | [raids.md](raids.md) | List of current Raid Bosses. |
| **`research`** | `object` | [research.md](research.md) | Current field research tasks and seasonal info. |
| **`eggs`** | `array` | [eggs.md](eggs.md) | List of Pokémon currently hatching from eggs. |
| **`rocketLineups`** | `array` | [rocketLineups.md](rocketLineups.md) | Current Team GO Rocket member lineups. |
| **`event-links`** | `object` | [event-links.md](event-links.md) | Map of event IDs to LeekDuck URLs. |

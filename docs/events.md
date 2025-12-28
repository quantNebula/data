
# Endpoints

- Minimized: [`GET https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json`](https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json)

**Note:** The endpoint URL assumes the standard Google Cloud Storage public URL format for the bucket `resume_site_goblin` and the `files/` directory structure.

# Example Event Object

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
| **`eventID`**   | `string` | The unique identifier for the event, which appears as the last segment in the LeekDuck event URL path.
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

## Note for Start/End dates

The `start` and `end` fields are DateTime objects encoded in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

Most events in Pokémon GO occur based around a user's local timezone. The `timezone` field typically indicates "Local Time". If the event happens at a specific global time, the ISO string may end in `Z` (UTC), though typically the scraper provides the raw string from the source.

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

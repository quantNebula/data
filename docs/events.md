# Events Data Documentation

## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json

## Overview

The endpoint returns an array of event objects representing current and upcoming Pokémon GO events. Events are sorted chronologically and include detailed information about bonuses, spawns, raids, research, and other event-specific content.

## Base Structure

All events share these common fields:

```json
{
  "eventID": "example-event-2026",
  "name": "Example Event",
  "eventType": "event",
  "heading": "Event",
  "image": "https://cdn.leekduck.com/assets/img/events/...",
  "imageWidth": 1280,
  "imageHeight": 720,
  "start": "2026-01-18T14:00:00.000",
  "end": "2026-01-18T17:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": {}
}
```

### Base Fields Reference

| Field | Type | Description |
|-------|------|-------------|
| `eventID` | string | Unique event identifier in kebab-case |
| `name` | string | Event display name |
| `eventType` | string | Event category (see Event Types section) |
| `heading` | string | Human-readable category label |
| `image` | string | Event banner image URL |
| `imageWidth` | integer | Banner width in pixels |
| `imageHeight` | integer | Banner height in pixels |
| `start` | string | Start time in ISO 8601 format |
| `end` | string | End time in ISO 8601 format |
| `timezone` | string\|null | "Local Time" for local events, null for UTC |
| `status` | string | Event status: "current" (active) or "upcoming" (future) |
| `isCurrent` | boolean | True if event is currently active |
| `extraData` | object\|null | Event-specific details (structure varies by eventType) |

## Event Types and Complete Examples

The following sections show actual JSON examples from the live data feed, demonstrating the real structure of each event type.

### community-day

**Example: Grookey Community Day**

```json
{
  "eventID": "january-communityday2026",
  "name": "Grookey Community Day",
  "eventType": "community-day",
  "heading": "Community Day",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2026/2026-01-18-january-communityday2026/grookey-community-day-january-2026.jpg",
  "imageWidth": 1206,
  "imageHeight": 679,
  "start": "2026-01-18T14:00:00.000",
  "end": "2026-01-18T17:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": {
    "communityday": {
      "spawns": [
        {
          "name": "Grookey",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm810.icon.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": true
        }
      ],
      "bonuses": [
        {
          "text": "Increased Spawns",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/wildgrass.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "3x Catch Stardust",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/stardust3x.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "3-hour Incense",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/incense.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "1-hour Lures*",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/lure.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "2x Catch Candy",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/candy.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "2x Chance to receive Candy XL from catching Pok\u00e9mon",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/candyxl.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "One additional Special Trade can be made for a maximum of three for the day*",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/trade.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "Trades made will require 50% less Stardust*",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/stardust2x.png",
          "imageWidth": 100,
          "imageHeight": 100
        }
      ],
      "bonusDisclaimers": [
        "* While most bonuses are only active during the three hours of the event (2:00 p.m. to 5:00 p.m. local time), these bonuses will be active from 2:00 p.m. to 9:00 p.m. local time.",
        "The three-hour Incense bonus excludes Daily Adventure Incense and the one-hour Lure bonus excludes Golden Lure Modules."
      ],
      "shinies": [
        {
          "name": "Grookey",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm810.s.icon.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Thwackey",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm811.s.icon.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Rillaboom",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm812.s.icon.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        }
      ],
      "specialresearch": [
        {
          "step": 1,
          "name": "Grookey Community Day (1/3)",
          "tasks": [
            {
              "text": "Catch 3 Pok\u00e9mon",
              "reward": {
                "text": "Grookey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
                "imageWidth": 75,
                "imageHeight": 97
              }
            },
            {
              "text": "Catch 5 Pok\u00e9mon",
              "reward": {
                "text": "Grookey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
                "imageWidth": 75,
                "imageHeight": 97
              }
            },
            {
              "text": "Catch 10 Pok\u00e9mon",
              "reward": {
                "text": "Grookey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
                "imageWidth": 75,
                "imageHeight": 97
              }
            },
            {
              "text": "Make 7 Nice Throws",
              "reward": {
                "text": "Ultra Ball \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Ultra%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Make 5 Great Throws",
              "reward": {
                "text": "Star Piece \u00d71",
                "image": "https://cdn.leekduck.com/assets/img/items/Star%20Piece.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Make 5 Curveball Throws",
              "reward": {
                "text": "Incense \u00d71",
                "image": "https://cdn.leekduck.com/assets/img/items/Incense.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            }
          ],
          "rewards": [
            {
              "text": "Grookey",
              "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
              "imageWidth": 75,
              "imageHeight": 97
            },
            {
              "text": "\u00d750",
              "image": "https://cdn.leekduck.com/assets/img/candy/regular/810.png",
              "imageWidth": 256,
              "imageHeight": 256
            },
            {
              "text": "\u00d75",
              "image": "https://cdn.leekduck.com/assets/img/candy/xl/810.png",
              "imageWidth": 256,
              "imageHeight": 256
            }
          ]
        },
        {
          "step": 2,
          "name": "Grookey Community Day (2/3)",
          "tasks": [
            {
              "text": "Catch 3 Pok\u00e9mon",
              "reward": {
                "text": "Grookey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
                "imageWidth": 75,
                "imageHeight": 97
              }
            },
            {
              "text": "Catch 5 Pok\u00e9mon",
              "reward": {
                "text": "Grookey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
                "imageWidth": 75,
                "imageHeight": 97
              }
            },
            {
              "text": "Catch 10 Pok\u00e9mon",
              "reward": {
                "text": "Thwackey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm811.icon.png",
                "imageWidth": 109,
                "imageHeight": 150
              }
            },
            {
              "text": "Use 3 Berries to help catch Pok\u00e9mon",
              "reward": {
                "text": "Ultra Ball \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Ultra%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Use 5 Berries to help catch Pok\u00e9mon",
              "reward": {
                "text": "Charged TM \u00d71",
                "image": "https://cdn.leekduck.com/assets/img/items/Charged%20TM.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Use 7 Berries to help catch Pok\u00e9mon",
              "reward": {
                "text": "Silver Pinap Berry \u00d73",
                "image": "https://cdn.leekduck.com/assets/img/items/Silver%20Pinap%20Berry.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            }
          ],
          "rewards": [
            {
              "text": "Grookey",
              "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
              "imageWidth": 75,
              "imageHeight": 97
            },
            {
              "text": "\u00d77500",
              "image": "https://cdn.leekduck.com/assets/img/items/Stardust.png",
              "imageWidth": 256,
              "imageHeight": 256
            },
            {
              "text": "\u00d73",
              "image": "https://cdn.leekduck.com/assets/img/items/Rare%20Candy.png",
              "imageWidth": 256,
              "imageHeight": 256
            }
          ]
        },
        {
          "step": 3,
          "name": "Grookey Community Day (3/3)",
          "tasks": [
            {
              "text": "Catch 3 Pok\u00e9mon",
              "reward": {
                "text": "Grookey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
                "imageWidth": 75,
                "imageHeight": 97
              }
            },
            {
              "text": "Catch 5 Pok\u00e9mon",
              "reward": {
                "text": "Grookey",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
                "imageWidth": 75,
                "imageHeight": 97
              }
            },
            {
              "text": "Catch 10 Pok\u00e9mon",
              "reward": {
                "text": "Rillaboom",
                "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm812.icon.png",
                "imageWidth": 175,
                "imageHeight": 220
              }
            },
            {
              "text": "Spin 3 Pok\u00e9Stops or Gyms",
              "reward": {
                "text": "Ultra Ball \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Ultra%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Spin 5 Pok\u00e9Stops or Gyms",
              "reward": {
                "text": "Rocket Radar \u00d71",
                "image": "https://cdn.leekduck.com/assets/img/items/Rocket%20Radar.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Explore 1 km",
              "reward": {
                "text": "Premium Battle Pass \u00d71",
                "image": "https://cdn.leekduck.com/assets/img/items/Premium%20Battle%20Pass.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            }
          ],
          "rewards": [
            {
              "text": "Grookey",
              "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm810.icon.png",
              "imageWidth": 75,
              "imageHeight": 97
            },
            {
              "text": "10000 XP",
              "image": "https://cdn.leekduck.com/assets/img/icons/xp.png",
              "imageWidth": 256,
              "imageHeight": 257
            },
            {
              "text": "\u00d71",
              "image": "https://cdn.leekduck.com/assets/img/items/Rare%20Candy%20XL.png",
              "imageWidth": 159,
              "imageHeight": 171
            }
          ]
        }
      ],
      "featuredAttack": null,
      "photobomb": null,
      "pokestopShowcases": [],
      "fieldResearchTasks": [],
      "lureModuleBonus": null,
      "ticketedResearch": {
        "price": 2,
        "description": ""
      }
    }
  }
}
```

**Key Fields:**
- `extraData.communityday.spawns`: Featured Pokémon appearing more frequently
- `extraData.communityday.bonuses`: Active bonuses (Stardust multipliers, Candy bonuses, XP boosts)
- `extraData.communityday.shinies`: Available shiny Pokémon (featured + evolutions)
- `extraData.communityday.specialresearch`: Paid research storyline with steps, tasks, and rewards
- `extraData.communityday.featuredAttack`: Exclusive move learned during evolution window

### event

**Example: Pinch Perfect**

```json
{
  "eventID": "pinch-perfect-2026",
  "name": "Pinch Perfect",
  "eventType": "event",
  "heading": "Event",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2026/2026-01-06-pinch-perfect-2026/pinch-perfect-2026.jpg",
  "imageWidth": 1280,
  "imageHeight": 720,
  "start": "2026-01-06T10:00:00.000",
  "end": "2026-01-11T20:00:00.000",
  "timezone": "Local Time",
  "status": "current",
  "isCurrent": true,
  "extraData": {
    "event": {
      "bonuses": [
        {
          "text": "2\u00d7 XP for catching Pok\u00e9mon",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/xp.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "Increased chance of encountering Shiny Corphish, Shiny Dwebble, and Shiny Clauncher",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/none.png",
          "imageWidth": 100,
          "imageHeight": 100
        }
      ],
      "bonusDisclaimers": [],
      "features": [
        "<strong>Klawf</strong>, the Ambush Pok\u00e9mon, will make its Pok\u00e9mon GO debut!",
        "Also, for the first time in Pok\u00e9mon GO, you\u2019ll be able to encounter Shiny Dhelmise\u2014if you\u2019re lucky!"
      ],
      "shinies": [],
      "customSections": {
        "spawns": {
          "paragraphs": [
            "You may encounter event-themed Pok\u00e9mon in the wild, including Corphish, Dwebble, Clauncher, and more.",
            "<strong>Note:</strong> It seems like Dwebble, Corphish, and Clauncher will appear more frequently in the wild during their corresponding Timed Research periods:",
            "You might even encounter Tirtouga and Archen!"
          ],
          "lists": [
            [
              "<strong>Dwebble:</strong> Tuesday, January 6, at 10:00 a.m. to Thursday, January 8, at 10:00 a.m. local time",
              "<strong>Corphish:</strong> Thursday, January 8, at 10:00 a.m. to Saturday, January 10, at 10:00 a.m. local time",
              "<strong>Clauncher:</strong> Saturday, January 10, at 10:00 a.m. to Sunday, January 11, at 8:00 p.m. local time"
            ],
            [
              "<div class=\"pkmn-list-img ground\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_027_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Sandshrew</div>",
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_072_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Tentacool</div>",
              "<div class=\"pkmn-list-img rock\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_095_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Onix</div>",
              "<div class=\"pkmn-list-img ground\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_111_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Rhyhorn</div>",
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_129_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Magikarp</div>",
              "<div class=\"pkmn-list-img ground\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_328_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Trapinch</div>",
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_341_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Corphish</div>",
              "<div class=\"pkmn-list-img ground\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_529_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Drilbur</div>",
              "<div class=\"pkmn-list-img bug\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_557_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Dwebble</div>",
              "<div class=\"pkmn-list-img rock\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_688_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Binacle</div>",
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_692_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Clauncher</div>"
            ],
            [
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_564_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Tirtouga</div>",
              "<div class=\"pkmn-list-img rock\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_566_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Archen</div>"
            ]
          ],
          "pokemon": []
        },
        "raids": {
          "paragraphs": [
            "The following Pok\u00e9mon will appear in raids."
          ],
          "lists": [
            [
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_341_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Corphish</div>",
              "<div class=\"pkmn-list-img bug\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_557_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Dwebble</div>",
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_692_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Clauncher</div>"
            ],
            [
              "<div class=\"pkmn-list-img ghost\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pm781.icon.png?v2\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Dhelmise</div>",
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pm977.icon.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Dondozo</div>"
            ]
          ],
          "pokemon": []
        },
        "research": {
          "paragraphs": [
            "Event-themed Field Research tasks will be available!",
            "The following Pok\u00e9mon will be available to encounter when you complete Field Research tasks!",
            "Three event-themed Timed Research focused on catching Pok\u00e9mon will be available throughout the event on the following schedule!",
            "Event-exclusive Timed Research",
            "<strong>Tuesday, January 6, at 10:00 a.m. to Thursday, January 8, 2026, at 10:00 a.m. local time</strong>",
            "Complete the research tasks to earn encounters with Dwebble and Klawf.",
            "Event-exclusive Timed Research",
            "<strong>Thursday, January 8, at 10:00 a.m. to Saturday, January 10, 2026, at 10:00 a.m. local time</strong>",
            "Complete the research tasks to earn encounters with Corphish and Klawf.",
            "Complete the research tasks to earn encounters with Clauncher and Klawf.",
            "Paid Timed Research",
            "For US$1.99 (or the equivalent pricing tier in your local currency), you\u2019ll be able to access event-exclusive Timed Research.",
            "Timed Research rewards include the following."
          ],
          "lists": [
            [
              "<span class=\"task\"> Catch 5 Pok\u00e9mon</span><div class=\"reward-list\"><span class=\"rewards-header\">POSSIBLE REWARDS</span><div class=\"reward \"><span class=\"reward-bubble resource-reward\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/items/Poke Ball.png\"><div class=\"quantity\">\u00d710</div></span><span class=\"reward-label\">Pok\u00e9 Ball <span>\u00d710</span></span></div><div class=\"reward \"><span class=\"reward-bubble resource-reward\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/items/Great Ball.png\"><div class=\"quantity\">\u00d75</div></span><span class=\"reward-label\">Great Ball <span>\u00d75</span></span></div><div class=\"reward \"><span class=\"reward-bubble resource-reward\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/items/Ultra Ball.png\"><div class=\"quantity\">\u00d73</div></span><span class=\"reward-label\">Ultra Ball <span>\u00d73</span></span></div></div>",
              "<span class=\"task\"> Catch 10 Pok\u00e9mon</span><div class=\"reward-list\"><span class=\"rewards-header\">POSSIBLE REWARDS</span><div class=\"reward \"><span class=\"reward-bubble water\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pokemon_icon_341_00.png\"><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"></span><span class=\"reward-label\"><span>Corphish</span></span><span class=\"cp-values water\"><span class=\"max-cp\"><div>Max CP</div> 527 </span><span class=\"min-cp\"><div>Min CP</div> 490 </span></span></div><div class=\"reward \"><span class=\"reward-bubble bug\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pokemon_icon_557_00.png\"><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"></span><span class=\"reward-label\"><span>Dwebble</span></span><span class=\"cp-values bug\"><span class=\"max-cp\"><div>Max CP</div> 524 </span><span class=\"min-cp\"><div>Min CP</div> 488 </span></span></div><div class=\"reward \"><span class=\"reward-bubble water\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pokemon_icon_692_00.png\"><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"></span><span class=\"reward-label\"><span>Clauncher</span></span><span class=\"cp-values water\"><span class=\"max-cp\"><div>Max CP</div> 466 </span><span class=\"min-cp\"><div>Min CP</div> 431 </span></span></div></div>",
              "<span class=\"task\"> Catch 15 Pok\u00e9mon</span><div class=\"reward-list\"><span class=\"rewards-header\">REWARD</span><div class=\"reward \"><span class=\"reward-bubble resource-reward\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/items/Stardust.png\"><div class=\"quantity\">\u00d71500</div></span><span class=\"reward-label\">Stardust <span>\u00d71500</span></span></div></div>",
              "<span class=\"task\"> Win a&nbsp;raid</span><div class=\"reward-list\"><span class=\"rewards-header\">POSSIBLE REWARDS</span><div class=\"reward \"><span class=\"reward-bubble ghost\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm781.icon.png?v2\"><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"></span><span class=\"reward-label\"><span>Dhelmise</span></span><span class=\"cp-values ghost\"><span class=\"max-cp\"><div>Max CP</div> 1264 </span><span class=\"min-cp\"><div>Min CP</div> 1206 </span></span></div><div class=\"reward \"><span class=\"reward-bubble rock\"><img class=\"reward-image\" src=\"https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm950.icon.png\"></span><span class=\"reward-label\"><span>Klawf</span></span><span class=\"cp-values rock\"><span class=\"max-cp\"><div>Max CP</div> 1030 </span><span class=\"min-cp\"><div>Min CP</div> 978 </span></span></div></div>"
            ],
            [
              "<div class=\"pkmn-list-img water\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_692_00.png\"></div><img class=\"shiny-icon\" src=\"https://cdn.leekduck.com/assets/img/icons/shiny-icon.png\" alt=\"shiny\"><div class=\"pkmn-name\">Clauncher</div>",
              "<div class=\"pkmn-list-img rock\"><img src=\"https://cdn.leekduck.com/assets/img/pokemon_icons/pm950.icon.png\"></div><div class=\"pkmn-name\">Klawf</div>"
            ],
            [
              "Two Premium Battles Passes",
              "Encounters with Corphish, Dwebble, Clauncher, Dhelmise, and Klawf",
              "And more!"
            ]
          ],
          "pokemon": []
        },
        "sales": {
          "paragraphs": [
            "The <strong>Pinch Perfect Ultra Ticket Box</strong> will be available for <strong>US$1.99</strong> (or the equivalent pricing tier in your local currency) and includes an event ticket and five Ultra Balls!",
            "Check it out in the <a href=\"https://pokemongo.com/store\">Pok\u00e9mon GO Web Store</a>!"
          ],
          "lists": [],
          "pokemon": []
        }
      }
    }
  }
}
```

**Key Fields:**
- `extraData.event.bonuses`: Active bonuses
- `extraData.event.features`: Event highlights
- `extraData.event.shinies`: Available shinies
- `extraData.event.customSections`: Nested objects with spawns, raids, eggs, research, sales information

### go-battle-league

**Example: Ultra League and Sunshine Cup: Great League Edition | Precious Paths**

```json
{
  "eventID": "gbl-precious-paths_ultra-league_sunshine-cup-great-league-edition",
  "name": "Ultra League and Sunshine Cup: Great League Edition | Precious Paths",
  "eventType": "go-battle-league",
  "heading": "GO Battle League",
  "image": "https://cdn.leekduck.com/assets/img/events/go-battle-league-ultra-league-machamp-swampert.jpg",
  "imageWidth": 1024,
  "imageHeight": 576,
  "start": "2026-01-06T21:00:00.000Z",
  "end": "2026-01-13T21:00:00.000Z",
  "timezone": null,
  "status": "current",
  "isCurrent": true,
  "extraData": null
}
```

### go-pass

**Example: GO Pass: January**

```json
{
  "eventID": "go-pass-january-2026",
  "name": "GO Pass: January",
  "eventType": "go-pass",
  "heading": "GO Pass",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2026/2026-01-06-go-pass-january-2026/go-pass-january-2026.jpg",
  "imageWidth": 1920,
  "imageHeight": 1080,
  "start": "2026-01-06T10:00:00.000",
  "end": "2026-02-03T10:00:00.000",
  "timezone": "Local Time",
  "status": "current",
  "isCurrent": true,
  "extraData": null
}
```

### max-battles

**Example: Dynamax Weekend**

```json
{
  "eventID": "dynamax-weekend-february-2026",
  "name": "Dynamax Weekend",
  "eventType": "max-battles",
  "heading": "Max Battles",
  "image": "https://cdn.leekduck.com/assets/img/events/events-default-img.jpg",
  "imageWidth": 1024,
  "imageHeight": 512,
  "start": "2026-01-31T10:00:00.000",
  "end": "2026-02-01T20:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": null
}
```

### max-mondays

**Example: Dynamax Caterpie during Max Monday**

```json
{
  "eventID": "max-mondays-2026-01-12",
  "name": "Dynamax Caterpie during Max Monday",
  "eventType": "max-mondays",
  "heading": "Max Mondays",
  "image": "https://cdn.leekduck.com/assets/img/events/max-battles-kanto.jpg",
  "imageWidth": 1920,
  "imageHeight": 1080,
  "start": "2026-01-12T18:00:00.000",
  "end": "2026-01-12T19:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": {
    "maxmondays": {
      "featured": {
        "name": "Caterpie",
        "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_010_00.png",
        "imageWidth": 256,
        "imageHeight": 256,
        "canBeShiny": true
      },
      "bonus": "January 12, 2026"
    }
  }
}
```

### pokemon-go-tour

**Example: Pokémon GO Tour: Kalos - Tainan 2026**

```json
{
  "eventID": "pokemon-go-tour-kalos-tainan-2026",
  "name": "Pok\u00e9mon GO Tour: Kalos - Tainan 2026",
  "eventType": "pokemon-go-tour",
  "heading": "Pok\u00e9mon GO Tour",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2026/2026-02-20-pokemon-go-tour-kalos-tainan-2026/pokemon-go-tour-kalos-tainan-2026.jpg",
  "imageWidth": 1280,
  "imageHeight": 720,
  "start": "2026-02-20T01:00:00.000Z",
  "end": "2026-02-22T09:00:00.000Z",
  "timezone": null,
  "status": "upcoming",
  "isCurrent": false,
  "extraData": {
    "gotour": {
      "eventInfo": {
        "name": "Pok\u00e9mon GO Tour: Kalos - Tainan\u00a02026",
        "location": "",
        "dates": "",
        "time": "",
        "ticketPrice": null,
        "ticketUrl": ""
      },
      "exclusiveBonuses": [],
      "ticketAddOns": [],
      "whatsNew": [],
      "habitats": {
        "General": {
          "info": "Event-themed Pok\u00e9mon will appear in three unique habitats during the event. When you purchase your ticket, you\u2019ll select which habitat you\u2019d like to start your day in. You\u2019ll be able to visit all three habitats during your event day! A map of the venue, including the habitat locations, will be available closer to the event.",
          "spawns": []
        }
      },
      "incenseEncounters": [],
      "eggs": {
        "2km": [],
        "5km": [
          {
            "name": "Larvitar ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_246_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Bagon ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_371_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Beldum ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_374_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Gible ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_443_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Red Flower Flab\u00e9b\u00e9 ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm669.fRED.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Yellow Flower Flab\u00e9b\u00e9 ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm669.fYELLOW.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Orange Flower Flab\u00e9b\u00e9 ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm669.fORANGE.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Blue Flower Flab\u00e9b\u00e9 ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm669.fBLUE.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "White Flower Flab\u00e9b\u00e9 ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm669.fWHITE.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Kangaskhan ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_115_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Heracross ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_214_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Hawlucha ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_701_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Klefki ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_707_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          }
        ],
        "7km": [],
        "10km": []
      },
      "raids": {
        "oneStar": [],
        "threeStar": [],
        "fiveStar": [],
        "mega": []
      },
      "research": {
        "field": [],
        "special": [],
        "timed": [],
        "masterwork": []
      },
      "shinyDebuts": [],
      "shinies": [
        {
          "name": "Honedge",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_679_00_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Hawlucha",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_701_00_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Klefki",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_707_00_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        }
      ],
      "sales": [
        "A limited number of Pok\u00e9mon GO Tour: Kalos T-shirts will be available for purchase at the event! By purchasing a Pok\u00e9mon GO Tour: Kalos T-shirt in person, you\u2019ll also receive a voucher card with a redemption code for an exclusive matching GO Tour T-shirt avatar item.",
        "<img class=\"center-image\" src=\"https://cdn.leekduck.com/assets/img/events/article-images/2026/2026-02-20-pokemon-go-tour-kalos-tainan-2026/go-tour-kalos-shirt.webp\" loading=\"lazy\">",
        "Preorders will not be available for Pok\u00e9mon GO Tour: Kalos \u2013 Tainan. Quantities are limited. No returns or exchanges will be allowed."
      ],
      "costumedPokemon": []
    }
  }
}
```

**Key Fields:**
- `extraData.gotour.eventInfo`: Location, dates, pricing
- `extraData.gotour.habitats`: Rotating spawn habitats with schedules
- `extraData.gotour.raids`: Raid lineup by tier
- `extraData.gotour.shinyDebuts`: Pokémon with shiny forms debuting

### pokemon-spotlight-hour

**Example: Mareep Spotlight Hour**

```json
{
  "eventID": "pokemonspotlighthour2026-01-13",
  "name": "Mareep Spotlight Hour",
  "eventType": "pokemon-spotlight-hour",
  "heading": "Pok\u00e9mon Spotlight Hour",
  "image": "https://cdn.leekduck.com/assets/img/events/pokemonspotlighthour.jpg",
  "imageWidth": 1024,
  "imageHeight": 512,
  "start": "2026-01-13T18:00:00.000",
  "end": "2026-01-13T19:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": {
    "spotlight": {
      "name": "Mareep",
      "canBeShiny": true,
      "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_179_00.png",
      "imageWidth": 256,
      "imageHeight": 256,
      "bonus": "2\u00d7 Evolution XP",
      "list": [
        {
          "name": "Mareep",
          "canBeShiny": true,
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_179_00.png",
          "imageWidth": 256,
          "imageHeight": 256
        }
      ]
    }
  }
}
```

**Key Fields:**
- `extraData.spotlight.name`: Featured Pokémon name
- `extraData.spotlight.bonus`: Active bonus during hour (e.g., "2× Evolution XP", "2× Transfer Candy")
- `extraData.spotlight.canBeShiny`: Shiny availability

### pokestop-showcase

**Example: Klawf and Dhelmise PokéStop Showcases**

```json
{
  "eventID": "pokemon-showcase-klawf-dhelmise",
  "name": "Klawf and Dhelmise Pok\u00e9Stop Showcases",
  "eventType": "pokestop-showcase",
  "heading": "Pok\u00e9Stop Showcase",
  "image": "https://cdn.leekduck.com/assets/img/events/pokestop-showcases-default.jpg",
  "imageWidth": 1280,
  "imageHeight": 720,
  "start": "2026-01-10T10:00:00.000",
  "end": "2026-01-11T20:00:00.000",
  "timezone": "Local Time",
  "status": "current",
  "isCurrent": true,
  "extraData": {
    "pokestopshowcase": {
      "featured": [
        {
          "name": "Dhelmise",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm781.icon.png?v2",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Klawf",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm950.icon.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        }
      ],
      "description": "There will be Pok\u00e9Stop Showcases featuring Klawf and Dhelmise. Top the leaderboard to win rewards including Premium Items!DhelmiseKlawf"
    }
  }
}
```

### raid-battles

**Example: Genesect (Burn Drive), Genesect (Chill Drive) in 5-star Raid Battles**

```json
{
  "eventID": "genesect-burn-drive-genesect-chill-drive-in-5-star-raid-battles-january-2026",
  "name": "Genesect (Burn Drive), Genesect (Chill Drive) in 5-star Raid Battles",
  "eventType": "raid-battles",
  "heading": "Raid Battles",
  "image": "https://cdn.leekduck.com/assets/img/events/genesect.jpg",
  "imageWidth": 660,
  "imageHeight": 330,
  "start": "2026-01-05T10:00:00.000",
  "end": "2026-01-16T10:00:00.000",
  "timezone": "Local Time",
  "status": "current",
  "isCurrent": true,
  "extraData": null
}
```

### raid-day

**Example: Kyurem Fusion Raid Day**

```json
{
  "eventID": "kyurem-fusion-raid-day",
  "name": "Kyurem Fusion Raid Day",
  "eventType": "raid-day",
  "heading": "Raid Day",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2026/2026-01-10-kyurem-fusion-raid-day/fusion-raid-day.jpg",
  "imageWidth": 1280,
  "imageHeight": 720,
  "start": "2026-01-10T14:00:00.000",
  "end": "2026-01-10T17:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": {
    "raidday": {
      "featured": [
        {
          "name": "Reshiram",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_643_00.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": true
        },
        {
          "name": "Zekrom",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_644_00.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": true
        },
        {
          "name": "Kyurem (Black)",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_646_13.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Kyurem (White)",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_646_12.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        }
      ],
      "featuredAttacks": [],
      "raids": {
        "fiveStar": [],
        "mega": [],
        "other": []
      },
      "shinies": [
        {
          "name": "Reshiram",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_643_00_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Zekrom",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_644_00_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Kyurem",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_646_11_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Kyurem (Black)",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_646_13_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        },
        {
          "name": "Kyurem (White)",
          "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_646_12_shiny.png",
          "imageWidth": 256,
          "imageHeight": 256,
          "canBeShiny": false
        }
      ],
      "bonuses": [
        {
          "text": "Reshiram, Zekrom, Black Kyurem* and White Kyurem* will appear more frequently in raids, and these Raid Bosses will alternate each half hour during event hours. Reshiram and Black Kyurem will appear in raids first, followed by Zekrom and White Kyurem",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/none.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "Receive up to five additional free Raid Passes from spinning Gym Photo Discs (for a total of six)",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/freeraidpass.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "Increased chance to encounter Shiny Reshiram, Shiny Zekrom, and Shiny Kyurem in raids",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/none.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "The Remote Raid Pass limit will increase to 20 from Friday, January 9, at 5:00 p.m. to Saturday, January 10, 2026, at 8:00 p.m. PST",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/remoteraidpass.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "Eight additional Raid Passes from spinning Gym Photo Discs (for a daily total of 14)",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/freeraidpass.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "Increased chance to get Rare Candy XL from Raid Battles",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/rarecandyxl.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "50% more XP from Raid Battles",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/xp.png",
          "imageWidth": 100,
          "imageHeight": 100
        },
        {
          "text": "2\u00d7 Stardust from Raid Battles",
          "image": "https://cdn.leekduck.com/assets/img/events/bonuses/stardust2x.png",
          "imageWidth": 100,
          "imageHeight": 100
        }
      ],
      "ticketBonuses": [],
      "ticketPrice": null,
      "alternationPattern": "",
      "specialMechanics": []
    }
  }
}
```

**Key Fields:**
- `extraData.raidday.featured`: Raid bosses appearing in raids
- `extraData.raidday.bonuses`: Event-specific bonuses (extra passes, XP/Stardust multipliers)
- `extraData.raidday.shinies`: Shiny forms available
- `extraData.raidday.alternationPattern`: Description of rotating raid bosses (if applicable)

### raid-hour

**Example: Genesect (Burn Drive), Genesect (Chill Drive) Raid Hour**

```json
{
  "eventID": "raidhour20260114",
  "name": "Genesect (Burn Drive), Genesect (Chill Drive) Raid Hour",
  "eventType": "raid-hour",
  "heading": "Raid Hour",
  "image": "https://cdn.leekduck.com/assets/img/events/raidhour.jpg",
  "imageWidth": 960,
  "imageHeight": 480,
  "start": "2026-01-14T18:00:00.000",
  "end": "2026-01-14T19:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": {
    "raidhour": {
      "featured": {
        "name": "Genesect (Burn Drive), Genesect (Chill Drive) Raid\u00a0Hour",
        "image": "",
        "canBeShiny": false
      },
      "canBeShiny": false
    }
  }
}
```

### research

**Example: Masterwork Research: A Precious Catch**

```json
{
  "eventID": "masterwork-research-a-precious-catch",
  "name": "Masterwork Research: A Precious Catch",
  "eventType": "research",
  "heading": "Research",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2025/2025-12-02-masterwork-research-a-precious-catch/masterball.jpg",
  "imageWidth": 1280,
  "imageHeight": 720,
  "start": "2025-12-02T10:00:00.000",
  "end": "2026-03-03T10:00:00.000",
  "timezone": null,
  "status": "current",
  "isCurrent": true,
  "extraData": {
    "research": {
      "name": "Masterwork Research: A Precious\u00a0Catch",
      "researchType": "masterwork",
      "isPaid": true,
      "price": 7.99,
      "description": "Ticket-exclusive Masterwork Research",
      "tasks": [
        {
          "step": 1,
          "name": "Masterwork Research: A Precious Catch (1/?)",
          "tasks": [
            {
              "text": "Catch 250 Pok\u00e9mon",
              "reward": {
                "text": "Pok\u00e9 Ball \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Poke%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Use 150 Berries to help catch Pok\u00e9mon",
              "reward": {
                "text": "Razz Berry \u00d715",
                "image": "https://cdn.leekduck.com/assets/img/items/Razz%20Berry.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Make 100 Nice Curveball Throws",
              "reward": {
                "text": "Stardust \u00d72500",
                "image": "https://cdn.leekduck.com/assets/img/items/Stardust.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            }
          ],
          "rewards": [
            {
              "text": "2500 XP",
              "image": "https://cdn.leekduck.com/assets/img/icons/xp.png",
              "imageWidth": 256,
              "imageHeight": 257
            },
            {
              "text": "Cetoddle",
              "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm974.icon.png",
              "imageWidth": 195,
              "imageHeight": 145
            }
          ]
        },
        {
          "step": 2,
          "name": "Masterwork Research: A Precious Catch (2/?)",
          "tasks": [
            {
              "text": "Catch 300 Pok\u00e9mon",
              "reward": {
                "text": "Pok\u00e9 Ball \u00d725",
                "image": "https://cdn.leekduck.com/assets/img/items/Poke%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Make 100 Great Curveball Throws",
              "reward": {
                "text": "Pinap Berry \u00d715",
                "image": "https://cdn.leekduck.com/assets/img/items/Pinap%20Berry.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Transfer 50 Pok\u00e9mon",
              "reward": {
                "text": "Great Ball \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Great%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            }
          ],
          "rewards": [
            {
              "text": "3500 XP",
              "image": "https://cdn.leekduck.com/assets/img/icons/xp.png",
              "imageWidth": 256,
              "imageHeight": 257
            },
            {
              "text": "Primeape",
              "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pokemon_icon_057_00.png",
              "imageWidth": 158,
              "imageHeight": 166
            }
          ]
        },
        {
          "step": 3,
          "name": "Masterwork Research: A Precious Catch (3/?)",
          "tasks": [
            {
              "text": "Catch 350 Pok\u00e9mon",
              "reward": {
                "text": "Great Ball \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Great%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Defeat 25 Team GO Rocket members",
              "reward": {
                "text": "Charged TM \u00d75",
                "image": "https://cdn.leekduck.com/assets/img/items/Charged%20TM.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Catch 75 different species of Pok\u00e9mon",
              "reward": {
                "text": "Pinap Berry \u00d715",
                "image": "https://cdn.leekduck.com/assets/img/items/Pinap%20Berry.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            }
          ],
          "rewards": [
            {
              "text": "4500 XP",
              "image": "https://cdn.leekduck.com/assets/img/icons/xp.png",
              "imageWidth": 256,
              "imageHeight": 257
            },
            {
              "text": "Bisharp",
              "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pokemon_icon_625_00.png",
              "imageWidth": 123,
              "imageHeight": 215
            }
          ]
        },
        {
          "step": 4,
          "name": "Masterwork Research: A Precious Catch (4/?)",
          "tasks": [
            {
              "text": "Visit Pok\u00e9Stops on 7 different days",
              "reward": {
                "text": "2500 XP",
                "image": "https://cdn.leekduck.com/assets/img/icons/xp.png",
                "imageWidth": 256,
                "imageHeight": 257
              }
            },
            {
              "text": "Catch a Pok\u00e9mon on 7 different days",
              "reward": {
                "text": "Stardust \u00d72500",
                "image": "https://cdn.leekduck.com/assets/img/items/Stardust.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Hatch 20 Eggs",
              "reward": {
                "text": "",
                "image": "",
                "imageWidth": null,
                "imageHeight": null
              }
            },
            {
              "text": "Catch 75 Pok\u00e9mon in a single day",
              "reward": {
                "text": "Ultra Ball \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Ultra%20Ball.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Make 50 Excellent Throws",
              "reward": {
                "text": "Max Revive \u00d720",
                "image": "https://cdn.leekduck.com/assets/img/items/Max%20Revive.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            },
            {
              "text": "Catch 500 Pok\u00e9mon",
              "reward": {
                "text": "Silver Pinap Berry \u00d73",
                "image": "https://cdn.leekduck.com/assets/img/items/Silver%20Pinap%20Berry.png",
                "imageWidth": 256,
                "imageHeight": 256
              }
            }
          ],
          "rewards": [
            {
              "text": "5000 XP",
              "image": "https://cdn.leekduck.com/assets/img/icons/xp.png",
              "imageWidth": 256,
              "imageHeight": 257
            },
            {
              "text": "\u00d75000",
              "image": "https://cdn.leekduck.com/assets/img/items/Stardust.png",
              "imageWidth": 256,
              "imageHeight": 256
            },
            {
              "text": "\u00d71",
              "image": "https://cdn.leekduck.com/assets/img/items/Master%20Ball.png",
              "imageWidth": 256,
              "imageHeight": 256
            }
          ]
        }
      ],
      "rewards": [
        "One Master Ball",
        "XP and Stardust",
        "Encounters with event-themed Pok\u00e9mon such as Cetoddle",
        "And more!"
      ],
      "encounters": [],
      "promoCodes": [],
      "expires": false,
      "webStoreInfo": "Trainers can purchase the Master Ball Ultra Ticket Box for the Masterwork Research: A Precious Catch and a bonus of 3 Rare Candies. "
    }
  }
}
```

### season

**Example: Precious Paths**

```json
{
  "eventID": "season-21-precious-paths",
  "name": "Precious Paths",
  "eventType": "season",
  "heading": "Season",
  "image": "https://cdn.leekduck.com/assets/img/events/article-images/2025/2025-12-02-season-21-precious-paths/season-21-precious-paths.jpg",
  "imageWidth": 1277,
  "imageHeight": 675,
  "start": "2025-12-02T10:00:00.000",
  "end": "2026-03-03T10:00:00.000",
  "timezone": "Local Time",
  "status": "current",
  "isCurrent": true,
  "extraData": {
    "season": {
      "name": "Precious\u00a0Paths",
      "bonuses": [],
      "spawns": [],
      "eggs": {
        "2km": [
          {
            "name": "Bulbasaur ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_001_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Charmander ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_004_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Squirtle ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_007_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Chikorita ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_152_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Cyndaquil ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_155_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Totodile ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_158_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Treecko ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_252_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Torchic ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_255_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Mudkip ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_258_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Turtwig ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_387_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Chimchar ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_390_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Piplup ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_393_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Snivy ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_495_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Tepig ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_498_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Oshawott ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_501_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Chespin ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_650_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Fennekin ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_653_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Froakie ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_656_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Rowlet ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm722.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Litten ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm725.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Popplio ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm728.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Grookey ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm810.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          },
          {
            "name": "Scorbunny ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm813.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          },
          {
            "name": "Sobble ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm816.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          },
          {
            "name": "Sprigatito ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm906.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Fuecoco ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm909.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Quaxly ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm912.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Cleffa ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_173_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Igglybuff ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_174_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Smoochum ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_238_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Meditite ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_307_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Bonsly ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_438_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Petilil ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_548_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Ducklett ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_580_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Litleo ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_667_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Bergmite ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm712.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Wimpod ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm767.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Smoliv ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm928.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Munchlax ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_446_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Audino ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_531_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Elgyem ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_605_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Inkay ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_686_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Crabrawler ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm739.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Komala ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm775.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Togedemaru ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm777.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Snom ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm872.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          },
          {
            "name": "Indeedee (Male) ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm876.fMALE.icon.png?v2",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Indeedee (Female) ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm876.fFEMALE.icon.png?v2",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Skarmory ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_227_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Bonsly ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_438_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Riolu ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_447_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Tirtouga ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_564_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Archen ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_566_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Alolan Diglett ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_050_61.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Galarian Meowth ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_052_31.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Alolan Geodude ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_074_61.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Galarian Corsola ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm222.fGALARIAN.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Galarian Zigzagoon ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_263_31.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Galarian Darumaka ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_554_31.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Galarian Stunfisk ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_618_31.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          }
        ],
        "5km": [],
        "7km": [],
        "10km": [],
        "12km": [],
        "route": [
          {
            "name": "Hisuian Growlithe ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm58.fHISUIAN.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Galarian Slowpoke ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_079_31.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Hisuian Sneasel ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm215.fHISUIAN.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Galarian Corsola ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm222.fGALARIAN.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Beldum ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_374_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Pawniard ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_624_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Honedge ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_679_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          },
          {
            "name": "Carbink ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm703.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Jangmo-o ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm782.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Toxel ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm848.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Dreepy ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm885.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          },
          {
            "name": "Charcadet ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm935.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Tinkatink ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm957.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          },
          {
            "name": "Frigibax ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm996.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Beldum ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_374_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Gible ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pokemon_icon_443_00.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Turtonator ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm776.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Drampa ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm780.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": true
          },
          {
            "name": "Dreepy ",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm885.icon.png",
            "imageWidth": 256,
            "imageHeight": 256,
            "canBeShiny": false
          }
        ],
        "adventure": []
      },
      "researchBreakthrough": [],
      "specialResearch": [
        "Season Special Research",
        "The Precious Paths Special Research will be available to claim from Tuesday, December 2, 2025, at 10:00 a.m. to Tuesday, March 3, 2026, at 9:59 a.m. local time.",
        "New parts of the Special Research will unlock over the course of the Season, so make sure to keep an eye on your research tab!",
        "Complete the Season Special Research to earn encounters with Sprigatito, Fuecoco, and Quaxly!",
        "Ticket-exclusive Masterwork Research",
        "For US$7.99 (or the equivalent pricing tier in your local currency), Trainers will receive a Special Research granting a Master Ball as one of the rewards.",
        "Receiving a rare and powerful Master Ball is a special occasion\u2014think wisely about how you\u2019ll use it, and keep a lookout for future opportunities in Pok\u00e9mon GO to acquire more!"
      ],
      "masterworkResearch": [],
      "communityDays": [],
      "features": [
        "The Precious Paths Special Research will be available to claim from Tuesday, December 2, 2025, at 10:00 a.m. to Tuesday, March 3, 2026, at 9:59 a.m. local time.",
        "New parts of the Special Research will unlock over the course of the Season, so make sure to keep an eye on your research tab!",
        "<img class=\"center-image\" src=\"https://cdn.leekduck.com/assets/img/events/article-images/2025/2025-12-02-season-21-precious-paths/precious-paths-special-research.png\" loading=\"lazy\">",
        "For US$7.99 (or the equivalent pricing tier in your local currency), Trainers will receive a Special Research granting a Master Ball as one of the rewards.",
        "Receiving a rare and powerful Master Ball is a special occasion\u2014think wisely about how you\u2019ll use it, and keep a lookout for future opportunities in Pok\u00e9mon GO to acquire more!",
        "<img class=\"center-image\" src=\"https://cdn.leekduck.com/assets/img/events/article-images/2025/2025-12-02-season-21-precious-paths/masterball-masterwork-research-precious-catch.jpg\" loading=\"lazy\">",
        "New Pok\u00e9mon have been spotted in Pok\u00e9mon GO! Sudowoodo wearing holiday attire, Charjabug wearing holiday attire, Clobbopus, Grapploct, and more make their debuts throughout the season!",
        "<img class=\"center-image\" src=\"https://cdn.leekduck.com/assets/img/events/article-images/2025/2025-12-02-season-21-precious-paths/new-pokemon-precious-paths.png\" loading=\"lazy\">",
        "New Dynamax and Gigantamax Pok\u00e9mon make their Pok\u00e9mon GO debut! Challenge and catch Dynamax Hitmonlee, Dynamax Hitmonchan, Gigantamax Meowth, and more this Season!",
        "<img class=\"center-image\" src=\"https://cdn.leekduck.com/assets/img/events/article-images/2025/2025-12-02-season-21-precious-paths/max-pokemon-debuts.jpg\" loading=\"lazy\">",
        "Mark your calendar! Precious Paths will have Community Day events on the following dates:",
        "GO Pass, a limited-time progression track, will be available during Precious Paths! Complete Pass Tasks to earn GO Points to increase your rank and earn rewards.",
        "The GO Pass Deluxe, a paid version of GO Pass, will be available for Trainers looking for upgraded rewards and faster progression.",
        "<strong>Tier 1 (Rank 25):</strong>",
        "<strong>Tier 2 (Rank 50):</strong>",
        "<strong>Tier 3 (Rank 75):</strong>",
        "<strong>Tier 1 (Upgraded):</strong>",
        "The GO Battle League returns as part of Precious Paths! Get your Pok\u00e9mon ready to battle in competitions such as the Holiday Cup, Sunshine Cup, Love Cup, and more!",
        "Visit the <a href=\"https://pokemongolive.com/post/go-battle-league-precious-paths\">GO Battle League web page</a> for the schedule and more info!",
        "Pok\u00e9mon GO Tour: Kalos lands in 2026! Experience two in-person Pok\u00e9mon GO events, then join millions of Trainers around the world for the global finale!",
        "Complete Field Research to unlock Research Breakthrough encounters with one of the following Pok\u00e9mon.",
        "Be on the lookout for Pok\u00e9Stop Showcases throughout Pok\u00e9mon GO: Precious Paths. Log in to find out which Pok\u00e9mon you can show off!"
      ],
      "goBattleLeague": "",
      "goPass": [],
      "pokemonDebuts": [],
      "maxPokemonDebuts": []
    }
  }
}
```

**Key Fields:**
- `extraData.season.name`: Season name
- `extraData.season.bonuses`: Season-long active bonuses
- `extraData.season.spawns`: Biome/habitat spawn changes
- `extraData.season.eggs`: Complete egg pool by distance
- `extraData.season.researchBreakthrough`: 7-day streak reward Pokémon
- `extraData.season.communityDays`: Scheduled Community Days

### team-go-rocket

**Example: Precious Pals: Taken Over**

```json
{
  "eventID": "precious-pals-taken-over-2026",
  "name": "Precious Pals: Taken Over",
  "eventType": "team-go-rocket",
  "heading": "Team GO Rocket",
  "image": "https://cdn.leekduck.com/assets/img/events/events-default-img.jpg",
  "imageWidth": 1024,
  "imageHeight": 512,
  "start": "2026-01-23T10:00:00.000",
  "end": "2026-01-25T20:00:00.000",
  "timezone": "Local Time",
  "status": "upcoming",
  "isCurrent": false,
  "extraData": null
}
```

## Important Notes

1. **Historical Data**: Events are removed from the dataset once they end (no historical archive)
2. **Timezone Handling**: 
   - **Local Time Events** (`timezone: "Local Time"`): Times apply to player's local timezone
   - **UTC Events** (`timezone: null`): Times are applied globally at the same moment
3. **extraData Structure**: Varies significantly by `eventType`; consuming applications must handle missing fields gracefully
4. **Multiple Events**: Multiple events can be active simultaneously (e.g., season + weekly event + spotlight hour)
5. **Dynamic Structure**: The structure shown here reflects actual live data and may contain all available fields for each event type

## Field Availability

Not all events of the same type will have identical extraData structures. Fields may be omitted when not applicable to a specific event. Always check for field existence before accessing nested properties.

## Data Updates

The events feed is regularly updated to reflect:
- New event announcements
- Status changes (upcoming → current)
- Removal of expired events
- Updates to event details and extraData

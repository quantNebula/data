## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/raids.min.json

## JSON Example

```json
{
    "name": "Genesect (Burn)",
    "tier": "5-Star Raids",
    "canBeShiny": true,
    "types": [
        {
            "name": "bug",
            "image": "https://leekduck.com/assets/img/types/bug.png",
            "imageWidth": 32,
            "imageHeight": 32
        },
        {
            "name": "steel",
            "image": "https://leekduck.com/assets/img/types/steel.png",
            "imageWidth": 32,
            "imageHeight": 32
        }
    ],
    "combatPower": {
        "normal": { "min": 1833, "max": 1916 },
        "boosted": { "min": 2292, "max": 2395 }
    },
    "boostedWeather": [
        {
            "name": "rainy",
            "image": "https://leekduck.com/assets/img/weather/rainy.png",
            "imageWidth": 32,
            "imageHeight": 32
        },
        {
            "name": "snow",
            "image": "https://leekduck.com/assets/img/weather/snowy.png",
            "imageWidth": 32,
            "imageHeight": 32
        }
    ],
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons/pm649.fBURN.icon.png",
    "imageWidth": 256,
    "imageHeight": 256
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Raid boss Pokémon name, may include forms (e.g., "Genesect (Burn)") |
| `tier` | string | Raid difficulty: "1-Star Raids", "3-Star Raids", "5-Star Raids", or "Mega Raids" |
| `canBeShiny` | boolean | Whether shiny form can be encountered |
| `types` | array | Pokémon types (1-2 elements) |
| `types[].name` | string | Type name (lowercase): "normal", "fire", "water", "electric", "grass", "ice", "fighting", "poison", "ground", "flying", "psychic", "bug", "rock", "ghost", "dragon", "dark", "steel", "fairy" |
| `types[].image` | string | Type icon URL |
| `types[].imageWidth` | integer | Type icon width in pixels |
| `types[].imageHeight` | integer | Type icon height in pixels |
| `combatPower.normal.min` | integer | Minimum CP at base level (20 for 5-Star/Mega, 15 for 1/3-Star) |
| `combatPower.normal.max` | integer | Maximum CP at base level |
| `combatPower.boosted.min` | integer | Minimum CP when weather-boosted (25 for 5-Star/Mega, 20 for 1/3-Star) |
| `combatPower.boosted.max` | integer | Maximum CP when weather-boosted |
| `boostedWeather` | array | Weather conditions that boost this Pokémon |
| `boostedWeather[].name` | string | Weather name: "sunny", "rainy", "partly cloudy", "cloudy", "windy", "snow", or "fog" |
| `boostedWeather[].image` | string | Weather icon URL |
| `boostedWeather[].imageWidth` | integer | Weather icon width in pixels |
| `boostedWeather[].imageHeight` | integer | Weather icon height in pixels |
| `image` | string | Pokémon icon URL |
| `imageWidth` | integer | Pokémon icon width in pixels |
| `imageHeight` | integer | Pokémon icon height in pixels |

## Data Structure

The endpoint returns a flat array of raid boss objects representing all currently active raid bosses across all tiers. During events, this list expands with featured bosses. Outside major events, a standard rotation is maintained.

## Raid Tiers

**1-Star Raids**:
- Entry-level difficulty, soloable by most trainers
- Base encounter level: 15
- Weather-boosted level: 20
- Typically common or easily accessible Pokémon

**3-Star Raids**:
- Moderate difficulty, may require 2-3 trainers or strong solo team
- Base encounter level: 15
- Weather-boosted level: 20
- Features evolved forms, regional exclusives, and rotating selections

**5-Star Raids** (Legendary Raids):
- High difficulty, typically requires 3-5+ trainers
- Base encounter level: 20
- Weather-boosted level: 25
- Features Legendary and Mythical Pokémon
- Limited-time availability (usually 1-3 weeks)

**Mega Raids**:
- Similar difficulty to 5-Star
- Base encounter level: 20
- Weather-boosted level: 25
- Defeating grants Mega Energy for that species
- Catching is not possible; energy only

**Shadow Raids**:
- Feature Shadow versions of Legendary Pokémon
- Higher difficulty than standard 5-Star raids
- Require Shadow Raid Passes (purple)
- Shadow Pokémon have different CP ranges

## Combat Power (CP) Ranges

CP ranges indicate the possible values when catching the raid boss:

**Normal (Base Level)**:
- For 5-Star/Mega: Level 20 (100% IV determines max CP)
- For 1-Star/3-Star: Level 15

**Boosted (Weather)**:
- For 5-Star/Mega: Level 25 (+5 levels from weather)
- For 1-Star/3-Star: Level 20 (+5 levels from weather)

The `min` value represents 10/10/10 IVs (minimum possible from raids).
The `max` value represents 15/15/15 IVs (perfect, 100%).

CP at encounter is determined by: Species base stats + Level + Individual Values (IVs)

## Weather Boost Mechanics

`boostedWeather` lists in-game weather conditions that:
1. Make the boss more difficult (increased CP in raid battle)
2. Increase catch level from 20→25 or 15→20
3. Guarantee minimum 10/10/10 IVs (same as non-boosted)
4. Provide extra rewards (Stardust, more Premier Balls)

**Weather Type Mappings**:
- **Sunny**: Boosts Fire, Grass, Ground
- **Rainy**: Boosts Water, Electric, Bug  
- **Partly Cloudy**: Boosts Normal, Rock
- **Cloudy**: Boosts Fairy, Fighting, Poison
- **Windy**: Boosts Dragon, Flying, Psychic
- **Snow**: Boosts Ice, Steel
- **Fog**: Boosts Dark, Ghost

Weather determined by player's real-world location and conditions.

## Type Information

Each `types` array entry provides:
- Type name (lowercase, standardized)
- Type badge icon (32×32px)
- Used for weakness/resistance calculations
- Order matters: first type is primary

Pokémon can have 1-2 types. Dual-typing affects:
- Damage multipliers (resistances stack)
- Weather boost eligibility
- Battle effectiveness

## Shiny Availability

`canBeShiny: true` means:
- Shiny variant can be encountered after defeating boss
- Shiny rate typically ~1/20 for Legendary raids
- Shiny rate ~1/64 for other tiers
- Shiny Pokémon are always caught (100% catch rate)
- First encounter of new shiny release often has boosted rates

## Image Assets

Main Pokémon image (`image`, 256×256px):
- High-resolution icon for display
- May include form indicators (e.g., "pm649.fBURN.icon.png" for Genesect Burn Drive)

Type images (32×32px):
- Standardized type badges
- Consistent across all Pokémon

Weather images (32×32px):
- Weather condition icons
- Match in-game weather display

## Raid Rotation Patterns

**5-Star Legendary Rotation**:
- Typically 1-3 weeks per boss
- May feature multiple forms simultaneously
- Special events may extend duration
- Sometimes alternates (half-hour rotations on Raid Days)

**Mega Raid Rotation**:
- Usually 1-2 weeks per Mega Evolution
- Cycles through available Mega forms
- Overlaps with 5-Star rotation

**1-Star and 3-Star Pools**:
- More stable, change with seasons/events
- Feature newer releases and event-themed Pokémon
- Used to introduce new species

## Important Notes

- Raid data updates reflect current active bosses only
- During events, temporary bosses are added; removed when event ends
- Shadow Raid bosses are tracked separately in raid-battles events
- CP values assume optimal IV spread; actual catches may vary within range
- Mega Raids don't provide encounters, only Mega Energy (no CP ranges needed)
- Boss availability varies by time of day: 5-Star/Mega raids more common during peak hours
- Remote Raid accessibility allows global participation with Remote Raid Passes

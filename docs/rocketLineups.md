## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/rocketLineups.min.json

## JSON Example

```json
{
    "name": "Giovanni",
    "title": "Team GO Rocket Boss",
    "type": "",
    "typeImage": "",
    "typeImageWidth": null,
    "typeImageHeight": null,
    "gender": "Male",
    "firstPokemon": [
        {
            "name": "Persian",
            "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm53.icon.png",
            "imageWidth": 134,
            "imageHeight": 146,
            "types": ["normal"],
            "isEncounter": false,
            "canBeShiny": false,
            "shinyAssetUrl": null
        }
    ],
    "secondPokemon": [...],
    "thirdPokemon": [...]
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Rocket member name (e.g., "Giovanni", "Cliff", "Fire-type Female Grunt") |
| `title` | string | Role: "Team GO Rocket Boss", "Team GO Rocket Leader", or "Team GO Rocket Grunt" |
| `type` | string | Type specialization (e.g., "fire", "water"), empty string for non-typed encounters |
| `typeImage` | string | Type badge icon URL, empty string for non-typed |
| `typeImageWidth` | integer\|null | Type badge width in pixels, null for non-typed |
| `typeImageHeight` | integer\|null | Type badge height in pixels, null for non-typed |
| `gender` | string | "Male" or "Female" |
| `firstPokemon` | array | Possible first slot Pokémon (one randomly selected per battle) |
| `secondPokemon` | array | Possible second slot Pokémon (one randomly selected per battle) |
| `thirdPokemon` | array | Possible third slot Pokémon (one randomly selected per battle) |
| `*Pokemon[].name` | string | Pokémon name, may include regional forms |
| `*Pokemon[].image` | string | Pokémon icon URL |
| `*Pokemon[].imageWidth` | integer | Icon width in pixels |
| `*Pokemon[].imageHeight` | integer | Icon height in pixels |
| `*Pokemon[].types` | array | Pokémon types |
| `*Pokemon[].isEncounter` | boolean | Whether this Pokémon can be caught after winning |
| `*Pokemon[].canBeShiny` | boolean | Whether shiny form is available for Shadow version |
| `*Pokemon[].shinyAssetUrl` | string\|null | Shiny sprite URL, null if unavailable |

## Data Structure

The endpoint returns an array of Team GO Rocket member objects. The array includes:
- Giovanni (Boss) - Requires Super Rocket Radar
- Leaders (Cliff, Arlo, Sierra) - Require Rocket Radar  
- Grunts (Various types) - Appear at invaded PokéStops/Balloons

Each member has a three-slot lineup with randomized selections per battle.

## Team GO Rocket Hierarchy

**Team GO Rocket Boss** (`title: "Team GO Rocket Boss"`):
- Giovanni - Appears only during special research
- Requires Super Rocket Radar to locate
- Always has fixed first Pokémon (Persian)
- Rotates legendary third Pokémon periodically
- Defeating grants Super Rocket Radar parts from research

**Team GO Rocket Leaders** (`title: "Team GO Rocket Leader"`):
- Cliff, Arlo, Sierra
- Require Rocket Radar to locate (6 Mysterious Components)
- Appear at random PokéStops or Rocket Balloons
- Lineups change monthly or with events
- More challenging than Grunts
- Better rewards (higher IV Shadow Pokémon, more items)

**Team GO Rocket Grunts** (`title: "Team GO Rocket Grunt"`):
- Type-specific or generic trainers
- Appear at invaded PokéStops (lasts 30 minutes)
- Appear in Rocket Balloons (6 AM, 12 PM, 6 PM daily)
- No Rocket Radar required
- Easier battles
- Identifiable by dialogue hints

## Type Specialization

Grunts specialize in specific types:
- `type`: Type name ("fire", "water", "dragon", etc.) or empty string
- `typeImage`: Type badge icon URL
- Empty type means mixed/generic lineup

**Type Badge** (`typeImage`):
- Visual indicator of Grunt's specialty
- Helps predict lineup before battle
- 128×128px resolution
- Null for Giovanni, Leaders, and generic Grunts

## Gender Significance

`gender` field is critical:
- **Same type + Different gender = Different lineup**
- Example: "Fire-type Female Grunt" ≠ "Fire-type Male Grunt"
- Lineups completely different, not just cosmetic
- Affects catchable Shadow Pokémon
- Must differentiate in tracking/database

**Special Cases**:
- Giovanni: Always Male
- Leaders: Fixed gender per character
- Generic Grunts: May have Male/Female variants with different lineups

## Battle Slot Mechanics

### Slot Selection

Each battle randomly selects:
1. **One** Pokémon from `firstPokemon` array
2. **One** Pokémon from `secondPokemon` array  
3. **One** Pokémon from `thirdPokemon` array

Selection is **independent** per slot:
- Not weighted by position in array
- True random selection (equal probability)
- Determined at battle start
- Cannot be predicted

### Slot Properties

Each Pokémon in slot arrays includes:
- `name`: Pokémon species (may include form)
- `image`: Icon with dimensions
- `types`: Type array (affects battle strategy)
- `isEncounter`: **Critical field** - Whether catchable after victory
- `canBeShiny`: Whether Shadow version can be shiny
- `shinyAssetUrl`: Shiny Shadow form sprite (null if unavailable)

## Encounter Mechanics

### Shadow Pokémon Capture

After defeating Rocket member:
1. **One** of the three Pokémon used in battle becomes catchable
2. Only Pokémon with `isEncounter: true` eligible
3. Random selection from eligible Pokémon that appeared
4. If multiple eligible, one randomly selected
5. Appears as Shadow variant (purple aura, red eyes)

**Important Rules**:
- Must appear in the actual battle (from selected slot)
- First slot usually guaranteed `isEncounter: false`
- Second/third slots typically have `isEncounter: true`
- Giovanni's third slot (legendary) always `isEncounter: true`

### Shadow Pokémon Properties

Shadow Pokémon caught from Rocket battles:
- Have purple aura and red eyes
- Learn Frustration (Charged Move)
- Deal 20% more damage
- Take 20% more damage  
- Cost more resources to power up
- Can be Purified (removes Shadow status)
- **Cannot be traded while Shadow**
- Purifying gives:
  - +2 to each IV (max 15)
  - Learns Return move
  - Reduced power-up costs
  - Cheaper to trade

### Shiny Shadow Pokémon

`canBeShiny: true` for encounter Pokémon:
- Extremely rare (~1/64 or lower)
- Only specific species can be shiny as Shadow
- Shiny rate does NOT improve on purification
- Highly valuable for collectors
- Shiny Shadow stronger than Shiny Purified in some cases

## Lineup Rotation

**Giovanni Rotation**:
- Changes every few months
- First slot always Persian (signature Pokémon)
- Second slot rotates monthly
- Third slot is current featured Legendary Shadow Pokémon
- Announced via Special Research

**Leader Rotation**:
- Changes monthly, sometimes mid-month
- First slot relatively stable (signature Pokémon)
- Second and third slots rotate frequently
- Aligned with events and new Shadow Pokémon releases

**Grunt Rotation**:
- More stable than Leaders
- Changes with seasons or major events
- New type specialists added with new Pokémon generations
- Generic Grunts remain fairly constant

## Dialogue Hints

Grunts announce type before battle (not in dataset):
- "Don't tangle with us!" = Grass-type
- "These waters are treacherous!" = Water-type
- "You're gonna be frozen in your tracks!" = Ice-type
- "Check out my cute Pokémon!" = Fairy-type
- Etc.

Helps players predict lineup and prepare counters.

## Battle Difficulty

**Grunts**:
- Lowest difficulty
- Pokémon ~Level 25-30
- Basic movesets
- Predictable AI

**Leaders**:
- Medium-high difficulty  
- Pokémon ~Level 30-35
- Full movesets
- Use Protect Shields (2 shields)

**Giovanni**:
- Highest difficulty
- Pokémon ~Level 35-40
- Optimized movesets
- Uses Protect Shields (2 shields)
- More aggressive AI

## Important Notes

- Rocket lineups update reflects current active rosters
- Historical lineups not maintained
- Battle rewards include:
  - Shadow Pokémon encounter
  - Mysterious Components (Grunts: 2, Leaders: varies)
  - Stardust (500-1000)
  - Chance at 12 km Egg (Leaders only)
- Defeating 6 Grunts = 1 Rocket Radar (to find Leaders)
- Leaders drop random Mysterious Components, not full Rocket Radar
- Shadow Pokémon IVs: 0-15 for each stat (no minimum floor)
- Purification adds +2 to each IV (capped at 15)
- Can't change teams during Rocket battle
- Battle can be fled and re-attempted (lineup rerolls)
- PokéStops remain invaded until battle completed or 30 minutes expire

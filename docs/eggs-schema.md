# Pokémon GO Egg Data Schema
## Overview
The `eggs.min.json` file contains comprehensive data about the Pokémon egg pool in Pokémon GO. This dataset describes which Pokémon can hatch from different egg types, their rarity, shiny availability, and other relevant properties.
**Note:** The egg pool changes frequently with events and game updates. This documentation focuses on the data structure rather than specific current contents.
## Data Access
The data is available via CDN:
```
https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json
```
### Usage Example
```javascript
// Fetch the data
fetch('https://cdn.jsdelivr.net/gh/quantNebula/data@master/eggs.min.json')
  .then(response => response.json())
  .then(eggs => {
    console.log(`Loaded ${eggs.length} egg entries`);
  });
```
## File Structure
The file contains a JSON array of Pokémon objects, where each object represents a specific Pokémon (or form variant) available from eggs.
```json
[
  {
    "name": "Bulbasaur",
    "eggType": "2 km",
    "isAdventureSync": false,
    "image": "https://cdn.leekduck.com/assets/img/pokemon_icons_crop/pm1.icon.png",
    "imageWidth": 107,
    "imageHeight": 126,
    "canBeShiny": true,
    "shinyAssetUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_001_00_shiny.png",
    "combatPower": {
      "min": 637,
      "max": 637
    },
    "isRegional": false,
    "isGiftExchange": false,
    "rarity": 4,
    "rarityTier": "Very Rare"
  }
]
```
## JSON Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pokémon GO Egg Data Schema",
  "description": "Schema for Pokémon GO egg pool data",
  "type": "array",
  "items": {
    "type": "object",
    "required": [
      "name",
      "eggType",
      "isAdventureSync",
      "image",
      "imageWidth",
      "imageHeight",
      "canBeShiny",
      "combatPower",
      "isRegional",
      "isGiftExchange",
      "rarity",
      "rarityTier"
    ],
    "properties": {
      "name": {
        "type": "string",
        "description": "The name of the Pokémon. May include form variations (e.g., 'Galarian Meowth', 'Indeedee (Male)')"
      },
      "eggType": {
        "type": "string",
        "description": "The type of egg this Pokémon can hatch from",
        "enum": ["1 km", "2 km", "5 km", "7 km", "10 km", "12 km"]
      },
      "isAdventureSync": {
        "type": "boolean",
        "description": "Whether this Pokémon is exclusive to Adventure Sync reward eggs"
      },
      "image": {
        "type": "string",
        "format": "uri",
        "description": "URL to the Pokémon's icon image"
      },
      "imageWidth": {
        "type": "integer",
        "description": "Width of the icon image in pixels",
        "minimum": 1
      },
      "imageHeight": {
        "type": "integer",
        "description": "Height of the icon image in pixels",
        "minimum": 1
      },
      "canBeShiny": {
        "type": "boolean",
        "description": "Whether this Pokémon can be encountered in its shiny form when hatched"
      },
      "shinyAssetUrl": {
        "type": ["string", "null"],
        "format": "uri",
        "description": "URL to the shiny variant icon image. null if shiny form is not available or not yet released"
      },
      "combatPower": {
        "type": "object",
        "description": "The Combat Power (CP) for this Pokémon when hatched",
        "required": ["min", "max"],
        "properties": {
          "min": {
            "type": "integer",
            "description": "Combat Power value (always identical to max for eggs)",
            "minimum": 0
          },
          "max": {
            "type": "integer",
            "description": "Combat Power value (always identical to min for eggs)",
            "minimum": 0
          }
        }
      },
      "isRegional": {
        "type": "boolean",
        "description": "Whether this Pokémon is normally region-locked in the wild"
      },
      "isGiftExchange": {
        "type": "boolean",
        "description": "Whether this Pokémon can ONLY be obtained from 7 km eggs received through friend gifts (true = gift-exclusive)"
      },
      "rarity": {
        "type": "integer",
        "description": "Numeric rarity value (1-5) within this specific egg pool",
        "minimum": 1,
        "maximum": 5
      },
      "rarityTier": {
        "type": "string",
        "description": "Human-readable rarity tier",
        "enum": ["Common", "Uncommon", "Rare", "Very Rare", "Ultra Rare"]
      }
    }
  }
}
```
## Property Definitions
### Core Properties
| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `name` | string | Yes | The Pokémon's name, including form variants (e.g., "Galarian Meowth", "Indeedee (Male)", "Basculin (White Striped)") |
| `eggType` | string | Yes | The egg distance type this Pokémon hatches from |
| `isAdventureSync` | boolean | Yes | Whether this Pokémon is exclusive to Adventure Sync reward eggs (same distance, different pool) |
| `image` | string (URI) | Yes | URL to the Pokémon's icon image |
| `imageWidth` | integer | Yes | Width of the icon image in pixels |
| `imageHeight` | integer | Yes | Height of the icon image in pixels |
| `canBeShiny` | boolean | Yes | Whether this Pokémon can be shiny when hatched. `false` means shiny form not yet released |
| `shinyAssetUrl` | string/null | Yes | URL to the shiny variant icon image. `null` if shiny form is not available or not yet released |
| `combatPower` | object | Yes | CP value for the hatched Pokémon (min and max are always identical) |
| `isRegional` | boolean | Yes | Whether this Pokémon is normally region-locked in wild encounters |
| `isGiftExchange` | boolean | Yes | For 7 km eggs only: `true` means ONLY from gift eggs, `false` means available in general 7 km pool |
| `rarity` | integer | Yes | Numeric rarity value (1-5) relative to this specific egg pool |
| `rarityTier` | string | Yes | Human-readable rarity tier |
### Egg Types
The `eggType` property indicates which egg distance category the Pokémon hatches from:
#### **1 km Eggs**
- **Content**: Complete roster of starter Pokémon from all generations (Gen 1-9)
- **Includes**: All first-stage starters: Bulbasaur, Charmander, Squirtle through Sprigatito, Fuecoco, Quaxly
- **Total**: 27 starter Pokémon
- **Acquisition**: Event-specific, typically during special research or limited-time events
- **Characteristics**: All starters are Very Rare (rarity: 4)
#### **2 km Eggs**
- **Content**: Common Pokémon, baby Pokémon, some rare finds
- **Includes**: Baby Pokémon (Cleffa, Igglybuff, Smoochum), common species, and notably Larvesta (Ultra Rare)
- **Acquisition**: PokéStops and Gyms
- **Rarity Range**: Common (1) to Ultra Rare (5)
#### **5 km Eggs**
- **Content**: Standard Pokémon pool with moderate rarity, includes gender variants
- **Includes**: Mix of common to rare species, Indeedee gender forms, Munchlax
- **Acquisition**: PokéStops and Gyms (regular) or Adventure Sync rewards (25km weekly)
- **Adventure Sync Pool**: Separate pool including Riolu, Bonsly, Tirtouga, Skarmory, Archen
#### **7 km Eggs**
- **Content**: Regional form variants (Alolan, Galarian, Hisuian) and special color variants
- **Includes**: Alolan forms, Galarian forms, Hisuian forms, Basculin (White Striped)
- **Acquisition**: **EXCLUSIVELY** from opening friend gifts - cannot be obtained any other way
- **Gift Exchange Flag**: Some Pokémon are gift-exclusive (`isGiftExchange: true`), others may appear in general 7 km pool
#### **10 km Eggs**
- **Content**: Rare Pokémon, pseudo-legendaries, sought-after species
- **Includes**: Beldum, Jangmo-o, Dreepy, Frigibax, and notably Larvesta (Ultra Rare)
- **Acquisition**: PokéStops and Gyms (regular) or Adventure Sync rewards (50km weekly)
- **Adventure Sync Pool**: Separate pool including Gible, Beldum, Dreepy, Drampa, Turtonator
- **Rarity Range**: Primarily Common (1) to Rare (3), with Ultra Rare (5) exceptions
#### **12 km Eggs** (Strange Eggs)
- **Content**: Dark, Poison, Fighting-type Pokémon with sinister themes
- **Includes**: Sandile, Vullaby, Shroodle, Pancham, Salandit, Varoom
- **Acquisition**: **EXCLUSIVELY** from defeating Team GO Rocket Leaders (Cliff, Arlo, Sierra) - not from regular grunts
- **Characteristics**: All Common rarity (1), diverse typing beyond just Dark/Poison/Fighting
- **Inventory Note**: Limited to 1 Strange Egg at a time; must hatch current one to receive another
### Adventure Sync Eggs
When `isAdventureSync` is `true`, the Pokémon **only** hatches from eggs earned through Adventure Sync weekly rewards, **not** from regular PokéStop eggs:
- **5 km Adventure Sync**: Earned by walking 25km in a week
- **10 km Adventure Sync**: Earned by walking 50km in a week
**Important Distinction**: Adventure Sync eggs have completely separate Pokémon pools from regular eggs of the same distance. For example:
- Regular 10 km eggs: Pawniard, Carbink, Toxel, Honedge, etc.
- Adventure Sync 10 km eggs: Gible, Beldum, Dreepy, Drampa, Turtonator
**Rarity Differences**: The same Pokémon may have different rarity values in Adventure Sync vs. regular eggs:
- Dreepy: Rare (3) in regular 10 km, but Common (1) in Adventure Sync 10 km
- This reflects different pool distributions
### Combat Power
The `combatPower` object contains `min` and `max` values that are **always identical** for eggs:
```json
"combatPower": {
  "min": 637,
  "max": 637
}
```
**Why identical?** Eggs hatch Pokémon at fixed levels (20 for regular eggs, 25 for weather-boosted Adventure Sync eggs), which produces consistent CP. The min/max structure exists for schema consistency but should be treated as a single value.
**Display Recommendation**: Show as "CP: 637" rather than "CP: 637-637"
**Level Information**:
- Regular eggs: Level 20
- Weather-boosted Adventure Sync eggs: Level 25
- CP values in the data reflect level 20 hatches
### Rarity System
Each Pokémon has both a numeric `rarity` (1-5) and a human-readable `rarityTier`:
| rarity | rarityTier | Description |
|--------|-----------|-------------|
| 1 | Common | Frequently encountered within this egg pool |
| 2 | Uncommon | Moderately rare |
| 3 | Rare | Less common, desirable |
| 4 | Very Rare | Significantly rare, highly sought |
| 5 | Ultra Rare | Extremely rare, special events |
**Important**: Rarity is **relative to the specific egg pool**, not global. The same Pokémon can have different rarities in different contexts:
- Larvesta: Ultra Rare (5) in all three egg types it appears in
- Dreepy: Rare (3) in regular 10 km, Common (1) in Adventure Sync 10 km
### Regional and Gift Exchange Flags
**`isRegional`**: Indicates the Pokémon is normally restricted to specific geographic regions in wild encounters (e.g., Kangaskhan in Australia, Corsola in tropics). During events, these may become globally available through eggs, allowing trainers worldwide to obtain region-exclusives.
**`isGiftExchange`**: Specifically for 7 km eggs, this flag clarifies availability:
- **`true`**: ONLY available from friend gift eggs (gift-exclusive)
- **`false`**: Available in general 7 km egg pool
**Note**: All 7 km eggs come from gifts, but some Pokémon within 7 km eggs are restricted to gift-exclusive pools while others may be more generally available.
## Form Variations and Special Cases
### Regional Forms
The `name` field includes comprehensive form designations:
**Alolan Forms** (from the Alola region):
- "Alolan Geodude" - Electric/Rock type
- "Alolan Diglett" - Ground/Steel type
- Found in 7 km eggs
**Galarian Forms** (from the Galar region):
- "Galarian Meowth" - Steel type
- "Galarian Zigzagoon" - Dark/Normal type
- "Galarian Darumaka" - Ice type
- "Galarian Stunfisk" - Ground/Steel type
- "Galarian Corsola" - Ghost type
- "Galarian Slowpoke" - Psychic type
- Found in 7 km eggs
**Hisuian Forms** (from the Hisui region):
- "Hisuian Sneasel" - Fighting/Poison type
- "Hisuian Growlithe" - Fire/Rock type
- Found in 7 km eggs (gift-exclusive)
**Special Color Variants**:
- "Basculin (White Striped)" - Unique color form that can evolve into Basculegion
- Found in 7 km eggs (gift-exclusive)
### Gender-Specific Forms
Some Pokémon have distinct gender forms with different appearances and stats:
**Indeedee**:
- "Indeedee (Male)" - CP: 1396
- "Indeedee (Female)" - CP: 1370
- Different designs, both found in 5 km eggs
Gender forms are treated as separate entries with unique images and properties.
### Duplicate Pokémon Entries
**Critical**: Some Pokémon appear multiple times in the dataset with different configurations. This represents:
1. Availability in multiple egg types simultaneously
2. Differences between regular and Adventure Sync pools
3. Distinctions in gift-exclusive vs. general 7 km availability
**Examples of Duplicates**:
**Larvesta** (appears 3 times):
- 2 km egg: Ultra Rare, CP 855
- 5 km egg: Ultra Rare, CP 855
- 10 km egg: Ultra Rare, CP 855
- *Same stats across all egg types, indicating current or rotating availability*
**Bonsly** (appears 2 times):
- 2 km egg: Common, regular pool
- 5 km egg: Common, Adventure Sync exclusive
- *Different acquisition methods*
**Beldum** (appears 2 times):
- 10 km egg: Common, regular pool
- 10 km egg: Common, Adventure Sync exclusive
- *Same egg distance, different pools*
**Dreepy** (appears 2 times):
- 10 km egg: Rare (3), regular pool
- 10 km egg: Common (1), Adventure Sync exclusive
- *Different rarity values in different pools*
**Galarian Corsola** (appears 2 times):
- 7 km egg: Common, isGiftExchange: false
- 7 km egg: Common, isGiftExchange: true
- *Indicates change in availability status or dual availability*
**Handling Duplicates**: When querying by Pokémon name, filter by additional criteria (egg type, Adventure Sync status, gift exchange status) to get the specific entry you need.
### Shiny Availability Patterns
Shiny availability shows interesting patterns by generation:
**Generations 1-7 Starters**: All `canBeShiny: true`
- Bulbasaur through Popplio: All shiny-capable
**Generation 8 Starters**: All `canBeShiny: false`
- Grookey, Scorbunny, Sobble: Shinies not yet released
**Generation 9 Starters**: All `canBeShiny: true`
- Sprigatito, Fuecoco, Quaxly: Shinies available
This pattern reflects Niantic's release schedule for shiny variants.
### Image URL Patterns
Images are hosted on a CDN and follow consistent URL patterns:
- **Standard Pokémon**: Include Pokémon ID number (e.g., `pm1.icon.png` for Bulbasaur)
- **Regional forms**: Include form designation (e.g., `.fALOLA`, `.fGALARIAN`, `.fHISUIAN`)
- **Gender forms**: Include gender designation (e.g., `.fMALE`, `.fFEMALE`)
- **Color variants**: Include variant designation (e.g., `.fWHITE_STRIPED`)
## Egg Rewards and Mechanics
### Additional Rewards
Beyond the Pokémon itself, eggs provide:
**Stardust** (based on egg distance):
- 2 km: 400-800 Stardust
- 5 km: 600-1,600 Stardust
- 7 km: 800-1,600 Stardust
- 10 km: 1,600-3,200 Stardust
- 12 km: 1,600-3,200 Stardust
**Candy** (based on egg distance):
- 2 km: 5-10 Candy
- 5 km: 10-21 Candy
- 7 km: 10-21 Candy
- 10 km: 16-32 Candy
- 12 km: 16-32 Candy
**XP**: Fixed amount based on egg distance
### Acquisition Methods
**Critical Distinctions**:
| Egg Type | Acquisition Method | Notes |
|----------|-------------------|-------|
| 1 km | Special events | Event-specific availability, not always available |
| 2 km | PokéStops & Gyms | Most common egg type |
| 5 km | PokéStops & Gyms OR Adventure Sync (25km) | Two separate pools |
| 7 km | Friend Gifts ONLY | 100% from opening gifts, never from stops |
| 10 km | PokéStops & Gyms OR Adventure Sync (50km) | Two separate pools |
| 12 km | Rocket Leaders ONLY | Cliff, Arlo, or Sierra - not regular grunts |
**Adventure Sync Requirements**:
- Must have Adventure Sync enabled
- Must have egg space available when reward is claimed
- 25km weekly: Rewards 5 km Adventure Sync egg
- 50km weekly: Rewards 10 km Adventure Sync egg
**Strange Egg (12 km) Notes**:
- Can only hold ONE Strange Egg at a time
- Must defeat a Team GO Rocket Leader
- Must have egg space available when leader is defeated
- Cannot receive another until current Strange Egg is hatched
## Usage Examples
### Filtering by Egg Type
```javascript
// Get all Pokémon from 10 km eggs (including duplicates)
const tenKmPokemon = eggs.filter(p => p.eggType === "10 km");
// Get Adventure Sync exclusives
const adventureSyncPokemon = eggs.filter(p => p.isAdventureSync === true);
// Get regular (non-Adventure Sync) 10 km eggs
const regular10km = eggs.filter(p => 
  p.eggType === "10 km" && p.isAdventureSync === false
);
```
### Finding Shiny-Eligible Pokémon
```javascript
// Get all Pokémon that can be shiny from 5 km eggs
const shiny5kmPokemon = eggs.filter(p => 
  p.eggType === "5 km" && p.canBeShiny === true
);
// Get all shinies from any egg type
const allShinyEggs = eggs.filter(p => p.canBeShiny === true);
```
### Filtering by Rarity
```javascript
// Get Ultra Rare Pokémon
const ultraRare = eggs.filter(p => p.rarityTier === "Ultra Rare");
// Get rare or better from 2 km eggs
const rare2km = eggs.filter(p => 
  p.eggType === "2 km" && p.rarity >= 3
);
// Get all Common rarity from Adventure Sync eggs
const commonAS = eggs.filter(p =>
  p.isAdventureSync === true && p.rarity === 1
);
```
### Regional Forms and Variants
```javascript
// Get all Galarian forms
const galarianForms = eggs.filter(p => p.name.includes("Galarian"));
// Get all Alolan forms
const alolanForms = eggs.filter(p => p.name.includes("Alolan"));
// Get all Hisuian forms
const hisuianForms = eggs.filter(p => p.name.includes("Hisuian"));
// Get all regional-exclusive Pokémon available through eggs
const regionalPokemon = eggs.filter(p => p.isRegional === true);
// Get all regional forms (any region)
const allRegionalForms = eggs.filter(p => 
  p.name.includes("Alolan") || 
  p.name.includes("Galarian") || 
  p.name.includes("Hisuian")
);
```
### Gift Exchange Analysis
```javascript
// Get Pokémon ONLY from friend gifts (gift-exclusive)
const giftExclusives = eggs.filter(p => 
  p.eggType === "7 km" && p.isGiftExchange === true
);
// Get all 7 km Pokémon (gift eggs) regardless of exclusivity
const all7km = eggs.filter(p => p.eggType === "7 km");
// Compare gift-exclusive vs general 7km pool
const generalGift = eggs.filter(p => 
  p.eggType === "7 km" && p.isGiftExchange === false
);
```
### Finding and Handling Duplicates
```javascript
// Find Pokémon that appear in multiple egg pools
const pokemonCounts = eggs.reduce((acc, p) => {
  acc[p.name] = (acc[p.name] || 0) + 1;
  return acc;
}, {});
const duplicates = Object.entries(pokemonCounts)
  .filter(([name, count]) => count > 1)
  .map(([name]) => name);
console.log('Pokémon in multiple eggs:', duplicates);
// Get all instances of a specific Pokémon
function getAllInstances(pokemonName) {
  return eggs.filter(p => p.name === pokemonName);
}
const larvestaInstances = getAllInstances('Larvesta');
console.log(`Larvesta appears in ${larvestaInstances.length} egg types`);
// Get the best source for a Pokémon (lowest distance)
function getBestSource(pokemonName) {
  const instances = eggs.filter(p => p.name === pokemonName);
  const distances = {
    "1 km": 1, "2 km": 2, "5 km": 5, 
    "7 km": 7, "10 km": 10, "12 km": 12
  };
  
  return instances.sort((a, b) => 
    distances[a.eggType] - distances[b.eggType]
  )[0];
}
```
### Grouping by Egg Type
```javascript
// Group all Pokémon by egg type
const byEggType = eggs.reduce((acc, pokemon) => {
  const key = pokemon.eggType;
  if (!acc[key]) acc[key] = [];
  acc[key].push(pokemon);
  return acc;
}, {});
console.log(`2 km eggs contain ${byEggType["2 km"].length} Pokémon`);
// Group with Adventure Sync distinction
const grouped = eggs.reduce((acc, pokemon) => {
  const key = pokemon.isAdventureSync 
    ? `${pokemon.eggType} (Adventure Sync)` 
    : pokemon.eggType;
  if (!acc[key]) acc[key] = [];
  acc[key].push(pokemon);
  return acc;
}, {});
```
### Finding Starter Pokémon
```javascript
// Get all starters (1 km eggs)
const allStarters = eggs.filter(p => p.eggType === "1 km");
// Group starters by generation based on ID patterns
const startersByGen = {
  gen1: allStarters.filter(p => ['Bulbasaur', 'Charmander', 'Squirtle'].includes(p.name)),
  gen2: allStarters.filter(p => ['Chikorita', 'Cyndaquil', 'Totodile'].includes(p.name)),
  // ... etc
};
// Get starters that can be shiny
const shinyStarters = allStarters.filter(p => p.canBeShiny === true);
const noShinyStarters = allStarters.filter(p => p.canBeShiny === false);
```
### Gender-Specific Pokémon
```javascript
// Find all Pokémon with gender variants
const genderVariants = eggs.filter(p => 
  p.name.includes('(Male)') || p.name.includes('(Female)')
);
// Get both genders of a specific Pokémon
function getGenderForms(baseName) {
  return eggs.filter(p => p.name.startsWith(baseName));
}
const indeedeeForms = getGenderForms('Indeedee');
console.log(indeedeeForms.map(p => `${p.name}: CP ${p.combatPower.min}`));
```
### Strange Eggs (12 km) Analysis
```javascript
// Get all Strange Egg Pokémon
const strangeEggs = eggs.filter(p => p.eggType === "12 km");
// Analyze type distribution in Strange Eggs
const strangeEggTypes = strangeEggs.map(p => {
  // Would need type data - this is placeholder
  return { name: p.name, cp: p.combatPower.min };
});
```
## Best Practices
### Data Consumption
1. **Handle duplicates intentionally**: Some Pokémon appear multiple times with different configurations. Always filter by the specific criteria you need (egg type + Adventure Sync status + gift exchange status).
2. **Treat CP as single value**: Although the schema has min/max, they're always identical for eggs. Display as a single value.
3. **Check Adventure Sync status**: Regular eggs and Adventure Sync eggs of the same distance are SEPARATE pools with different Pokémon.
4. **Consider rarity context**: Rarity is relative to each specific pool. A "Common" Pokémon in 10 km eggs may still be valuable.
5. **Cache images**: The icon URLs are stable CDN links, but consider caching them locally for better performance.
6. **Gift exchange clarity**: For 7 km eggs, distinguish between gift-exclusive (`isGiftExchange: true`) and general 7 km pool.
### Display Considerations
1. **Form names**: The `name` field includes all necessary form information for display - use it directly.
2. **CP display**: Since `min` and `max` are always identical, display as "CP: 637" rather than "CP: 637-637" or "CP: 637-637".
3. **Image dimensions**: Use the provided `imageWidth` and `imageHeight` for proper aspect ratio rendering and responsive design.
4. **Shiny indicators**: Use `canBeShiny` to show shiny availability indicators in your UI (✨ icon, star badge, etc.).
5. **Rarity visualization**: Consider color-coding or badging based on `rarityTier` for quick visual scanning.
6. **Duplicate handling**: When displaying egg pools, decide whether to:
   - Show all instances of duplicates (comprehensive view)
   - Deduplicate by name and show "Available in: 2km, 5km, 10km"
   - Filter to specific context (regular vs Adventure Sync)
### Query Optimization
1. **Index by name**: If frequently querying by Pokémon name, build a lookup index:
```javascript
const byName = eggs.reduce((acc, p) => {
  if (!acc[p.name]) acc[p.name] = [];
  acc[p.name].push(p);
  return acc;
}, {});
```
2. **Pre-filter pools**: Separate regular and Adventure Sync pools upfront:
```javascript
const regularEggs = eggs.filter(p => !p.isAdventureSync);
const adventureSyncEggs = eggs.filter(p => p.isAdventureSync);
```
3. **Egg type maps**: Create maps for quick access:
```javascript
const eggPools = {
  '1km': eggs.filter(p => p.eggType === '1 km'),
  '2km': eggs.filter(p => p.eggType === '2 km'),
  // ... etc
};
```
## Important Notes
- **Data volatility**: The egg pool changes frequently with in-game events, seasonal rotations, and updates. Always treat this data as a current snapshot that may become outdated quickly.
- **Regional availability**: Pokémon marked as `isRegional: true` are normally geo-restricted in the wild but can be globally available through eggs during events.
- **Egg acquisition methods**:
  - **1 km**: Event-specific, not always available
  - **2 km, 5 km, 10 km**: PokéStops and Gyms (regular pool)
  - **5 km, 10 km**: Adventure Sync rewards (separate pool, requires 25km/50km weekly)
  - **7 km**: Friend gifts exclusively - 100% of 7km eggs come from gifts
  - **12 km**: Team GO Rocket Leaders only (Cliff, Arlo, Sierra) - not from grunts
- **Combat Power**: Values represent level 20 hatches. Weather-boosted Adventure Sync eggs hatch at level 25 with proportionally higher CP.
- **Incubator efficiency**: 
  - Infinite Incubator: Free, unlimited use
  - Regular Incubator: 3 uses, 1× speed
  - Super Incubator: 3 uses, 1.5× speed
  - Consider using limited incubators on longer-distance eggs (10 km, 12 km)
- **Strange Egg limit**: Only one 12 km (Strange) Egg can be held at a time. Must hatch current Strange Egg before receiving another from Leaders.
- **Egg space management**: 
  - Maximum 9 eggs + 3 bonus slots (12 total with purchases)
  - Adventure Sync rewards require available egg space when claimed
  - Consider clearing space before Monday 9 AM for Adventure Sync rewards
- **Duplicate entries**: Some Pokémon appear multiple times in the dataset. This is intentional and represents:
  - Multiple egg type availability
  - Regular vs. Adventure Sync pool differences  
  - Gift-exclusive vs. general 7 km pool distinctions
  - Event rotations and availability changes
- **Shiny rates**: Egg hatches have approximately 1/50 shiny odds when `canBeShiny: true` (better than wild spawns).
- **Form variants**: Regional forms, gender forms, and color variants are distinct entries with unique properties and images.
# Events Data Documentation

## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/events.min.json

## Overview

The endpoint returns an array of event objects representing current and upcoming Pokémon GO events. Events are sorted chronologically and include detailed information about bonuses, spawns, raids, research, and other event-specific content.

## Base Event Structure

All events share these common base fields:

| Field | Type | Description | Possible Values |
|-------|------|-------------|-----------------|
| `eventID` | string | Unique event identifier in kebab-case | e.g., `january-communityday2026`, `kyurem-fusion-raid-day` |
| `name` | string | Event display name | e.g., `Grookey Community Day`, `Season 21: Precious Paths` |
| `eventType` | string | Event category (see Event Types section) | `community-day`, `raid-day`, `event`, `season`, etc. |
| `heading` | string | Human-readable category label | e.g., `Community Day`, `Raid Day`, `Event` |
| `image` | string | Event banner image URL | Full URL to event banner on CDN |
| `imageWidth` | integer | Banner width in pixels | Typically 1024-1280 |
| `imageHeight` | integer | Banner height in pixels | Typically 512-720 |
| `start` | string | Start time in ISO 8601 format | Format: `YYYY-MM-DDTHH:mm:ss.SSS` |
| `end` | string | End time in ISO 8601 format | Format: `YYYY-MM-DDTHH:mm:ss.SSS` |
| `timezone` | string \| null | Event timezone | `"Local Time"` or `null` (for UTC) |
| `status` | string | Event status | `"current"` (active) or `"upcoming"` (future) |
| `isCurrent` | boolean | True if event is currently active | `true` or `false` |
| `extraData` | object \| null | Event-specific details (see Event Types) | Structure varies by `eventType` |

### Timezone Handling

- **`"Local Time"`**: Start/end times apply to player's local timezone. Used for most events (Community Days, Spotlight Hours, local events)
- **`null`** (UTC): Times are applied globally at the exact same moment worldwide. Used for GO Battle League, season transitions, some raid releases

### Event Status

- **`"upcoming"`**: Event starts in the future
- **`"current"`**: Event is currently active
- Events transition from `"upcoming"` to `"current"` automatically at start time
- Past events are removed from the dataset (no historical archive)

## Event Types and ExtraData Structures

The `extraData` field contains event-specific information. Its structure varies significantly by `eventType`. Below are all possible fields for each event type based on actual data.

### community-day

**Total events in dataset**: 2

**ExtraData Structure:**

`extraData` contains: `communityday`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `communityday` | object | 1/2 | Optional |
| `communityday.bonusDisclaimers` | array | 1/2 | Bonus information (Optional) |
| `communityday.bonuses` | array | 1/2 | Bonus information (Optional) |
| `communityday.bonuses[].image` | string | 1/2 | Array item (Optional) |
| `communityday.bonuses[].imageHeight` | integer | 1/2 | Array item (Optional) |
| `communityday.bonuses[].imageWidth` | integer | 1/2 | Array item (Optional) |
| `communityday.bonuses[].text` | string | 1/2 | Array item (Optional) |
| `communityday.featuredAttack` | null | 1/2 | Optional |
| `communityday.fieldResearchTasks` | array | 1/2 | Optional |
| `communityday.lureModuleBonus` | null | 1/2 | Bonus information (Optional) |
| `communityday.photobomb` | null | 1/2 | Optional |
| `communityday.pokestopShowcases` | array | 1/2 | Optional |
| `communityday.shinies` | array | 1/2 | Shiny Pokémon (Optional) |
| `communityday.shinies[].canBeShiny` | boolean | 1/2 | Array item (Optional) |
| `communityday.shinies[].image` | string | 1/2 | Array item (Optional) |
| `communityday.shinies[].imageHeight` | integer | 1/2 | Array item (Optional) |
| `communityday.shinies[].imageWidth` | integer | 1/2 | Array item (Optional) |
| `communityday.shinies[].name` | string | 1/2 | Array item (Optional) |
| `communityday.spawns` | array | 1/2 | Featured Pokémon (Optional) |
| `communityday.spawns[].canBeShiny` | boolean | 1/2 | Array item (Optional) |
| `communityday.spawns[].image` | string | 1/2 | Array item (Optional) |
| `communityday.spawns[].imageHeight` | integer | 1/2 | Array item (Optional) |
| `communityday.spawns[].imageWidth` | integer | 1/2 | Array item (Optional) |
| `communityday.spawns[].name` | string | 1/2 | Array item (Optional) |
| `communityday.specialresearch` | array | 1/2 | Optional |
| `communityday.specialresearch[].name` | string | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].rewards` | array | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].rewards[].image` | string | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].rewards[].imageHeight` | integer | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].rewards[].imageWidth` | integer | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].rewards[].text` | string | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].step` | integer | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].tasks` | array | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].tasks[].reward` | object | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].tasks[].reward.image` | string | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].tasks[].reward.imageHeight` | integer | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].tasks[].reward.imageWidth` | integer | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].tasks[].reward.text` | string | 1/2 | Array item (Optional) |
| `communityday.specialresearch[].tasks[].text` | string | 1/2 | Array item (Optional) |
| `communityday.ticketedResearch` | object | 1/2 | Optional |
| `communityday.ticketedResearch.description` | string | 1/2 | Description (Optional) |
| `communityday.ticketedResearch.price` | integer | 1/2 | Price (Optional) |


### event

**Total events in dataset**: 5

**ExtraData Structure:**

`extraData` contains: `event`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `event` | object | 3/5 | Optional |
| `event.bonusDisclaimers` | array | 3/5 | Bonus information (Optional) |
| `event.bonuses` | array | 3/5 | Bonus information (Optional) |
| `event.bonuses[].image` | string | 3/5 | Array item (Optional) |
| `event.bonuses[].imageHeight` | integer | 3/5 | Array item (Optional) |
| `event.bonuses[].imageWidth` | integer | 3/5 | Array item (Optional) |
| `event.bonuses[].text` | string | 3/5 | Array item (Optional) |
| `event.customSections` | object | 3/5 | Optional |
| `event.customSections.eggs` | object | 2/5 | Game content section (Optional) |
| `event.customSections.eggs.lists` | array | 2/5 | Optional |
| `event.customSections.eggs.paragraphs` | array | 2/5 | Optional |
| `event.customSections.eggs.pokemon` | array | 2/5 | Optional |
| `event.customSections.raids` | object | 1/5 | Game content section (Optional) |
| `event.customSections.raids.lists` | array | 1/5 | Optional |
| `event.customSections.raids.paragraphs` | array | 1/5 | Optional |
| `event.customSections.raids.pokemon` | array | 1/5 | Optional |
| `event.customSections.research` | object | 3/5 | Game content section (Optional) |
| `event.customSections.research.lists` | array | 3/5 | Optional |
| `event.customSections.research.paragraphs` | array | 3/5 | Optional |
| `event.customSections.research.pokemon` | array | 3/5 | Optional |
| `event.customSections.sales` | object | 3/5 | Optional |
| `event.customSections.sales.lists` | array | 3/5 | Optional |
| `event.customSections.sales.paragraphs` | array | 3/5 | Optional |
| `event.customSections.sales.pokemon` | array | 3/5 | Optional |
| `event.customSections.spawns` | object | 3/5 | Featured Pokémon (Optional) |
| `event.customSections.spawns.lists` | array | 3/5 | Optional |
| `event.customSections.spawns.paragraphs` | array | 3/5 | Optional |
| `event.customSections.spawns.pokemon` | array | 3/5 | Optional |
| `event.features` | array | 3/5 | Optional |
| `event.shinies` | array | 3/5 | Shiny Pokémon (Optional) |


### go-battle-league

**Total events in dataset**: 8

**ExtraData**: `null` or not present

Events of this type typically do not contain additional structured data in `extraData`.


### go-pass

**Total events in dataset**: 1

**ExtraData**: `null` or not present

Events of this type typically do not contain additional structured data in `extraData`.


### max-battles

**Total events in dataset**: 2

**ExtraData**: `null` or not present

Events of this type typically do not contain additional structured data in `extraData`.


### max-mondays

**Total events in dataset**: 3

**ExtraData Structure:**

`extraData` contains: `maxmondays`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `maxmondays` | object | 3/3 |  |
| `maxmondays.bonus` | string | 3/3 | Bonus information |
| `maxmondays.featured` | object | 3/3 | Featured Pokémon |
| `maxmondays.featured.canBeShiny` | boolean | 3/3 | Shiny availability |
| `maxmondays.featured.image` | string | 3/3 | Image URL |
| `maxmondays.featured.imageHeight` | integer | 3/3 | Image dimensions (pixels) |
| `maxmondays.featured.imageWidth` | integer | 3/3 | Image dimensions (pixels) |
| `maxmondays.featured.name` | string | 3/3 | Name/title |


### pokemon-go-tour

**Total events in dataset**: 5

**ExtraData Structure:**

`extraData` contains: `gotour`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `gotour` | object | 5/5 |  |
| `gotour.costumedPokemon` | array | 5/5 |  |
| `gotour.eggs` | object | 5/5 | Game content section |
| `gotour.eggs.10km` | array | 5/5 |  |
| `gotour.eggs.2km` | array | 5/5 |  |
| `gotour.eggs.5km` | array | 5/5 |  |
| `gotour.eggs.5km[].canBeShiny` | boolean | 3/5 | Array item (Optional) |
| `gotour.eggs.5km[].image` | string | 3/5 | Array item (Optional) |
| `gotour.eggs.5km[].imageHeight` | integer | 3/5 | Array item (Optional) |
| `gotour.eggs.5km[].imageWidth` | integer | 3/5 | Array item (Optional) |
| `gotour.eggs.5km[].name` | string | 3/5 | Array item (Optional) |
| `gotour.eggs.7km` | array | 5/5 |  |
| `gotour.eventInfo` | object | 5/5 |  |
| `gotour.eventInfo.dates` | string | 5/5 |  |
| `gotour.eventInfo.location` | string | 5/5 |  |
| `gotour.eventInfo.name` | string | 5/5 | Name/title |
| `gotour.eventInfo.ticketPrice` | null | 5/5 |  |
| `gotour.eventInfo.ticketUrl` | string | 5/5 |  |
| `gotour.eventInfo.time` | string | 5/5 |  |
| `gotour.exclusiveBonuses` | array | 5/5 | Bonus information |
| `gotour.habitats` | object | 5/5 |  |
| `gotour.habitats.General` | object | 5/5 |  |
| `gotour.habitats.General.info` | string | 5/5 |  |
| `gotour.habitats.General.spawns` | array | 5/5 | Featured Pokémon |
| `gotour.incenseEncounters` | array | 5/5 |  |
| `gotour.raids` | object | 5/5 | Game content section |
| `gotour.raids.fiveStar` | array | 5/5 |  |
| `gotour.raids.mega` | array | 5/5 |  |
| `gotour.raids.oneStar` | array | 5/5 |  |
| `gotour.raids.threeStar` | array | 5/5 |  |
| `gotour.research` | object | 5/5 | Game content section |
| `gotour.research.field` | array | 5/5 |  |
| `gotour.research.masterwork` | array | 5/5 |  |
| `gotour.research.special` | array | 5/5 |  |
| `gotour.research.timed` | array | 5/5 |  |
| `gotour.sales` | array | 5/5 |  |
| `gotour.shinies` | array | 5/5 | Shiny Pokémon |
| `gotour.shinies[].canBeShiny` | boolean | 3/5 | Array item (Optional) |
| `gotour.shinies[].image` | string | 3/5 | Array item (Optional) |
| `gotour.shinies[].imageHeight` | integer | 3/5 | Array item (Optional) |
| `gotour.shinies[].imageWidth` | integer | 3/5 | Array item (Optional) |
| `gotour.shinies[].name` | string | 3/5 | Array item (Optional) |
| `gotour.shinyDebuts` | array | 5/5 |  |
| `gotour.ticketAddOns` | array | 5/5 |  |
| `gotour.whatsNew` | array | 5/5 |  |


### pokemon-spotlight-hour

**Total events in dataset**: 3

**ExtraData Structure:**

`extraData` contains: `spotlight`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `spotlight` | object | 3/3 |  |
| `spotlight.bonus` | string | 3/3 | Bonus information |
| `spotlight.canBeShiny` | boolean | 3/3 | Shiny availability |
| `spotlight.image` | string | 3/3 | Image URL |
| `spotlight.imageHeight` | integer | 3/3 | Image dimensions (pixels) |
| `spotlight.imageWidth` | integer | 3/3 | Image dimensions (pixels) |
| `spotlight.list` | array | 3/3 |  |
| `spotlight.list[].canBeShiny` | boolean | 3/3 | Array item |
| `spotlight.list[].image` | string | 3/3 | Array item |
| `spotlight.list[].imageHeight` | integer | 3/3 | Array item |
| `spotlight.list[].imageWidth` | integer | 3/3 | Array item |
| `spotlight.list[].name` | string | 3/3 | Array item |
| `spotlight.name` | string | 3/3 | Name/title |


### pokestop-showcase

**Total events in dataset**: 1

**ExtraData Structure:**

`extraData` contains: `pokestopshowcase`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `pokestopshowcase` | object | 1/1 |  |
| `pokestopshowcase.description` | string | 1/1 | Description |
| `pokestopshowcase.featured` | array | 1/1 | Featured Pokémon |
| `pokestopshowcase.featured[].canBeShiny` | boolean | 1/1 | Array item |
| `pokestopshowcase.featured[].image` | string | 1/1 | Array item |
| `pokestopshowcase.featured[].imageHeight` | integer | 1/1 | Array item |
| `pokestopshowcase.featured[].imageWidth` | integer | 1/1 | Array item |
| `pokestopshowcase.featured[].name` | string | 1/1 | Array item |


### raid-battles

**Total events in dataset**: 7

**ExtraData Structure:**

`extraData` contains: `raidbattles`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `raidbattles` | object | 4/7 | Optional |
| `raidbattles.alternationPattern` | string | 4/7 | Optional |
| `raidbattles.bosses` | array | 4/7 | Optional |
| `raidbattles.bosses[].canBeShiny` | boolean | 4/7 | Array item (Optional) |
| `raidbattles.bosses[].image` | string | 4/7 | Array item (Optional) |
| `raidbattles.bosses[].imageHeight` | integer | 4/7 | Array item (Optional) |
| `raidbattles.bosses[].imageWidth` | integer | 4/7 | Array item (Optional) |
| `raidbattles.bosses[].name` | string | 4/7 | Array item (Optional) |
| `raidbattles.featuredAttacks` | array | 4/7 | Optional |
| `raidbattles.shinies` | array | 4/7 | Shiny Pokémon (Optional) |
| `raidbattles.shinies[].canBeShiny` | boolean | 4/7 | Array item (Optional) |
| `raidbattles.shinies[].image` | string | 4/7 | Array item (Optional) |
| `raidbattles.shinies[].imageHeight` | integer | 4/7 | Array item (Optional) |
| `raidbattles.shinies[].imageWidth` | integer | 4/7 | Array item (Optional) |
| `raidbattles.shinies[].name` | string | 4/7 | Array item (Optional) |
| `raidbattles.tiers` | object | 4/7 | Optional |
| `raidbattles.tiers.fiveStar` | array | 4/7 | Optional |
| `raidbattles.tiers.mega` | array | 4/7 | Optional |
| `raidbattles.tiers.oneStar` | array | 4/7 | Optional |
| `raidbattles.tiers.threeStar` | array | 4/7 | Optional |


### raid-day

**Total events in dataset**: 3

**ExtraData Structure:**

`extraData` contains: `raidday`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `raidday` | object | 1/3 | Optional |
| `raidday.alternationPattern` | string | 1/3 | Optional |
| `raidday.bonuses` | array | 1/3 | Bonus information (Optional) |
| `raidday.bonuses[].image` | string | 1/3 | Array item (Optional) |
| `raidday.bonuses[].imageHeight` | integer | 1/3 | Array item (Optional) |
| `raidday.bonuses[].imageWidth` | integer | 1/3 | Array item (Optional) |
| `raidday.bonuses[].text` | string | 1/3 | Array item (Optional) |
| `raidday.featured` | array | 1/3 | Featured Pokémon (Optional) |
| `raidday.featured[].canBeShiny` | boolean | 1/3 | Array item (Optional) |
| `raidday.featured[].image` | string | 1/3 | Array item (Optional) |
| `raidday.featured[].imageHeight` | integer | 1/3 | Array item (Optional) |
| `raidday.featured[].imageWidth` | integer | 1/3 | Array item (Optional) |
| `raidday.featured[].name` | string | 1/3 | Array item (Optional) |
| `raidday.featuredAttacks` | array | 1/3 | Optional |
| `raidday.raids` | object | 1/3 | Game content section (Optional) |
| `raidday.raids.fiveStar` | array | 1/3 | Optional |
| `raidday.raids.mega` | array | 1/3 | Optional |
| `raidday.raids.other` | array | 1/3 | Optional |
| `raidday.shinies` | array | 1/3 | Shiny Pokémon (Optional) |
| `raidday.shinies[].canBeShiny` | boolean | 1/3 | Array item (Optional) |
| `raidday.shinies[].image` | string | 1/3 | Array item (Optional) |
| `raidday.shinies[].imageHeight` | integer | 1/3 | Array item (Optional) |
| `raidday.shinies[].imageWidth` | integer | 1/3 | Array item (Optional) |
| `raidday.shinies[].name` | string | 1/3 | Array item (Optional) |
| `raidday.specialMechanics` | array | 1/3 | Optional |
| `raidday.ticketBonuses` | array | 1/3 | Bonus information (Optional) |
| `raidday.ticketPrice` | null | 1/3 | Optional |


### raid-hour

**Total events in dataset**: 3

**ExtraData Structure:**

`extraData` contains: `raidhour`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `raidhour` | object | 3/3 |  |
| `raidhour.canBeShiny` | boolean | 3/3 | Shiny availability |
| `raidhour.featured` | object | 3/3 | Featured Pokémon |
| `raidhour.featured.canBeShiny` | boolean | 3/3 | Shiny availability |
| `raidhour.featured.image` | string | 3/3 | Image URL |
| `raidhour.featured.name` | string | 3/3 | Name/title |


### research

**Total events in dataset**: 1

**ExtraData Structure:**

`extraData` contains: `research`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `research` | object | 1/1 | Game content section |
| `research.description` | string | 1/1 | Description |
| `research.encounters` | array | 1/1 |  |
| `research.expires` | boolean | 1/1 |  |
| `research.isPaid` | boolean | 1/1 |  |
| `research.name` | string | 1/1 | Name/title |
| `research.price` | number | 1/1 | Price |
| `research.promoCodes` | array | 1/1 |  |
| `research.researchType` | string | 1/1 |  |
| `research.rewards` | array | 1/1 | Rewards |
| `research.tasks` | array | 1/1 | Task list |
| `research.tasks[].name` | string | 1/1 | Array item |
| `research.tasks[].rewards` | array | 1/1 | Array item |
| `research.tasks[].rewards[].image` | string | 1/1 | Array item |
| `research.tasks[].rewards[].imageHeight` | integer | 1/1 | Array item |
| `research.tasks[].rewards[].imageWidth` | integer | 1/1 | Array item |
| `research.tasks[].rewards[].text` | string | 1/1 | Array item |
| `research.tasks[].step` | integer | 1/1 | Array item |
| `research.tasks[].tasks` | array | 1/1 | Array item |
| `research.tasks[].tasks[].reward` | object | 1/1 | Array item |
| `research.tasks[].tasks[].reward.image` | string | 1/1 | Array item |
| `research.tasks[].tasks[].reward.imageHeight` | integer | 1/1 | Array item |
| `research.tasks[].tasks[].reward.imageWidth` | integer | 1/1 | Array item |
| `research.tasks[].tasks[].reward.text` | string | 1/1 | Array item |
| `research.tasks[].tasks[].text` | string | 1/1 | Array item |
| `research.webStoreInfo` | string | 1/1 |  |


### season

**Total events in dataset**: 1

**ExtraData Structure:**

`extraData` contains: `season`

| Field Path | Type | Occurrences | Notes |
|------------|------|-------------|-------|
| `season` | object | 1/1 |  |
| `season.bonuses` | array | 1/1 | Bonus information |
| `season.communityDays` | array | 1/1 |  |
| `season.eggs` | object | 1/1 | Game content section |
| `season.eggs.10km` | array | 1/1 |  |
| `season.eggs.12km` | array | 1/1 |  |
| `season.eggs.2km` | array | 1/1 |  |
| `season.eggs.2km[].canBeShiny` | boolean | 1/1 | Array item |
| `season.eggs.2km[].image` | string | 1/1 | Array item |
| `season.eggs.2km[].imageHeight` | integer | 1/1 | Array item |
| `season.eggs.2km[].imageWidth` | integer | 1/1 | Array item |
| `season.eggs.2km[].name` | string | 1/1 | Array item |
| `season.eggs.5km` | array | 1/1 |  |
| `season.eggs.7km` | array | 1/1 |  |
| `season.eggs.adventure` | array | 1/1 |  |
| `season.eggs.route` | array | 1/1 |  |
| `season.eggs.route[].canBeShiny` | boolean | 1/1 | Array item |
| `season.eggs.route[].image` | string | 1/1 | Array item |
| `season.eggs.route[].imageHeight` | integer | 1/1 | Array item |
| `season.eggs.route[].imageWidth` | integer | 1/1 | Array item |
| `season.eggs.route[].name` | string | 1/1 | Array item |
| `season.features` | array | 1/1 |  |
| `season.goBattleLeague` | string | 1/1 |  |
| `season.goPass` | array | 1/1 |  |
| `season.masterworkResearch` | array | 1/1 |  |
| `season.maxPokemonDebuts` | array | 1/1 |  |
| `season.name` | string | 1/1 | Name/title |
| `season.pokemonDebuts` | array | 1/1 |  |
| `season.researchBreakthrough` | array | 1/1 |  |
| `season.spawns` | array | 1/1 | Featured Pokémon |
| `season.specialResearch` | array | 1/1 |  |


### team-go-rocket

**Total events in dataset**: 1

**ExtraData**: `null` or not present

Events of this type typically do not contain additional structured data in `extraData`.


## Important Notes

1. **Field Availability**: Not all events of the same type will have identical `extraData` structures. Fields may be omitted when not applicable to a specific event.
2. **Array Structures**: Fields marked with `[]` indicate array items. The table shows the structure of individual array elements.
3. **Optional Fields**: The "Occurrences" column shows how many events of that type include each field. Less than 100% indicates an optional field.
4. **Dynamic Content**: The data structure evolves as new event types and features are added to the game.
5. **Historical Data**: Events are removed from the dataset once they end, so this documentation reflects currently active and upcoming events only.
6. **Multiple Active Events**: Multiple events can be active simultaneously (e.g., a season + weekly event + spotlight hour).

## Usage Recommendations

When consuming this API:

- Always check for field existence before accessing nested properties
- Use defensive programming for optional fields
- The `eventType` field is the primary discriminator for `extraData` structure
- Fields with low occurrence rates may be event-specific special features
- Implement proper error handling for missing or unexpected data structures

## Data Updates

The events feed is regularly updated to reflect:

- New event announcements
- Status changes (`upcoming` → `current`)
- Removal of expired events
- Updates to event details and `extraData`
- New event types as they are introduced to the game

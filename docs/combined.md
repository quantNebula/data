# Combined.min.json Architecture

## Overview

The `combined.min.json` file is a comprehensive aggregation of all Pokémon GO data endpoints. **This is an additional file created after all individual endpoint files are generated.** This document describes the architecture, generation process, and validation mechanisms to ensure the file is always complete and reliable.

**Important:** The individual files (`events.min.json`, `raids.min.json`, etc.) are always generated first. The `combined.min.json` file is created as a convenience by reading and aggregating these individual files.

## Architecture

### Data Sources

The combined file includes the following data sources:

| Key | File | Required | Description |
|-----|------|----------|-------------|
| `events` | events.min.json | ✓ | Game events, schedules, and event-specific features |
| `raids` | raids.min.json | ✓ | Current raid bosses across all tiers |
| `eggs` | eggs.min.json | ✓ | Pokémon available from eggs |
| `research` | research.min.json | ✓ | Field research tasks and rewards |
| `shiny` | shiny.min.json | ✓ | Shiny Pokémon availability and release dates |
| `rocketLineups` | rocketLineups.min.json | ✓ | Team GO Rocket leader lineups |

### File Structure

```json
{
  "events": [...],
  "raids": [...],
  "eggs": [...],
  "research": {
    "seasonalInfo": {...},
    "tasks": [...]
  },
  "shiny": [...],
  "rocketLineups": [...]
}
```

## Generation Process

### 1. Initial Scraping (scrape.js)

```bash
npm run scrape
```

The initial scrape:
1. Runs all individual scrapers in parallel
2. **Generates individual `.min.json` files** (events, raids, eggs, research, shiny, rocketLineups)
3. **Additionally creates** comprehensive `combined.min.json` by aggregating the individual files
4. Validates all sources are included in combined file
5. Reports any missing or invalid data sources

**Key Features:**
- Parallel scraping for efficiency
- Comprehensive validation of all sources
- Clear logging of included/missing sources
- Automatic combined file generation

### 2. Detailed Scraping (detailedscrape.js)

```bash
npm run detailedscrape
```

The detailed scrape:
1. Reads `events.min.json`
2. Scrapes additional event details
3. Writes temporary files to `files/temp/`
4. Does NOT generate combined file (that's next step)

### 3. Combining Details (combinedetails.js)

```bash
npm run combinedetails
```

The combine details process:
1. Reads `events.min.json`
2. Merges detailed data from `files/temp/`
3. **Updates `events.min.json`** (individual file)
4. **Regenerates comprehensive `combined.min.json`** by re-aggregating all individual files (with updated events)
5. Validates all data sources are included
6. Cleans up temp directory (only if successful)

**Note:** Both the individual `events.min.json` and the aggregated `combined.min.json` are updated.

**Key Features:**
- Uses override mechanism to include updated events
- Validates all other sources are still present
- Ensures combined file remains comprehensive
- Verbose logging for transparency

### 4. Complete Pipeline

```bash
npm run scrape:all
```

Runs the complete pipeline:
1. `scrape` - Initial data collection
2. `detailedscrape` - Event detail enhancement
3. `combinedetails` - Final aggregation and validation

## Centralized Generation Module

### combinedGenerator.js

The `combinedGenerator.js` module provides centralized logic for:

#### generateCombinedFile(options)

Generates comprehensive `combined.min.json` from all data sources.

**Options:**
- `overrides`: Object with data to override (e.g., `{ events: updatedEventsArray }`)
- `verbose`: Enable detailed logging

**Returns:**
```javascript
{
  success: boolean,      // Overall success status
  included: string[],    // Keys of included sources
  missing: string[],     // Keys of missing required sources
  invalid: string[],     // Keys with validation failures
  warnings: string[],    // Validation warnings
  outputPath: string,    // Path to generated file
  error: string          // Error message if failed
}
```

**Example:**
```javascript
const { generateCombinedFile } = require('./combinedGenerator');

// Generate with all files
generateCombinedFile({ verbose: true });

// Generate with updated events
generateCombinedFile({ 
  overrides: { events: updatedEventsArray },
  verbose: true 
});
```

#### validateCombinedFile()

Validates existing `combined.min.json` structure.

**Returns:**
```javascript
{
  exists: boolean,       // File exists
  valid: boolean,        // All validations pass
  sources: string[],     // Keys found in file
  missing: string[],     // Missing required sources
  issues: string[]       // Validation issues
}
```

#### getDataSourcesInfo()

Returns metadata about all data sources.

**Returns:**
```javascript
[
  {
    key: 'events',
    file: 'events.min.json',
    required: true,
    description: 'Game events, schedules, and event-specific features'
  },
  ...
]
```

## Validation

### Automatic Validation

During generation, the system:
1. Checks all required files exist
2. Validates JSON structure
3. Validates data structure (arrays, objects as expected)
4. Reports missing or invalid sources
5. Marks generation as failed if required sources missing

### Manual Validation

```bash
npm run validate:combined
```

The validation script:
1. Checks file existence
2. Validates all data sources present
3. Validates data structures
4. Reports comprehensive status
5. Exits with code 0 (pass) or 1 (fail)

**Example Output:**
```
==========================================
Validating combined.min.json
==========================================

File Status: EXISTS ✓

Expected Data Sources:
----------------------
✓ events               (required)
   Game events, schedules, and event-specific features
✓ raids                (required)
   Current raid bosses across all tiers
✓ eggs                 (required)
   Pokémon available from eggs
✓ research             (required)
   Field research tasks and rewards
✓ shiny                (required)
   Shiny Pokémon availability and release dates
✓ rocketLineups        (required)
   Team GO Rocket leader lineups

Validation Results:
-------------------
Sources included: 6/6
Missing required: 0
Issues found:     0

==========================================
✓ Validation PASSED - File is comprehensive
==========================================
```

## Error Handling

### Missing Sources

If required sources are missing:
- Generation continues with available sources
- Warning is logged: `⚠ Combined file incomplete! Missing: events, raids`
- Generation marked as failed
- Detailed report shows which sources missing

### Invalid Data

If data fails validation:
- Data is included anyway (non-fatal)
- Warning is logged
- Issue recorded in result

### Parse Errors

If JSON parsing fails:
- Source is skipped
- Error is logged
- If required, generation marked as failed

## Best Practices

### When Adding New Data Sources

1. **Update DATA_SOURCES** in `combinedGenerator.js`:
```javascript
{
    key: 'newSource',
    file: 'newSource.min.json',
    required: true,
    description: 'Description of new data',
    validateFn: (data) => Array.isArray(data) && data.length > 0
}
```

2. **Update Documentation**:
- Update this file
- Update `consuming-all-endpoints.md`

3. **Test Generation**:
```bash
npm run scrape:all
npm run validate:combined
```

### When Debugging

1. **Use verbose mode**:
```javascript
generateCombinedFile({ verbose: true });
```

2. **Check validation**:
```bash
npm run validate:combined
```

3. **Inspect individual files**:
```bash
cat files/events.min.json | jq length
cat files/raids.min.json | jq length
```

## CI/CD Integration

### Recommended Workflow

```yaml
- name: Scrape data
  run: npm run scrape:all

- name: Validate combined file
  run: npm run validate:combined

- name: Deploy if valid
  if: success()
  run: ./deploy.sh
```

### Exit Codes

- `0` - Success
- `1` - Failure (missing required sources or validation failed)

## Maintenance

### Regular Checks

1. **After updates**: Validate combined file
2. **Before deployment**: Run validation
3. **In CI/CD**: Automate validation

### Troubleshooting

**Combined file missing sources:**
```bash
# Re-run complete pipeline
npm run scrape:all

# Check validation
npm run validate:combined

# Inspect logs for errors
```

**Validation fails:**
```bash
# Check which sources are missing
npm run validate:combined

# Check individual files exist
ls -la files/*.min.json

# Manually regenerate
node -e "require('./combinedGenerator').generateCombinedFile({verbose:true})"
```

## Future Enhancements

Potential improvements:
- [ ] Add data source versioning
- [ ] Add checksums for integrity verification
- [ ] Add automatic retry on failure
- [ ] Add diff reporting between versions
- [ ] Add performance metrics
- [ ] Add webhook notifications on failure

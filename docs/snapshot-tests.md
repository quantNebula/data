# Snapshot Comparison Regression Tests

## Overview

The snapshot comparison regression tests are designed to detect unintended changes in the generated JSON data files and verify that no data loss occurs during scraping operations.

## What are Snapshot Tests?

Snapshot tests compare the current JSON output files against reference "snapshots" (known good versions). These tests help catch:

- **Unintended structural changes**: Changes to the JSON schema or data structure
- **Data loss**: Missing keys, reduced array lengths, or null values replacing data
- **Breaking changes**: Type mismatches or format changes that could break downstream consumers

## Files Tested

The following JSON files are compared against their snapshots:

- `events.min.json` - Game events and schedules
- `raids.min.json` - Current raid bosses
- `eggs.min.json` - Pokémon available from eggs
- `research.min.json` - Field and special research tasks
- `shiny.min.json` - Shiny Pokémon availability
- `rocketLineups.min.json` - Team GO Rocket leader lineups
- `combined.min.json` - Combined data file

## Running Snapshot Tests

### Run all tests including snapshots:
```bash
npm test
```

### Run only snapshot tests:
```bash
npm run test:snapshots
```

## Test Categories

The snapshot tests perform four categories of checks:

### 1. File Existence
Verifies that all required snapshot and current files exist.

### 2. Structural Integrity
Compares the structure of current files against snapshots to detect:
- Missing or extra keys in objects
- Changes in array lengths (especially decreases)
- Type mismatches
- Null values replacing data

### 3. Required Fields
Validates that essential fields are present in the data:
- Events: `eventID`, `name`, `eventType`, `start`, `end`
- Raids: `name`, `tier`
- Eggs: `name`, `eggType`
- Research: `tasks` array
- Shiny: `dexNumber`, `name`
- Rocket Lineups: `name`, `firstPokemon`
- Combined: All top-level sections

### 4. Data Consistency
Checks basic consistency of data structures:
- Arrays are not empty
- Required nested structures exist
- Data types are consistent

## Interpreting Test Results

### All tests pass ✅
```
✅ All snapshot comparison tests passed!
```
No structural changes or data loss detected. The current data matches the reference snapshots.

### Data loss detected ❌
```
✗ Data loss detected in raids.min.json:
  [high] Array length decreased at : 17 → 16
  [high] Missing keys at .events[0]: startTime
```
This indicates potential data loss. Review the changes carefully:
- If this is unintentional, fix the scraper
- If this is intentional, update the snapshots

### Structural changes but no data loss ✓
```
✓ Structural changes detected but no data loss: events.min.json
  (5 differences found - this may be expected)
```
The structure changed but no data was lost. This might be expected if:
- New fields were added
- Values changed (but keys remain)
- Array order changed (but length is same)

Review the changes and update snapshots if intentional.

## Updating Snapshots

When you make intentional changes to the data structure or scraping logic:

### Update all snapshots:
```bash
npm run update-snapshots
```

This copies all current JSON files from `files/` to `snapshots/`.

### When to update snapshots:
- After intentionally modifying the scraper logic
- When adding new fields or data
- When changing data formats intentionally
- After verifying that structural changes are correct

### When NOT to update snapshots:
- When tests fail due to bugs in your code
- When data loss is detected and unintended
- Without reviewing the differences first

## Best Practices

1. **Run tests before scraping**: Ensure your baseline is clean
   ```bash
   npm test
   ```

2. **Review differences carefully**: When tests fail, check what changed
   ```bash
   npm run test:snapshots
   ```

3. **Update snapshots intentionally**: Only update after verifying changes
   ```bash
   npm run update-snapshots
   ```

4. **Commit snapshot updates**: Include snapshot updates in your commits
   ```bash
   git add snapshots/
   git commit -m "Update snapshots for new event fields"
   ```

5. **Use snapshots as documentation**: Snapshots serve as examples of expected data structure

## Snapshot Directory Structure

```
snapshots/
├── events.min.json          # Reference snapshot for events
├── raids.min.json           # Reference snapshot for raids
├── eggs.min.json            # Reference snapshot for eggs
├── research.min.json        # Reference snapshot for research
├── shiny.min.json           # Reference snapshot for shiny data
├── rocketLineups.min.json   # Reference snapshot for rocket lineups
└── combined.min.json        # Reference snapshot for combined data
```

## Integration with CI/CD

The snapshot tests are part of the standard test suite (`npm test`) and will:
- Run automatically in CI/CD pipelines
- Fail the build if data loss is detected
- Provide clear error messages about what changed

## Troubleshooting

### Tests fail after scraping
This is expected! The data has changed. Review the changes:
1. Check if the changes are intentional
2. If yes, run `npm run update-snapshots`
3. If no, investigate why the scraper output changed

### Snapshots are missing
Run the scraper first to generate the data files:
```bash
npm run scrape:all
npm run update-snapshots
```

### False positives
If tests report changes that are actually fine (e.g., value changes that don't affect structure):
- This is expected behavior
- The test will show "no data loss" for these cases
- Update snapshots if the changes are intentional

## Related Documentation

- [Events Documentation](./events.md)
- [Raids Documentation](./raids.md)
- [Research Documentation](./research.md)
- [Eggs Documentation](./eggs.md)
- [Shiny Documentation](./shiny.md)
- [Rocket Lineups Documentation](./rocketLineups.md)

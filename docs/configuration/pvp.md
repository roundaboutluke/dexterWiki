# PvP Tracking

Dexter can calculate PvP (Player vs Player) rankings internally and alert users when Pokemon with good PvP IVs are found.

## Data Source

```json
{
  "pvp": {
    "dataSource": "internal"
  }
}
```

| Value | Description |
|-------|-------------|
| `"internal"` | Dexter calculates PvP ranks itself (recommended) |
| `"webhook"` | Use PvP data from webhook payload (requires scanner support) |

## Configuration

```json
{
  "pvp": {
    "levelCaps": [50],
    "includeMegaEvolution": false,
    "littleLeagueCanEvolve": false,
    "pvpEvolutionDirectTracking": false,
    "filterByTrack": false
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `levelCaps` | `[50]` | Array of level caps for rank calculations. Use `[50, 51]` for both regular and best-buddy |
| `includeMegaEvolution` | `false` | Include mega evolutions in PvP calculations |
| `littleLeagueCanEvolve` | `false` | Allow evolvable Pokemon in Little League |
| `pvpEvolutionDirectTracking` | `false` | Track evolved forms directly for PvP |
| `filterByTrack` | `false` | Only calculate PvP for Pokemon being tracked |

## Display Settings

```json
{
  "pvp": {
    "pvpDisplayMaxRank": 10,
    "pvpDisplayGreatMinCP": 1400,
    "pvpDisplayUltraMinCP": 2350,
    "pvpDisplayLittleMinCP": 450
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `pvpDisplayMaxRank` | `10` | Maximum rank to show in alert details |
| `pvpDisplayGreatMinCP` | `1400` | Minimum CP to display for Great League |
| `pvpDisplayUltraMinCP` | `2350` | Minimum CP to display for Ultra League |
| `pvpDisplayLittleMinCP` | `450` | Minimum CP to display for Little League |

## Filter Limits

These control what values users can set when creating PvP tracking filters:

```json
{
  "pvp": {
    "pvpFilterMaxRank": 5000,
    "pvpFilterGreatMinCP": 1000,
    "pvpFilterUltraMinCP": 2000,
    "pvpFilterLittleMinCP": 400
  }
}
```

## User-Level Configuration

| Option | Default | Description |
|--------|---------|-------------|
| `tracking.defaultUserTrackingLevelCap` | `0` | Default level cap for new user PvP tracking. `0` = all levels |

## PvP Tracking Commands

Users track PvP Pokemon with the `/pokemon` command and PvP options:

```
/pokemon pokemon:Azumarill pvp_league:Great pvp_ranks:1-50
```

This tracks Azumarill ranked 1–50 in Great League.

### DTS Helpers

PvP data is available in alert templates:

- `{{{pvpPokemon}}}` — PvP ranking details
- `{{pvpSlug nameEng}}` — URL-safe Pokemon name for PvP websites (e.g. Mr. Mime → Mr_Mime)

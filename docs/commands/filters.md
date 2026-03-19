# /filters

Show or remove your active tracking filters.

## Subcommands

### /filters show

Review all your current filters, optionally for a specific profile.

```
/filters show
```

| Option | Type | Description |
|--------|------|-------------|
| `profile` | String | Review filters for a specific profile. Supports autocomplete. |

### /filters remove

Remove one or more tracking filters.

```
/filters remove type:pokemon tracking:Pikachu IV90+
```

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `type` | Choice | Yes | Filter category to remove from |
| `tracking` | String | Yes | Specific filter to remove. Supports autocomplete. |
| `profile` | String | No | Profile to remove from. Supports autocomplete. |

The `type` option accepts the following values:

| Value | Description |
|-------|-------------|
| `pokemon` | Pokemon spawn filters |
| `raid` | Raid boss filters |
| `egg` | Raid egg filters |
| `maxbattle` | Max battle filters |
| `rocket` | Team Rocket filters |
| `pokestop-event` | Pokestop event filters |
| `quest` | Quest reward filters |
| `gym` | Gym change filters |
| `weather` | Weather filters |
| `lure` | Lure filters |
| `nest` | Nest filters |
| `fort` | Fort/POI change filters |

## Examples

Show all filters on your active profile:

```
/filters show
```

Show filters for a specific profile:

```
/filters show profile:Work
```

Remove a specific Pokemon filter:

```
/filters remove type:pokemon tracking:Pikachu IV90+
```

Remove a raid filter from a named profile:

```
/filters remove type:raid tracking:Mewtwo profile:Home
```

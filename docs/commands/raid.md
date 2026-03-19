# /raid

Track raids by boss, level, or egg. This command uses subcommands to distinguish between the three tracking modes.

## Subcommands

### /raid boss

Track a specific raid boss Pokemon.

```
/raid boss pokemon:Mewtwo distance:1000
```

| Option | Type | Description |
|--------|------|-------------|
| `pokemon` | String | Raid boss name or number. Supports autocomplete. |
| `gym` | String | Specific gym name. Supports autocomplete. |
| `team` | Choice | Controlling team: `All`, `Valor (Red)`, `Mystic (Blue)`, `Instinct (Yellow)`, `Uncontested (White)` |
| `rsvp` | Choice | RSVP matching: `off`, `on`, `only` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the raid expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

### /raid level

Track all raids of a given level.

```
/raid level level:5 distance:500
```

| Option | Type | Description |
|--------|------|-------------|
| `level` | String | Raid level or Pokemon name. Supports autocomplete. |
| `gym` | String | Specific gym name. Supports autocomplete. |
| `team` | Choice | Controlling team: `All`, `Valor (Red)`, `Mystic (Blue)`, `Instinct (Yellow)`, `Uncontested (White)` |
| `rsvp` | Choice | RSVP matching: `off`, `on`, `only` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the raid expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

### /raid egg

Track raid eggs by level (before the boss hatches).

```
/raid egg level:5
```

| Option | Type | Description |
|--------|------|-------------|
| `level` | String | Egg level. Supports autocomplete. |
| `gym` | String | Specific gym name. Supports autocomplete. |
| `team` | Choice | Controlling team: `All`, `Valor (Red)`, `Mystic (Blue)`, `Instinct (Yellow)`, `Uncontested (White)` |
| `rsvp` | Choice | RSVP matching: `off`, `on`, `only` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the egg expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Shared Options

All three subcommands share the `team`, `rsvp`, `gym`, `distance`, `clean`, `template`, and `profile` options.

- **team** -- Filter raids at gyms controlled by a specific team.
- **rsvp** -- Control RSVP behavior. Set to `on` to include RSVP details in the alert, `only` to receive alerts only when RSVPs are present, or `off` to ignore RSVPs.

## Examples

Track Mega Rayquaza raids within 2 km:

```
/raid boss pokemon:Rayquaza distance:2000
```

Track all level 5 raid eggs at Mystic gyms:

```
/raid egg level:5 team:Mystic (Blue)
```

Track level 1 raids at a specific gym:

```
/raid level level:1 gym:Central Park Fountain
```

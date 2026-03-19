# /gym

Track gym team changes and slot changes. Useful for monitoring gym control and defender activity.

## Usage

```
/gym team:Mystic (Blue) distance:500
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `team` | Choice | Team filter: `Everything`, `Mystic (Blue)`, `Valor (Red)`, `Instinct (Yellow)`, `Uncontested`, `Normal`. Leave blank for a guided flow. |
| `gym` | String | Specific gym name. Supports autocomplete. |
| `slot_changes` | Boolean | Alert when defenders are added or removed |
| `battle_changes` | Boolean | Alert when a gym is being battled (requires `enableGymBattle` in config) |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the event expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

!!! note
    The `battle_changes` option is only available when the administrator has enabled `tracking.enableGymBattle` in the server configuration.

## Examples

Track all gym team changes within 1 km:

```
/gym team:Everything distance:1000
```

Track slot changes at a specific gym:

```
/gym gym:Central Park Fountain slot_changes:True
```

Track when Valor takes a gym anywhere:

```
/gym team:Valor (Red)
```

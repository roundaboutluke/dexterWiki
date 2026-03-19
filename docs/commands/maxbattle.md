# /maxbattle

Track Dynamax and Gigantamax battles at Power Spots. This command uses subcommands to filter by boss or level.

## Subcommands

### /maxbattle boss

Track max battles featuring a specific Pokemon.

```
/maxbattle boss pokemon:Bulbasaur distance:1000
```

| Option | Type | Description |
|--------|------|-------------|
| `pokemon` | String | Pokemon name or number. Supports autocomplete. |
| `station` | String | Specific Power Spot station name. Supports autocomplete. |
| `gmax_only` | Boolean | Only match Gigantamax battles |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the battle expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

### /maxbattle level

Track max battles by difficulty level.

```
/maxbattle level level:5
```

| Option | Type | Description |
|--------|------|-------------|
| `level` | String | Max battle level or Pokemon name. Supports autocomplete. |
| `station` | String | Specific Power Spot station name. Supports autocomplete. |
| `gmax_only` | Boolean | Only match Gigantamax battles |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the battle expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Examples

Track all Gigantamax battles within 2 km:

```
/maxbattle level gmax_only:True distance:2000
```

Track max battles featuring Pikachu at any distance:

```
/maxbattle boss pokemon:Pikachu
```

Track level 3 max battles with auto-cleanup:

```
/maxbattle level level:3 clean:Enable
```

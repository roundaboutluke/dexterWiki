# /nest

Track Pokemon nests. Receive notifications when a specific Pokemon is nesting within your configured distance or area.

## Usage

```
/nest pokemon:Gible distance:5000
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `pokemon` | String | Pokemon name or number. Supports autocomplete. |
| `min_spawn` | Integer | Minimum average spawns per hour to trigger a notification |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the nest rotation |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Examples

Track Gible nests with at least 5 average spawns:

```
/nest pokemon:Gible min_spawn:5
```

Track any nest within 10 km:

```
/nest distance:10000
```

Track Dratini nests with auto-cleanup:

```
/nest pokemon:Dratini clean:Enable
```

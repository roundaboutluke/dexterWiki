# /rocket

Track Team GO Rocket invasions at Pokestops, including grunts, leaders, and Giovanni.

## Usage

```
/rocket type:Water Grunt distance:500
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `type` | String | Grunt or leader type (e.g. Water Grunt, Cliff, Giovanni). Supports autocomplete. |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the invasion expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Examples

Track all Team Rocket invasions within 1 km:

```
/rocket distance:1000
```

Track Giovanni sightings anywhere:

```
/rocket type:Giovanni
```

Track Dragon-type grunts with auto-cleanup:

```
/rocket type:Dragon Grunt clean:Enable
```

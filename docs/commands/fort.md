# /fort

Track changes to Pokestops and Gyms (points of interest). This includes new POIs, removals, name changes, photo changes, and location changes.

## Usage

```
/fort type:Gym new:True
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `type` | Choice | POI type: `Everything`, `Pokestop`, `Gym`. Leave blank for a guided flow. |
| `new` | Boolean | Track newly added POIs |
| `removal` | Boolean | Track POI removals |
| `name` | Boolean | Track name changes |
| `photo` | Boolean | Track photo changes |
| `location` | Boolean | Track location changes |
| `include_empty` | Boolean | Include POIs with no name or photo |
| `distance` | Integer | Maximum distance in meters |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Examples

Track all new Pokestops and Gyms within 5 km:

```
/fort type:Everything new:True distance:5000
```

Track gym removals:

```
/fort type:Gym removal:True
```

Track name and photo changes for all POIs:

```
/fort type:Everything name:True photo:True
```

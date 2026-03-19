# /pokestop-event

Track Pokestop events (also known as incidents). These are special interactions at Pokestops such as showcases, routes, or other limited-time events.

## Usage

```
/pokestop-event type:Showcase distance:1000
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `type` | String | Event type. Supports autocomplete. |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the event expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Examples

Track all Pokestop events within 2 km:

```
/pokestop-event distance:2000
```

Track a specific event type with auto-cleanup:

```
/pokestop-event type:Showcase clean:Enable
```

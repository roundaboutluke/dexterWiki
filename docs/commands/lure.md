# /lure

Track lure modules placed on Pokestops.

## Usage

```
/lure type:Glacial distance:500
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `type` | Choice | Lure type: `Everything`, `Basic`, `Glacial`, `Mossy`, `Magnetic`, `Rainy`, `Golden`. Leave blank for a guided flow. |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after the lure expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Examples

Track all lure types within 1 km:

```
/lure type:Everything distance:1000
```

Track Golden lures anywhere:

```
/lure type:Golden
```

Track Glacial lures with auto-cleanup:

```
/lure type:Glacial clean:Enable
```

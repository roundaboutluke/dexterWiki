# Static Maps

Dexter can include map images in alerts showing the location of Pokemon, raids, quests, etc.

## Providers

| Provider | Config Value | Description |
|----------|-------------|-------------|
| None | `"none"` | No map images |
| TileServerCache | `"tileservercache"` | Self-hosted tile server (recommended) |
| Google Maps | `"google"` | Google Static Maps API |
| Mapbox | `"mapbox"` | Mapbox Static Images API |
| OpenStreetMap | `"osm"` | OpenStreetMap-based static maps |

```json
{
  "geocoding": {
    "staticProvider": "tileservercache",
    "staticProviderURL": "http://your-tileserver:8080"
  }
}
```

## TileServerCache

TileServerCache is the recommended provider. It supports both simple static maps and multi-static maps (showing multiple points of interest).

### Template Prefix

Dexter uses a configurable template prefix for TileServerCache templates:

```json
{
  "geocoding": {
    "tileserverTemplatePrefix": "dexter-"
  }
}
```

This means your TileServerCache templates should be named like:

| Template | Name |
|----------|------|
| Monster | `dexter-monster` |
| Raid | `dexter-raid` |
| Quest | `dexter-quest` |
| Pokestop | `dexter-pokestop` |
| Gym | `dexter-gym` |
| Nest | `dexter-nest` |
| Weather | `dexter-weather` |
| Max Battle | `dexter-maxbattle` |
| Fort Update | `dexter-fort-update` |
| Area Overview | `dexter-area` / `dexter-areaoverview` |
| Distance | `dexter-distance` |
| Location | `dexter-location` |

For multi-static maps (e.g. weather change with affected Pokemon), the prefix is applied before `multi-`:

| Multi Template | Name |
|----------------|------|
| Multi Monster | `dexter-multi-monster` |
| Multi Raid | `dexter-multi-raid` |
| Multi Weather | `dexter-multi-weather` |

!!! tip "Backwards compatibility"
    If migrating from PoracleJS, set `"tileserverTemplatePrefix": "poracle-"` to reuse your existing templates.

### Per-Type Overrides

Override the template name for specific types:

```json
{
  "geocoding": {
    "tileserverTemplates": {
      "monster": "custom-monster-template",
      "raid": "custom-raid-template"
    }
  }
}
```

### TileServer Settings

Configure map dimensions, zoom, and rendering per alert type:

```json
{
  "geocoding": {
    "tileserverSettings": {
      "default": {
        "type": "staticMap",
        "width": 500,
        "height": 350,
        "zoom": 15,
        "pregenerate": false,
        "includeStops": false
      },
      "monster": {
        "type": "staticMap",
        "width": 500,
        "height": 350,
        "zoom": 16
      },
      "weather": {
        "type": "multiStaticMap",
        "width": 600,
        "height": 400,
        "zoom": 13
      }
    }
  }
}
```

| Setting | Default | Description |
|---------|---------|-------------|
| `type` | `"staticMap"` | `staticMap` or `multiStaticMap` |
| `width` | `500` | Map width in pixels |
| `height` | `350` | Map height in pixels |
| `zoom` | `15` | Map zoom level |
| `pregenerate` | `false` | Pre-generate the tile (faster delivery, uses more storage) |
| `includeStops` | `false` | Include nearby pokestops on the map |

### Map Styles

Different styles can be used for different times of day:

```json
{
  "geocoding": {
    "dayStyle": "",
    "dawnStyle": "",
    "duskStyle": "",
    "nightStyle": ""
  }
}
```

## Google Maps

```json
{
  "geocoding": {
    "staticProvider": "google",
    "staticKey": ["YOUR_GOOGLE_API_KEY"]
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `width` | `320` | Map width |
| `height` | `200` | Map height |
| `zoom` | `15` | Zoom level |
| `scale` | `2` | Image scale (1 or 2) |
| `type` | `"klokantech-basic"` | Map style |

## Mapbox

```json
{
  "geocoding": {
    "staticProvider": "mapbox",
    "staticKey": ["YOUR_MAPBOX_TOKEN"]
  }
}
```

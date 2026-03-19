# Geofence API

## Generate Geofence Map

Returns a static map image URL showing the outline of a named geofence area.

```http
GET /api/geofence/{name}/map
```

### Response

```json
{
  "status": "ok",
  "url": "https://tiles.example.com/staticmap/pregenerated/abc123"
}
```

## Distance Map

Returns a static map image URL showing a distance radius from a location.

```http
GET /api/geofence/distance/map?lat=51.5&lon=-0.1&d=1000
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `lat` | Latitude |
| `lon` | Longitude |
| `d` | Distance in meters |

### Response

```json
{
  "status": "ok",
  "url": "https://tiles.example.com/staticmap/pregenerated/def456"
}
```

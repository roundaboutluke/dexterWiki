# Humans API

Manage user profiles, locations, and tracking filters.

## Get User

```http
GET /api/humans/{id}
```

### Response

```json
{
  "status": "ok",
  "human": {
    "id": "123456789",
    "name": "username",
    "type": "discord:user",
    "enabled": true,
    "latitude": 51.5,
    "longitude": -0.1,
    "area": "[\"Downtown\"]"
  }
}
```

## Set Location

```http
POST /api/humans/{id}/setLocation
Content-Type: application/json

{
  "latitude": 51.5074,
  "longitude": -0.1278
}
```

### Response

```json
{
  "status": "ok"
}
```

## Validate Location

Check if a location falls within any configured geofence areas.

```http
POST /api/humans/validateLocation
Content-Type: application/json

{
  "latitude": 51.5074,
  "longitude": -0.1278
}
```

### Response

```json
{
  "status": "ok",
  "areas": ["Downtown", "City Centre"],
  "valid": true
}
```

## Get Tracking Filters

```http
GET /api/humans/{id}/tracking
```

### Response

Returns all active tracking filters for the user, grouped by type (pokemon, raid, quest, etc.).

## Set Tracking Filter

```http
POST /api/humans/{id}/tracking
Content-Type: application/json

{
  "type": "pokemon",
  "pokemon_id": 25,
  "min_iv": 90,
  "distance": 1000
}
```

## Remove Tracking Filter

```http
DELETE /api/humans/{id}/tracking/{trackingId}
```

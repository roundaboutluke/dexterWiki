# Geocoding

Geocoding provides human-readable addresses for alert locations. Dexter supports reverse geocoding (coordinates → address) for use in alert templates.

## Providers

| Provider | Config Value | Description |
|----------|-------------|-------------|
| None | `"none"` | No geocoding |
| Nominatim | `"nominatim"` | Free, OpenStreetMap-based (recommended) |
| Pelias | `"pelias"` | Self-hosted geocoder with advanced features |
| Google | `"google"` | Google Maps Geocoding API |

## Nominatim

Free and requires no API key. Rate-limited by the public service.

```json
{
  "geocoding": {
    "provider": "nominatim",
    "providerURL": "https://nominatim.openstreetmap.org/reverse"
  }
}
```

!!! tip "Self-hosted Nominatim"
    For high-volume setups, consider [self-hosting Nominatim](https://nominatim.org/release-docs/latest/admin/Installation/) to avoid rate limits.

## Pelias

Self-hosted geocoder with more control over results:

```json
{
  "geocoding": {
    "provider": "pelias",
    "providerURL": "http://your-pelias:4000/v1/reverse"
  }
}
```

### Pelias Options

| Option | Default | Description |
|--------|---------|-------------|
| `peliasLayers` | `""` | Comma-separated layers to search (e.g. `"address,street,neighbourhood"`) |
| `peliasPreferredLayer` | `""` | Preferred result layer |
| `peliasResultSize` | `1` | Number of results to fetch |
| `peliasBoundaryCountry` | `""` | Restrict results to a country code (e.g. `"US"`) |

## Google

```json
{
  "geocoding": {
    "provider": "google",
    "geocodingKey": ["YOUR_API_KEY"]
  }
}
```

## Address Format

Customise how addresses appear in alerts:

```json
{
  "geocoding": {
    "addressFormat": "{{{streetName}}} {{streetNumber}}"
  }
}
```

Available fields depend on the geocoding provider but typically include: `streetName`, `streetNumber`, `neighbourhood`, `city`, `county`, `state`, `country`, `postcode`.

## Geocoding Cache

Dexter caches geocoding results to reduce API calls. The cache is stored at `.cache/geocoderCache.json`.

| Option | Default | Description |
|--------|---------|-------------|
| `cacheDetail` | `3` | Decimal places for coordinate rounding in cache keys. Lower = more cache hits, less precision |
| `forwardOnly` | `false` | Only perform forward geocoding (address → coordinates), skip reverse |

## Street Intersections

For more descriptive locations, configure [GeoNames](http://www.geonames.org/) usernames for street intersection lookups:

```json
{
  "geocoding": {
    "intersectionUsers": ["your_geonames_username"]
  }
}
```

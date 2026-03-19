# API

Dexter provides a REST API for external tools and integrations. The API must be enabled by setting an API secret in the configuration.

## Enabling the API

```json
{
  "server": {
    "apiSecret": "your-secret-key"
  }
}
```

!!! warning
    If `apiSecret` is empty, the API is disabled. Choose a strong, random secret.

## Authentication

All API requests must include the secret as a query parameter or header:

```
GET http://localhost:3030/api/config/poracleWeb?secret=your-secret-key
```

## Endpoints

| Section | Description |
|---------|-------------|
| [Configuration](config.md) | Read server configuration |
| [Geofence](geofence.md) | Generate geofence maps and distance maps |
| [Humans](humans.md) | Manage user profiles, locations, and tracking |

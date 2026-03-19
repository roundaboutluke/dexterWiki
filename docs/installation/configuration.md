# Basic Configuration

Dexter uses a layered configuration system:

- `config/default.json` — built-in defaults (do not modify)
- `config/local.json` — your overrides (created on first start)

Dexter reads `default.json` first, then merges `local.json` on top. You only need to include settings you want to change in `local.json`.

!!! tip "JSONC support"
    Config files support JSON with comments (`//` and `/* */`), so you can annotate your settings.

## Firewall

Dexter listens on port `3030` by default. If your scanner sends webhooks from another machine, ensure this port is accessible. For local-only setups, no firewall changes are needed.

!!! warning
    If exposing Dexter to the internet, use a reverse proxy (nginx, caddy) with TLS. Do not expose Dexter directly.

## Geofence

The geofence file at `config/geofence.json` defines named areas for tracking. Create geofences using a tool like [Fence Editor](http://geo.jasparke.net/):

1. Click **Create** and enter a name (no hyphens, spaces are OK)
2. Draw the fence by clicking vertices on the map
3. Close the polygon by clicking the starting point
4. Repeat for all areas
5. Click **Save** → **Click to Download**

Place the downloaded file at `config/geofence.json`.

## Database

In `config/local.json`, set your database connection:

```json
{
  "database": {
    "client": "mysql",
    "conn": {
      "host": "127.0.0.1",
      "database": "dexter",
      "user": "dexteruser",
      "password": "yourStrongPassword",
      "port": 3306
    }
  }
}
```

### Scanner Database (optional)

If you want Dexter to look up pokestop names or other scanner data:

```json
{
  "database": {
    "scannerType": "golbat",
    "scanner": {
      "host": "127.0.0.1",
      "database": "golbat",
      "user": "readonlyuser",
      "password": "password",
      "port": 3306
    }
  }
}
```

Supported scanner types: `golbat`, `rdm`, `mad`, `none`

## Notification Service

Configure at least one:

- [Discord](discord-bot.md) — set `discord.enabled: true` and add your bot token
- [Telegram](telegram-bot.md) — set `telegram.enabled: true` and add your bot token

## Geocoding (optional)

To include addresses and maps in alerts, configure a geocoding provider:

```json
{
  "geocoding": {
    "provider": "nominatim",
    "providerURL": "https://nominatim.openstreetmap.org/reverse"
  }
}
```

See [Geocoding Configuration](../configuration/geocoding.md) for all providers.

## Validating Your Config

Dexter validates your config on startup. If the file is invalid JSON, it will log an error and exit. You can check your file with any JSON linter, keeping in mind that JSONC comments are allowed.

## Next Steps

- [Scanner Setup](scanner.md) — configure webhook delivery
- [Full Config Reference](../configuration/config-reference.md) — all available settings

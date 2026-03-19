# Migration from PoracleJS

Dexter is a Go rewrite of PoracleJS with significant enhancements. This guide covers migrating from an existing PoracleJS installation.

## Database Compatibility

Dexter uses the same database schema as PoracleJS. In most cases, you can point Dexter at an existing PoracleJS database and it will work immediately.

!!! tip "Back up first"
    Always back up your database before migrating: `mysqldump -u root -p poracle > poracle_backup.sql`

## Configuration Mapping

Dexter uses the same `config/local.json` format. Most PoracleJS configuration keys work unchanged. Key differences:

| PoracleJS | Dexter | Notes |
|-----------|--------|-------|
| `npm install` | `go build ./cmd/dexter` | Build step |
| `pm2 start poracle.js` | `./dexter` | Run command |
| Node.js 16+ | Go 1.25+ | Runtime requirement |
| Worker threads | Goroutines | Concurrency model (transparent) |

### New Configuration Keys

These are Dexter-specific additions not present in PoracleJS:

| Key | Default | Description |
|-----|---------|-------------|
| `general.diademURL` | `""` | Base URL for Diadem map links in DTS |
| `general.timezoneDbPath` | `""` | Path to timezone database for per-location scheduling |
| `general.timezoneDbType` | `"boltdb"` | Timezone database backend type |
| `general.timezoneDbEncoding` | `"msgpack"` | Encoding format for timezone database |
| `general.timezoneDbSnappy` | `true` | Snappy compression for timezone database |
| `discord.disableCommandResponses` | `false` | Suppress bot replies to legacy `!` commands |
| `discord.slash.disabled` | `false` | Disable Discord slash command registration |
| `discord.slash.deregisterOnStart` | `false` | Delete and re-register all slash commands on startup |
| `discord.slash.hideTemplateOptions` | `true` | Hide the template option from slash command forms |
| `geocoding.providerKey` | `""` | API key for the geocoding provider (e.g. Pelias) |
| `geocoding.peliasLayers` | `""` | **(Pelias)** CSV list of layers to request (e.g. `venue,address,street`) |
| `geocoding.peliasPreferredLayer` | `""` | **(Pelias)** Prefer results matching this layer |
| `geocoding.peliasResultSize` | `0` | **(Pelias)** Number of results to request |
| `geocoding.peliasBoundaryCountry` | `""` | **(Pelias)** Country code filter (e.g. `GB`) |
| `geocoding.tileserverTemplatePrefix` | `"dexter-"` | Prefix for tileservercache template names |
| `geocoding.tileserverTemplates` | `{}` | Per-type template name overrides (bypasses prefix) |
| `fallbacks.imgUrlStation` | *(GitHub URL)* | Fallback station icon URL |

## TileServer Templates

PoracleJS uses the template prefix `poracle-` (e.g. `poracle-monster`, `poracle-raid`). Dexter defaults to `dexter-` but you can keep your existing templates:

```json
{
  "geocoding": {
    "tileserverTemplatePrefix": "poracle-"
  }
}
```

This way you don't need to rename or duplicate your TileServerCache templates.

## Slash Commands

Dexter has full slash command support. If migrating from a PoracleJS setup that used `!` prefix commands:

1. Slash commands are registered automatically on first start
2. Legacy `!` prefix commands still work alongside slash commands
3. Users don't need to re-register — their existing profiles and filters carry over
4. Set `discord.slash.deregisterOnStart: true` on first launch to cleanly register all commands

## DTS Templates

Dexter's DTS system is compatible with PoracleJS templates. Your existing `config/dts.json` should work as-is.

### New DTS Helpers

Dexter adds these Handlebars helpers not available in PoracleJS:

| Helper | Description |
|--------|-------------|
| `{{pvpSlug name}}` | URL-safe Pokemon name (e.g. `Mr. Mime` → `Mr_Mime`) |
| `{{lowercase value}}` | Lowercase a string |

## Migration Steps

1. **Back up** your PoracleJS database
2. **Stop** PoracleJS
3. **Build** Dexter: `go build -o dexter ./cmd/dexter`
4. **Copy** your `config/local.json` and `config/dts.json` to the Dexter directory
5. **Set** `tileserverTemplatePrefix` to `"poracle-"` if reusing templates
6. **Start** Dexter: `./dexter`
7. **Verify** alerts are flowing, slash commands work, existing user filters are active

## Feature Additions

After migrating, you can take advantage of Dexter-specific features:

- [Scheduling & Profiles](configuration/scheduling.md) — active/quiet hours, quest digest
- [Max Battle Tracking](commands/maxbattle.md) — Dynamax battle alerts
- [Raid RSVP](operation/raid-rsvp.md) — in-place raid post updates
- [Slash Commands](commands/index.md) — full autocomplete and localisation
- Changed Pokemon alerts — know when a spawn changes mid-lifespan
- Quest AR/No-AR filtering

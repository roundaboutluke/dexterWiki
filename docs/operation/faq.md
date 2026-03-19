# Frequently Asked Questions

## Installation

### What Go version do I need?

Dexter requires Go 1.25 or later. Check with `go version`.

### Can I run Dexter alongside PoracleJS?

Yes, but they should use different databases and different bot tokens. They can both receive webhooks from the same scanner — just add both endpoints to your scanner's webhook config.

### How do I validate my config file?

Dexter validates the config on startup. If the file is invalid, it will log the error and exit. Config files support JSON with comments (JSONC).

## Configuration

### How do I test geocoding?

Set up a tracking filter and trigger an alert. The alert should include address information if geocoding is configured correctly. Check the logs for geocoding errors.

### What's the difference between `locale` and `language`?

- `general.locale` — the primary language for the bot (e.g. `"en"`, `"de"`)
- `locale.language` — the secondary/"alt" language for DTS templates (allows showing names in two languages)

### How do I increase throughput?

1. Add worker bot tokens: `discord.token: ["main", "worker1", "worker2"]`
2. Increase processing workers: `tuning.webhookProcessingWorkers`
3. Increase concurrent destinations: `tuning.concurrentDiscordDestinationsPerBot`
4. Ensure `tuning.fastMonsters` is `true` (default)

## Tracking

### Why am I not receiving alerts?

1. Check you're registered (`/start`)
2. Check you have a location set (`/profile location set`)
3. Check you have active filters (`/filters show`)
4. Verify the alerts aren't being suppressed by quiet hours
5. Check the bot has permission to DM you (Discord privacy settings)

### How does distance tracking work?

When you set a location and specify a distance on a tracking command, Dexter only sends alerts for events within that radius of your location. If you don't specify a distance, it defaults to `tracking.defaultDistance` (0 = unlimited, area-based only).

### What does "everything" do?

The `everything` keyword in tracking commands matches all Pokemon/raids/etc. Use with caution — it can generate a lot of alerts. The admin can control this with `tracking.everythingFlagPermissions`.

### How do I track PvP Pokemon?

Use the PvP options on `/pokemon`:

```
/pokemon pokemon:Azumarill pvp_league:Great pvp_ranks:1-50
```

See [PvP Tracking](../configuration/pvp.md) for configuration.

## Operations

### How do I check if Dexter is healthy?

Check the logs for regular webhook processing activity. The process should show steady memory usage without growth.

### How do I back up user data?

Back up your MySQL/MariaDB database regularly. The `humans` and tracking tables contain all user configuration.

### Can I run multiple instances?

Yes, for different communities or servers. Each instance needs its own database and bot token.

### How do I handle duplicate alerts?

Dexter deduplicates incoming webhooks automatically. If you're seeing duplicates, check that your scanner isn't sending the same webhook to Dexter twice.

## Migration

### I'm migrating from PoracleJS — is the database compatible?

Dexter uses the same database schema as PoracleJS. In most cases you can point Dexter at an existing PoracleJS database. See [Migration Guide](../migration.md) for details.

### Can I reuse my PoracleJS tileserver templates?

Yes — set `"tileserverTemplatePrefix": "poracle-"` to use your existing templates without renaming them.

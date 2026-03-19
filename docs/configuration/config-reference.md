# Config Reference

This is the complete reference for every setting in Dexter's `config/default.json`. You should never edit `default.json` directly -- instead, copy any sections you want to change into `config/local.json`. Dexter merges `local.json` on top of `default.json` at startup.

!!! tip "Validating your config"
    After editing `local.json`, make sure it is valid JSON before starting Dexter. The default file uses cJSON-style comments which standard JSON linters will reject -- your `local.json` should be plain JSON without comments.

---

## server

Controls the webhook listener that receives data from your scanner.

| Option | Default | Description |
|--------|---------|-------------|
| `host` | `"127.0.0.1"` | Interface to listen on. Use `127.0.0.1` for localhost only, or `0.0.0.0` to listen on all network interfaces. |
| `port` | `"3030"` | Port the webhook listener binds to. |
| `ipWhitelist` | `[]` | Array of IP addresses allowed to send webhooks. Empty means all IPs are allowed (subject to blacklist). |
| `ipBlacklist` | `[]` | Array of IP addresses blocked from sending webhooks. |
| `apiSecret` | `""` | Secret token required to access the Dexter API. Leave blank to disable the API entirely. |

---

## general

Core runtime settings that affect how Dexter processes webhooks and generates alerts.

| Option | Default | Description |
|--------|---------|-------------|
| `environment` | `"production"` | Runtime environment. Leave as `production` unless you are developing Dexter itself. |
| `alertMinimumTime` | `120` | Minimum seconds remaining before despawn for an alert to fire. Events expiring sooner than this are silently dropped. |
| `ignoreLongRaids` | `false` | When `true`, raids longer than 47 minutes are ignored. Useful for suppressing spam during raid-hour or special events. |
| `imgUrl` | *(UICONS repo URL)* | Base URL for the `{{imgUrl}}` DTS placeholder. Should point to a UICONS-compatible icon repository. |
| `imgUrlAlt` | `""` | Secondary icon base URL exposed as `{{imgUrlAlt}}` in DTS. |
| `stickerUrl` | *(Telegram UICONS repo)* | Base URL for `{{stickerUrl}}` in DTS, used for Telegram `.webp` stickers. |
| `images` | `{}` | Per-type overrides for the UICONS icon repository. Keys are controller types: `monster`, `gym`, `nest`, `invasion`, `lure`, `quest`, `raid`, `weather`. Values are repository URLs. |
| `stickers` | `{}` | Per-type overrides for the UICONS sticker repository (same key format as `images`). |
| `requestShinyImages` | `false` | When `true`, request shiny-variant icons from the UICONS repository. |
| `populatePokestopName` | `false` | **(RDM only)** Look up nearby Pokestop names in the scanner database for wild Pokemon encounters. |
| `locale` | `"en"` | Default language for Dexter. Supported values include `en`, `de`, `fr`, `it`, `ru`, and others with locale packs. |
| `disabledCommands` | `[]` | Array of command names to disable (e.g. `["nest", "lure"]`). Disabled commands will not respond to users. |
| `disablePokemon` | `false` | Disable processing of Pokemon webhooks entirely. |
| `disableRaid` | `false` | Disable processing of raid webhooks. |
| `disablePokestop` | `false` | Disable processing of Pokestop webhooks. On RDM/Golbat this also disables invasion processing. |
| `disableInvasion` | `false` | Disable processing of invasion webhooks. |
| `disableLure` | `false` | Disable processing of lure webhooks. |
| `disableQuest` | `false` | Disable processing of quest webhooks. |
| `disableWeather` | `false` | Disable processing of weather webhooks. |
| `disableNest` | `false` | Disable processing of nest webhooks. |
| `disableGym` | `false` | Disable processing of gym webhooks. |
| `disableFortUpdate` | `false` | Disable processing of fort-update webhooks. |
| `processConfirmedInvasionLineups` | `false` | **(Golbat)** Process confirmed invasion lineup data from Golbat webhooks. |
| `disableUnconfirmedInvasion` | `false` | When `true`, only process invasions that have confirmed lineup data. |
| `roleCheckMode` | `"ignore"` | Action to take when a user loses their required role. `"ignore"` -- log only. `"delete"` -- remove user and all trackings from the database. `"disable-user"` -- disable the user (preserving trackings) and re-enable if the role is restored. |
| `availableLanguages` | `{}` | Explicit allowlist of languages users can switch to with the `!language` command. When empty, Dexter auto-discovers languages from locale packs. See [Languages](languages.md) for details. |
| `defaultTemplateName` | `1` | Default DTS template ID or name assigned to new trackings. |
| `dtsDictionary` | `{}` | Library of custom DTS template snippets available in alert templates. |
| `shortlinkProvider` | `"hideuri"` | URL shortener provider. Supported: `hideuri`, `shlink`, `yourls`. |
| `shortlinkProviderURL` | `""` | Base URL for self-hosted shortlink providers (Shlink, YOURLS). |
| `shortlinkProviderKey` | `""` | API key for the shortlink provider. |
| `shortlinkProviderDomain` | `""` | Custom domain for the shortlink provider, if supported. |
| `rdmURL` | `""` | Base URL for RDM map links in DTS (e.g. `https://myRDM.com/`). |
| `reactMapURL` | `""` | Base URL for ReactMap links in DTS (e.g. `https://myReactMap.com/`). |
| `diademURL` | `""` | Base URL for Diadem links in DTS (e.g. `https://myDiadem.com/`). Do not include `/map`. |
| `rocketMadURL` | `""` | Base URL for RocketMAD links in DTS (e.g. `https://myRocketMAD.com/`). |
| `timezoneDbPath` | `""` | Base path for the timezone database file (without extension). Required for per-location timezone lookups used by scheduling. Leave blank to disable. |
| `timezoneDbType` | `"boltdb"` | Timezone database backend type. |
| `timezoneDbEncoding` | `"msgpack"` | Encoding format for timezone database entries. |
| `timezoneDbSnappy` | `true` | Whether to use Snappy compression for the timezone database. |

---

## database

Database connection settings for Dexter's own database and optional scanner database access.

| Option | Default | Description |
|--------|---------|-------------|
| `client` | `"mysql"` | Database driver. Must be `mysql` (covers both MySQL and MariaDB). SQLite is no longer supported. |
| `conn.host` | `"127.0.0.1"` | Dexter database hostname. |
| `conn.database` | `"poracle"` | Dexter database name. |
| `conn.user` | `"poracleuser"` | Dexter database username. |
| `conn.password` | `"poraclepassword"` | Dexter database password. |
| `conn.port` | `3306` | Dexter database port. |
| `scannerType` | `"none"` | Scanner database type. Supported: `none`, `golbat`, `rdm`, `mad`. |
| `scanner.host` | `"127.0.0.1"` | Scanner database hostname. |
| `scanner.database` | `"scannerdb"` | Scanner database name. |
| `scanner.user` | `"scanneruser"` | Scanner database username. |
| `scanner.password` | `"scannerpassword"` | Scanner database password. |
| `scanner.port` | `3306` | Scanner database port. |

!!! warning
    Change the default database credentials before running Dexter in production.

---

## discord

Discord bot configuration. See also the dedicated [Discord](discord.md) page for setup instructions.

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `true` | Whether the Discord bot is enabled. |
| `activity` | `"Dexter"` | Status activity text shown on the primary bot. |
| `workerStatus` | `"invisible"` | Online status for worker bots. Options: `available`, `dnd`, `idle`, `invisible`. |
| `workerActivity` | `"Dexter Helper"` | Status activity text shown on worker bots. |
| `disableAutoGreetings` | `false` | When `true`, suppress the automatic welcome DM when a user gains a qualifying role. |
| `uploadEmbedImages` | `false` | When `true`, download images and upload them directly to Discord's CDN instead of linking external URLs. |
| `disableCommandResponses` | `false` | When `true`, suppress bot replies to legacy `!` commands. Useful when another tool provides command responses. |
| `slash.disabled` | `false` | When `true`, do not register or handle Discord slash commands. Use this if another application shares the same bot token. |
| `slash.deregisterOnStart` | `false` | When `true`, delete all registered global slash commands on startup for a clean reinstall. |
| `slash.hideTemplateOptions` | `true` | When `true`, hide the "template" option from Discord slash command forms. |
| `checkRole` | `false` | Periodically verify that all registered Discord users still hold a qualifying role. Requires `general.roleCheckMode` to control the action taken. |
| `checkRoleInterval` | `6` | Hours between automated role checks. |
| `token` | `[""]` | Array of Discord bot tokens. The first token is used as the command controller; additional tokens are worker bots for sending messages. |
| `guilds` | `[""]` | Array of Discord guild (server) IDs that Dexter operates in. |
| `channels` | `[""]` | Array of channel IDs where users can register with Dexter. Not used in area-security mode. |
| `userRole` | `[""]` | Array of role IDs that automatically grant Dexter access. Not used in area-security mode. |
| `admins` | `[""]` | Array of Discord user IDs with administrator privileges (can run `!channel add` and other admin commands). |
| `delegatedAdministration` | `{}` | Grant specific users admin rights over specific channels or webhooks. See example below. |
| `commandSecurity` | `{}` | Restrict specific commands to certain user or role IDs. See example below. |
| `prefix` | `"!"` | Command prefix for legacy text commands. |
| `ivColors` | *(see below)* | Array of six hex colour codes for IV tiers, from worst to best. |
| `dmLogChannelID` | `""` | Channel ID to log all user commands to. Leave blank to disable. |
| `dmLogChannelDeletionTime` | `0` | Minutes after which logged command messages are deleted. `0` disables deletion. |
| `messageDeleteDelay` | `0` | Extra delay in milliseconds before cleaning up command-response messages. |
| `unrecognisedCommandMessage` | `""` | Custom reply for unrecognised commands sent in DM. |
| `unregisteredUserMessage` | `""` | Custom reply for unregistered users who DM the bot. |
| `lostRoleMessage` | `""` | Custom DM sent to a user when they lose their qualifying role. |

??? example "Delegated Administration"
    ```json
    "delegatedAdministration": {
      "channelTracking": {
        "channel-or-guild-or-category-id": ["admin-user-id"]
      },
      "webhookTracking": {
        "webhook-name": ["admin-user-id"]
      }
    }
    ```

??? example "Command Security"
    ```json
    "commandSecurity": {
      "monster": ["user-id", "role-id"],
      "pvp": ["role-id"]
    }
    ```
    Valid command names include: `monster`, `pvp`, `gym`, `invasion`, `lure`, `nest`, and others.

### IV Colour Tiers

The `ivColors` array maps six hex colours to IV percentage bands:

| Min IV | Max IV | Default Colour |
|--------|--------|----------------|
| 0% | 24.9% | `#9D9D9D` (grey) |
| 25% | 49.9% | `#FFFFFF` (white) |
| 50% | 81.9% | `#1EFF00` (green) |
| 82% | 89.9% | `#0070DD` (blue) |
| 90% | 99.9% | `#A335EE` (purple) |
| 100% | 100% | `#FF8000` (orange) |

---

## telegram

Telegram bot configuration. See also the dedicated [Telegram](telegram.md) page for setup instructions.

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `false` | Whether the Telegram bot is enabled. |
| `token` | `""` | Bot token from `@BotFather`. |
| `admins` | `[""]` | Array of Telegram user IDs with administrator privileges. |
| `delegatedAdministration` | `{}` | Grant specific users admin rights over specific groups or channels, same structure as Discord. |
| `channels` | `[""]` | Array of Telegram group IDs where users can register. Groups must be supergroups (IDs starting with `-100`). Not used in area-security mode. |
| `groupWelcomeText` | *(see default.json)* | Message posted in the group when a user registers. Supports `{user}` placeholder. |
| `botWelcomeText` | `"You are now registered with Dexter"` | DM sent by the bot when a user registers. |
| `botGoodbyeMessage` | `""` | DM sent to a user when they lose access through reconciliation. |
| `unregisteredUserMessage` | `""` | Custom reply for unregistered users who message the bot. |
| `unrecognisedCommandMessage` | `""` | Custom reply for unrecognised commands. |
| `checkRole` | `false` | Periodically verify that all registered Telegram users are still members of a registration group. |
| `checkRoleInterval` | `6` | Hours between automated membership checks. |
| `registerOnStart` | `false` | When `true`, automatically attempt registration when a user sends `/start`. |

---

## geocoding

Address lookup, reverse geocoding, and static map image settings. See also [Geocoding](geocoding.md) and [Static Maps](static-maps.md).

| Option | Default | Description |
|--------|---------|-------------|
| `provider` | `"none"` | Geocoding provider for address lookups. Options: `none`, `nominatim` (self-hosted), `pelias`, `google`. |
| `providerURL` | `""` | URL of the geocoding service (required for `nominatim` and `pelias`). |
| `providerKey` | `""` | API key for the geocoding provider (e.g. Pelias `api_key`). |
| `forwardOnly` | `false` | When `true`, disable reverse geocoding lookups. Only forward (address-to-coordinate) lookups are performed. |
| `peliasLayers` | `""` | **(Pelias only)** CSV list of layers to request (e.g. `venue,address,street`). |
| `peliasPreferredLayer` | `""` | **(Pelias only)** If set, Dexter prefers the first result whose `properties.layer` matches this value. |
| `peliasResultSize` | `0` | **(Pelias only)** Number of results to request from Pelias. Increasing helps ensure the preferred layer appears in the result set. |
| `peliasBoundaryCountry` | `""` | **(Pelias only)** Country code filter for Pelias results (e.g. `GB`, `US`). |
| `cacheDetail` | `3` | Decimal places of latitude/longitude used for geocoding cache keys. `3` gives ~110m precision; `4` gives ~11m. |
| `dayStyle` | `""` | Tileserver style name for daytime maps. Use `#(style)` in DTS templates. |
| `dawnStyle` | `""` | Tileserver style name for dawn maps. |
| `duskStyle` | `""` | Tileserver style name for dusk maps. |
| `nightStyle` | `""` | Tileserver style name for night maps. |
| `intersectionUsers` | `[]` | Array of GeoNames usernames for street intersection lookups. |
| `staticProvider` | `"none"` | Static map tile provider. Options: `none`, `tileservercache`, `google`, `osm`, `mapbox`. |
| `staticProviderURL` | `""` | URL of the tile server (required for `tileservercache`). |
| `tileserverTemplatePrefix` | `"dexter-"` | Prefix for tileservercache template names (e.g. `dexter-monster`, `dexter-raid`). Set to `"poracle-"` for backwards compatibility with existing tileservercache setups. |
| `tileserverTemplates` | `{}` | Per-type overrides for tileservercache template names. When set, the value is used as the full template name (no prefix applied). E.g. `{ "monster": "my-custom-monster" }`. |
| `geocodingKey` | `[""]` | Array of Google API keys for geocoding. Dexter rotates through them. |
| `staticKey` | `[""]` | Array of API keys for the static map provider (`google`, `osm`, `mapbox`). |
| `width` | `320` | Static map image width in pixels (non-tileservercache providers). |
| `height` | `200` | Static map image height in pixels (non-tileservercache providers). |
| `zoom` | `15` | Static map zoom level (non-tileservercache providers). |
| `spriteHeight` | `20` | Pokemon icon height on the static map in pixels. |
| `spriteWidth` | `20` | Pokemon icon width on the static map in pixels. |
| `scale` | `2` | Static map scale factor. |
| `type` | `"klokantech-basic"` | Map style type (non-tileservercache providers). |

### tileserverSettings

The `tileserverSettings` object within `geocoding` controls per-type settings for tileservercache:

| Option | Default | Description |
|--------|---------|-------------|
| `default.type` | `"staticMap"` | Tile request type. Can be `staticMap` or `multiStaticMap`. |
| `default.width` | `500` | Image width in pixels. |
| `default.height` | `250` | Image height in pixels. |
| `default.zoom` | `15` | Map zoom level. |
| `default.pregenerate` | `true` | Whether to pregenerate the tile. |
| `default.includeStops` | `false` | Whether to include Pokestops on the map. |

You can add per-type overrides using keys like `monster`, `raid`, `pokestop`, `quest`, `weather`, `location`, `nest`, `gym`:

```json
"tileserverSettings": {
  "default": {
    "type": "staticMap",
    "width": 500,
    "height": 250,
    "zoom": 15,
    "pregenerate": true,
    "includeStops": false
  },
  "monster": {
    "type": "staticMap",
    "includeStops": true
  }
}
```

---

## tracking

Restrictions and defaults for user tracking commands.

| Option | Default | Description |
|--------|---------|-------------|
| `everythingFlagPermissions` | `"allow-any"` | Controls the `everything` keyword on `!track`. `"allow-any"` -- unrestricted, stored as a wildcard. `"allow-and-always-individually"` -- allowed but stored as individual rows per Pokemon. `"allow-and-ignore-individually"` -- allowed, but users cannot opt for individual rows. `"deny"` -- users must track Pokemon individually. |
| `defaultDistance` | `0` | Default tracking radius in metres when no distance is specified. Useful for distance-only setups with no area fences. |
| `maxDistance` | `0` | Maximum tracking radius users can set, in metres. `0` means no limit. |
| `enableGymBattle` | `false` | Allow users to use the `battle_changes` option in `!gym` tracking. |
| `defaultUserTrackingLevelCap` | `0` | Default PvP level cap for new users. `0` means all levels are considered. |

---

## pvp

PvP rank calculation and display settings. See also [PvP Tracking](pvp.md).

| Option | Default | Description |
|--------|---------|-------------|
| `dataSource` | `"internal"` | Source of PvP rank data. `"internal"` uses Dexter's built-in calculator; `"webhook"` uses ranks provided by the scanner webhook. |
| `levelCaps` | `[50]` | Level caps included in internal rank calculations. E.g. `[50, 51]` to include best-buddy levels. |
| `includeMegaEvolution` | `false` | Whether mega evolutions are included in internal rank calculations. |
| `littleLeagueCanEvolve` | `false` | Whether Pokemon that can still evolve are included in Little League calculations. |
| `pvpEvolutionDirectTracking` | `false` | Allow users to track PvP evolutions directly (e.g. tracking Vaporeon would match an Eevee that ranks well as Vaporeon). |
| `filterByTrack` | `false` | When `true`, PvP listings are automatically filtered by the user's tracking requirements. |
| `pvpDisplayMaxRank` | `10` | Maximum PvP rank shown in alert messages. Passed into DTS as a variable. |
| `pvpDisplayGreatMinCP` | `1400` | Minimum CP shown for Great League PvP results in alerts. |
| `pvpDisplayUltraMinCP` | `2350` | Minimum CP shown for Ultra League PvP results in alerts. |
| `pvpDisplayLittleMinCP` | `450` | Minimum CP shown for Little League PvP results in alerts. |
| `pvpFilterMaxRank` | `10` | Maximum rank users can set on `!track` PvP filters. Prevents excessively broad tracking. |
| `pvpFilterGreatMinCP` | `1400` | Minimum CP floor enforced on user Great League tracking requests. |
| `pvpFilterUltraMinCP` | `2350` | Minimum CP floor enforced on user Ultra League tracking requests. |
| `pvpFilterLittleMinCP` | `450` | Minimum CP floor enforced on user Little League tracking requests. |

---

## stats

Pokemon rarity statistics calculation.

| Option | Default | Description |
|--------|---------|-------------|
| `maxPokemonId` | `1010` | Highest Pokemon ID to consider in stats calculations. |
| `minSampleSize` | `10000` | Minimum number of Pokemon observations required before broadcasting rarity stats. |
| `pokemonCountToKeep` | `8` | Rolling window in hours used for rarity calculations. |
| `rarityRefreshInterval` | `5` | Minutes between rarity stat recalculations. |
| `rarityGroup2Uncommon` | `1` | Percentage threshold (of total seen) below which a Pokemon is classified as Uncommon. |
| `rarityGroup3Rare` | `0.5` | Percentage threshold for the Rare classification. |
| `rarityGroup4VeryRare` | `0.03` | Percentage threshold for the Very Rare classification. |
| `rarityGroup5UltraRare` | `0.01` | Percentage threshold for the Ultra Rare classification. |
| `excludeFromRare` | *(large array)* | Array of Pokemon IDs excluded from Rare / Very Rare / Ultra Rare classifications. Typically common species that should never appear as rare even during low-spawn periods. |

---

## tuning

Internal performance parameters. Change these carefully -- discuss in the Dexter Discord before adjusting.

!!! warning
    Increasing these values beyond recommended ranges can cause rate-limiting, excessive memory usage, or database connection exhaustion. Adjust incrementally.

| Option | Default | Description |
|--------|---------|-------------|
| `fastMonsters` | `true` | Use in-memory Pokemon matching to eliminate SQL lookups. Faster but uses more RAM. |
| `disablePokemonCache` | `true` | When `true`, disable the in-memory Pokemon cache, significantly reducing memory usage. |
| `maxDatabaseConnections` | `15` | Maximum database connections per worker thread. |
| `webhookProcessingWorkers` | `5` | Number of threads for inbound webhook processing. Five is sufficient for very large systems; going higher is likely counterproductive. |
| `concurrentWebhookProcessorsPerWorker` | `4` | Concurrent webhooks processed per worker. Each requires a database connection, so keep this below `maxDatabaseConnections`. |
| `concurrentDiscordDestinationsPerBot` | `10` | Concurrent outbound Discord messages per bot. Discord rate-limits by route and globally. |
| `concurrentTelegramDestinationsPerBot` | `10` | Concurrent outbound Telegram messages per bot. |
| `concurrentDiscordWebhookConnections` | `10` | Concurrent outbound Discord webhook connections. |

---

## logger

Logging configuration.

| Option | Default | Description |
|--------|---------|-------------|
| `consoleLogLevel` | `"verbose"` | Log level for console output (and pm2/systemd journals). Options: `silly`, `debug`, `verbose`, `info`, `warn`. |
| `logLevel` | `"verbose"` | Log level for on-disk log files. |
| `enableLogs.webhooks` | `false` | Enable hourly webhook log files. These can grow very large. |
| `enableLogs.discord` | `true` | Enable logging of outbound Discord messages. |
| `enableLogs.telegram` | `true` | Enable logging of outbound Telegram messages. |
| `enableLogs.pvp` | `false` | Enable detailed PvP calculation logging at the `verbose` level. |
| `timingStats` | `false` | Promote key timing statistics from `debug` to `verbose` level for easier observation. |
| `dailyLogLimit` | `7` | Number of days to retain daily log files (everything except webhook logs). |
| `webhookLogLimit` | `12` | Number of hours to retain webhook log files, if enabled. |

!!! tip
    Start with `verbose` and drop to `info` once everything is stable. Switch to `debug` when troubleshooting specific issues.

---

## locale

Date, time, and address formatting.

| Option | Default | Description |
|--------|---------|-------------|
| `timeformat` | `"en-gb"` | Locale string used for time display (e.g. `en-gb`, `en-us`, `de`). |
| `time` | `"LTS"` | Time format token. See Moment.js locale-aware formats. |
| `date` | `"L"` | Date format token. See Moment.js locale-aware formats. |
| `addressFormat` | `"{{{streetName}}} {{streetNumber}}"` | Handlebars template for the `{{addr}}` DTS placeholder. Available fields depend on your geocoding provider. |
| `language` | `"en"` | Secondary language used for `{{translateAlt}}` in DTS. Useful for retrieving English-language names for web links even when the primary locale is another language. |

---

## geofence

Geofence file and Koji integration.

| Option | Default | Description |
|--------|---------|-------------|
| `path` | `"./config/geofence.json"` | Path to the geofence file. Supports the format created by [geo.jasparke.net](http://geo.jasparke.net/) or standard GeoJSON. |
| `defaultGeofenceName` | `"city"` | Fallback fence name used when a GeoJSON feature has no `name` property. |
| `defaultGeofenceColor` | `"#3399ff"` | Default colour for geofence display. |
| `kojiOptions.bearerToken` | `""` | Bearer token for Koji geofence integration. Leave blank to disable. |

---

## weather

Weather change alerts and AccuWeather forecast integration. See also [Weather Forecast](weather-forecast.md).

| Option | Default | Description |
|--------|---------|-------------|
| `weatherChangeAlert` | `false` | Enable alerts when a weather change affects a tracked Pokemon's weather boost. |
| `showAlteredPokemon` | `false` | Include weather-affected Pokemon details in weather-change DTS. |
| `showAlteredPokemonStaticMap` | `false` | Show weather-affected Pokemon on the static map image. |
| `showAlteredPokemonMaxCount` | `10` | Maximum number of affected Pokemon listed per weather-change alert. |
| `enableWeatherForecast` | `false` | Enable AccuWeather-based weather forecasting. |
| `apiKeyAccuWeather` | `[""]` | Array of AccuWeather API keys. Dexter rotates through them to stay within quotas. |
| `apiKeyDayQuota` | `50` | Maximum API calls allowed per key per day. |
| `smartForecast` | `false` | Pull forecast data on-demand when no data exists for a cell, instead of waiting for the next refresh cycle. |
| `localFirstFetchHOD` | `3` | Hour of the day (local time) for the first scheduled forecast fetch. `3` (3 AM) is recommended. |
| `forecastRefreshInterval` | `8` | Hours between scheduled forecast API calls. |

---

## alertLimits

Rate-limiting to prevent individual users or channels from overwhelming the system.

| Option | Default | Description |
|--------|---------|-------------|
| `timingPeriod` | `240` | Rolling window in seconds over which message limits are calculated. |
| `dmLimit` | `20` | Maximum messages a single user can receive within `timingPeriod`. |
| `channelLimit` | `40` | Maximum messages a channel or group can receive within `timingPeriod`. |
| `maxLimitsBeforeStop` | `10` | Number of times a user can hit the rate limit within 24 hours before being stopped. |
| `disableOnStop` | `false` | When `true`, admin-disable the user when stopped (requiring admin intervention to re-enable) rather than a soft stop. |
| `shameChannel` | `""` | Discord channel ID where stopped/disabled users are announced. |
| `limitOverride` | `{}` | Per-target limit overrides. Keys are channel IDs, user IDs, or webhook names; values are the custom limit number. |

??? example "Limit Override"
    ```json
    "limitOverride": {
      "123456789012345678": 100,
      "my-webhook-name": 200
    }
    ```

---

## areaSecurity

Community-based access controls that restrict which areas users can track based on their roles or group membership. See also [Area Security](area-security.md).

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `false` | Enable area security. When active, this overrides `discord.channels`, `discord.userRole`, and `telegram.channels`. |
| `strictLocations` | `false` | When `true`, verify that every alert originates from within the community's `locationFence`, not just the user's tracked areas. |
| `communities` | `{}` | Map of community definitions. See example below. |

??? example "Community Configuration"
    ```json
    "communities": {
      "newyork": {
        "allowedAreas": ["manhattan", "bronx", "brooklyn", "queens"],
        "locationFence": "wholenewyork",
        "discord": {
          "channels": ["channel-id"],
          "userRole": ["role-id"]
        },
        "telegram": {
          "channels": ["group-id"]
        }
      },
      "chicago": {
        "allowedAreas": ["northwest", "southside", "central"],
        "locationFence": "wholechicago",
        "discord": {
          "channels": ["channel-id"],
          "userRole": ["role-id"]
        },
        "telegram": {
          "channels": ["group-id"]
        }
      }
    }
    ```

Each community defines:

- **`allowedAreas`** -- array of geofence names the community's users may add with `!area add`.
- **`locationFence`** -- the single geofence used for `strictLocations` checks.
- **`discord`** -- `channels` (registration channel IDs) and `userRole` (qualifying role IDs).
- **`telegram`** -- `channels` (registration group IDs).

---

## reconciliation

Automated user and channel reconciliation settings. The periodic check only runs if `checkRole` is enabled in the `discord` or `telegram` section. These settings also apply when role changes are observed in real time on Discord.

### reconciliation.discord

| Option | Default | Description |
|--------|---------|-------------|
| `updateUserNames` | `false` | Follow Discord username changes and update the database. |
| `removeInvalidUsers` | `true` | De-register users who lose their qualifying role during a role check. |
| `registerNewUsers` | `false` | Automatically register users who gain a qualifying role during a role check. |
| `updateChannelNames` | `true` | Keep channel names in the database synchronised with Discord. |
| `updateChannelNotes` | `false` | Update channel notes in the database with guild name and category. |
| `unregisterMissingChannels` | `false` | Remove channels from the database when they are deleted from Discord. |

### reconciliation.telegram

| Option | Default | Description |
|--------|---------|-------------|
| `updateUserNames` | `false` | Follow Telegram username changes and update the database. |
| `removeInvalidUsers` | `true` | Remove users who are no longer members of a registration group. |

---

## fallbacks

Fallback image URLs used when the primary image source fails.

| Option | Default | Description |
|--------|---------|-------------|
| `staticMap` | *(GitHub URL)* | Fallback image when static map generation fails. |
| `imgUrl` | *(GitHub URL)* | Fallback Pokemon icon. |
| `imgUrlWeather` | *(GitHub URL)* | Fallback weather icon. |
| `imgUrlEgg` | *(GitHub URL)* | Fallback egg icon. |
| `imgUrlGym` | *(GitHub URL)* | Fallback gym icon. |
| `imgUrlPokestop` | *(GitHub URL)* | Fallback Pokestop icon. |
| `imgUrlStation` | *(GitHub URL)* | Fallback station icon. |
| `pokestopUrl` | *(GitHub URL)* | Fallback Pokestop URL used in quest alerts. |

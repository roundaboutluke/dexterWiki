# Configuration

Dexter's configuration lives in `config/local.json`. The built-in defaults are in `config/default.json` — you should never modify that file directly. Dexter merges `local.json` on top of `default.json`, so you only need to include settings you want to change.

## Configuration Sections

| Section | Description |
|---------|-------------|
| [Config Reference](config-reference.md) | Complete reference of all settings |
| [Discord](discord.md) | Discord-specific configuration |
| [Telegram](telegram.md) | Telegram-specific configuration |
| [Alert Templates](alert-templates.md) | Customise alert message formatting |
| [Advanced DTS](alert-templates-advanced.md) | Handlebars helpers and advanced templating |
| [Static Maps](static-maps.md) | Map image providers and templates |
| [Geocoding](geocoding.md) | Address lookup and reverse geocoding |
| [Languages](languages.md) | Multi-language support |
| [Scheduling & Profiles](scheduling.md) | Active/quiet hours and profile switching |
| [PvP Tracking](pvp.md) | PvP rank calculations and leagues |
| [Area Security](area-security.md) | Community-based access controls |
| [Weather Forecast](weather-forecast.md) | AccuWeather integration |
| [Shortlinks](shortlinks.md) | URL shortening providers |

# Weather Forecast

Dexter can fetch weather forecasts from AccuWeather and alert users when weather changes will affect their tracked Pokemon.

## Weather Change Alerts

Basic weather change alerts (no API needed) notify users when in-game weather changes affect Pokemon they're tracking:

```json
{
  "general": {
    "weatherChangeAlert": true,
    "showAlteredPokemon": true,
    "showAlteredPokemonStaticMap": false,
    "showAlteredPokemonMaxCount": 10
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `weatherChangeAlert` | `false` | Send alerts when weather changes affect tracked Pokemon |
| `showAlteredPokemon` | `false` | Include list of affected Pokemon in the alert |
| `showAlteredPokemonStaticMap` | `false` | Show a multi-static map with affected Pokemon |
| `showAlteredPokemonMaxCount` | `10` | Maximum number of affected Pokemon to show |

## AccuWeather Forecast

For predictive weather alerts, configure AccuWeather API access:

```json
{
  "weather": {
    "enableWeatherForecast": true,
    "apiKeyAccuWeather": ["YOUR_API_KEY"],
    "apiKeyDayQuota": 50,
    "smartForecast": false,
    "localFirstFetchHOD": 3,
    "forecastRefreshInterval": 8
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `enableWeatherForecast` | `false` | Enable weather forecasting |
| `apiKeyAccuWeather` | `[]` | Array of AccuWeather API keys (rotated to stay within quotas) |
| `apiKeyDayQuota` | `50` | Daily API call quota per key |
| `smartForecast` | `false` | Use smart forecasting to reduce API calls |
| `localFirstFetchHOD` | `3` | Hour of day (0–23) for the first daily API fetch |
| `forecastRefreshInterval` | `8` | Hours between forecast refreshes |

### Multiple API Keys

AccuWeather's free tier has a daily call limit. To work around this, you can provide multiple API keys that are rotated automatically:

```json
{
  "weather": {
    "apiKeyAccuWeather": ["key1", "key2", "key3"],
    "apiKeyDayQuota": 50
  }
}
```

### Smart Forecast

When `smartForecast` is enabled, Dexter reduces API calls by only fetching forecasts for areas where users are actively tracking weather-dependent Pokemon.

## User Commands

Users track weather with:

```
/weather condition:Clear
/weather condition:Rainy location:Downtown
```

See the [/weather command reference](../commands/weather.md) for details.

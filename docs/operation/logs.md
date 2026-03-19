# Logs

Dexter uses structured logging with configurable verbosity levels.

## Log Levels

| Level | Description |
|-------|-------------|
| `silly` | Everything, including internal debug details |
| `debug` | Detailed debugging information |
| `verbose` | General operational information (default) |
| `info` | Important events only |
| `warn` | Warnings and errors only |

## Configuration

```json
{
  "logging": {
    "consoleLogLevel": "verbose",
    "logLevel": "verbose",
    "enableLogs": {
      "webhooks": true,
      "discord": true,
      "telegram": true,
      "pvp": true
    },
    "timingStats": false,
    "dailyLogLimit": 7,
    "webhookLogLimit": 12
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `consoleLogLevel` | `"verbose"` | Log level for console output |
| `logLevel` | `"verbose"` | Log level for file output |
| `enableLogs.webhooks` | `true` | Log incoming webhook events |
| `enableLogs.discord` | `true` | Log Discord API interactions |
| `enableLogs.telegram` | `true` | Log Telegram API interactions |
| `enableLogs.pvp` | `true` | Log PvP calculations |
| `timingStats` | `false` | Log timing statistics for performance analysis |
| `dailyLogLimit` | `7` | Days to retain daily log files |
| `webhookLogLimit` | `12` | Hours to retain webhook log files |

## Log Categories

Dexter separates logs into categories:

- **Main log** — application lifecycle, configuration, errors
- **Webhook log** — incoming webhook events and processing
- **Discord log** — Discord API calls, message delivery, rate limits
- **Telegram log** — Telegram API calls, message delivery
- **PvP log** — PvP rank calculations

## Troubleshooting

### No alerts being sent

1. Check that webhooks are arriving: look for webhook processing messages in the log
2. Verify your bot token and permissions
3. Check that users are registered and have active tracking filters
4. Verify your geofence areas cover the scanned region

### High latency alerts

1. Enable `timingStats` to identify bottlenecks
2. Check database connection performance
3. Consider increasing `webhookProcessingWorkers`
4. Add worker bot tokens for Discord delivery

### Rate limit warnings

See [Rate Limits](rate-limits.md) for configuration and tuning.

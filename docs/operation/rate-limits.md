# Rate Limits

Dexter manages message delivery to avoid hitting Discord and Telegram API rate limits, and to prevent individual users from overwhelming the system with overly broad tracking.

## How It Works

Dexter tracks messages sent per user/channel within a rolling time window. When a user exceeds their limit, messages are queued and delivered more slowly. If a user consistently exceeds limits, their tracking can be automatically disabled.

## Configuration

```json
{
  "alertLimits": {
    "timingPeriod": 240,
    "dmLimit": 20,
    "channelLimit": 40,
    "maxLimitsBeforeStop": 10,
    "disableOnStop": false,
    "shameChannel": "",
    "limitOverride": {}
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `timingPeriod` | `240` | Rolling window in seconds for rate calculation |
| `dmLimit` | `20` | Maximum DMs to a user per period |
| `channelLimit` | `40` | Maximum messages to a channel per period |
| `maxLimitsBeforeStop` | `10` | Number of times a user can hit the limit before being stopped |
| `disableOnStop` | `false` | Permanently disable the user's tracking (requires admin to re-enable) |
| `shameChannel` | `""` | Channel ID to post notifications when users are rate-limited |

## Per-Route Overrides

Override limits for specific users or channels:

```json
{
  "alertLimits": {
    "limitOverride": {
      "CHANNEL_ID": {
        "dmLimit": 50,
        "channelLimit": 100
      }
    }
  }
}
```

## Platform Limits

### Discord

- Discord has a global rate limit of approximately 5 messages per 5 seconds per channel
- Dexter automatically backs off when hitting Discord's rate limits
- Worker bots help distribute load across multiple connections

### Telegram

- Telegram limits bots to approximately 30 messages per second overall
- Per-chat limit of approximately 1 message per second
- Dexter respects these limits with automatic retry and backoff

## Tuning

For high-volume setups, adjust the concurrency settings:

```json
{
  "tuning": {
    "concurrentDiscordDestinationsPerBot": 10,
    "concurrentTelegramDestinationsPerBot": 10,
    "concurrentDiscordWebhookConnections": 10
  }
}
```

### Everything Flag

Control how broadly users can track:

```json
{
  "tracking": {
    "everythingFlagPermissions": "allow-any"
  }
}
```

| Value | Description |
|-------|-------------|
| `"allow-any"` | Users can use `everything` in tracking commands |
| `"allow-and-always-individually"` | `everything` works but always tracks individually too |
| `"allow-and-ignore-individually"` | `everything` works but ignores individual tracking |
| `"deny"` | Disable the `everything` flag |

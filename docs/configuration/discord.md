# Discord Configuration

## Basic Setup

```json
{
  "discord": {
    "enabled": true,
    "token": ["YOUR_BOT_TOKEN"],
    "guilds": ["SERVER_ID"],
    "channels": ["REGISTRATION_CHANNEL_ID"],
    "admins": ["YOUR_USER_ID"],
    "prefix": "!"
  }
}
```

## Settings Reference

### Core

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `true` | Enable Discord support |
| `token` | `[""]` | Array of bot tokens. Use multiple for worker bots to increase throughput |
| `guilds` | `[""]` | Array of Discord server (guild) IDs |
| `channels` | `[""]` | Array of channel IDs where users can register |
| `admins` | `[""]` | Array of user IDs with admin access |
| `prefix` | `"!"` | Legacy command prefix (slash commands always work) |

### Bot Status

| Option | Default | Description |
|--------|---------|-------------|
| `activity` | `"Dexter"` | Bot's activity/status text |
| `workerStatus` | `"invisible"` | Worker bot status: `available`, `dnd`, `idle`, `invisible` |
| `workerActivity` | `"Dexter Helper"` | Worker bot activity text |

### Slash Commands

| Option | Default | Description |
|--------|---------|-------------|
| `slash.disabled` | `false` | Disable slash commands entirely |
| `slash.deregisterOnStart` | `false` | Remove and re-register all slash commands on startup |
| `slash.hideTemplateOptions` | `true` | Hide the template option from slash commands |

!!! tip "Re-registering commands"
    If slash commands seem stale after an update, set `deregisterOnStart: true` once, restart, then set it back to `false`.

### Role Checking

| Option | Default | Description |
|--------|---------|-------------|
| `checkRole` | `false` | Enable role-based access checking |
| `checkRoleInterval` | `6` | Hours between role checks |
| `userRole` | `[""]` | Array of role IDs that grant automatic access |

### Auto-Registration

| Option | Default | Description |
|--------|---------|-------------|
| `disableAutoGreetings` | `false` | Don't send welcome DMs to newly registered users |

### Channel Posting

Dexter can post to channels using bot messages or webhooks. Webhook-based posting allows custom names and avatars per alert.

| Option | Default | Description |
|--------|---------|-------------|
| `uploadEmbedImages` | `false` | Upload map images as attachments instead of linking URLs |

### Message Management

| Option | Default | Description |
|--------|---------|-------------|
| `disableCommandResponses` | `false` | Don't send confirmation replies to commands |
| `messageDeleteDelay` | `0` | Extra milliseconds to delay message cleanup |

### Command Security

Restrict commands to specific channels or roles:

```json
{
  "discord": {
    "commandSecurity": {
      "pokemon": {
        "allowedChannels": ["CHANNEL_ID"],
        "allowedRoles": ["ROLE_ID"]
      }
    }
  }
}
```

### Delegated Administration

Grant admin powers for specific channels/webhooks to non-admin users:

```json
{
  "discord": {
    "delegatedAdministration": {
      "CHANNEL_ID": ["USER_ID_1", "USER_ID_2"]
    }
  }
}
```

### Logging

| Option | Default | Description |
|--------|---------|-------------|
| `dmLogChannelID` | `""` | Channel ID to log all user commands |
| `dmLogChannelDeletionTime` | `0` | Minutes before log messages are cleaned up (0 = never) |

### Custom Messages

| Option | Default | Description |
|--------|---------|-------------|
| `unrecognisedCommandMessage` | `""` | Reply when a user sends an unknown command |
| `unregisteredUserMessage` | `""` | Reply when an unregistered user tries a command |
| `lostRoleMessage` | `""` | Message sent when a user loses their required role |

### IV Colours

Custom colours for IV-based embed formatting:

```json
{
  "discord": {
    "ivColors": ["#ff0000", "#ff4400", "#ff8800", "#ffcc00", "#ffff00", "#00ff00"]
  }
}
```

### Worker Bots

For high-volume setups, use multiple bot tokens to distribute message sending:

```json
{
  "discord": {
    "token": ["MAIN_BOT_TOKEN", "WORKER_TOKEN_1", "WORKER_TOKEN_2"]
  }
}
```

The first token is the "main" bot that handles commands. Additional tokens are worker bots that help with alert delivery.

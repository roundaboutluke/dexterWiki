# Telegram Configuration

## Basic Setup

```json
{
  "telegram": {
    "enabled": true,
    "token": "YOUR_BOT_TOKEN",
    "admins": ["YOUR_USER_ID"],
    "channels": ["GROUP_ID"]
  }
}
```

## Settings Reference

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `false` | Enable Telegram support |
| `token` | `""` | Bot token from @BotFather |
| `admins` | `[""]` | Array of admin user IDs |
| `channels` | `[""]` | Array of group IDs where users can register |
| `checkRole` | `false` | Enable group-membership-based access checking |
| `checkRoleInterval` | `6` | Hours between role checks |
| `registerOnStart` | `false` | Auto-register users who message the bot |

## Custom Messages

| Option | Default | Description |
|--------|---------|-------------|
| `groupWelcomeText` | `"Welcome {user}..."` | Message shown when a new user joins a registration group |
| `botWelcomeText` | `"You are now registered..."` | DM sent when a user registers |
| `botGoodbyeMessage` | `""` | Message sent when a user unregisters |
| `unregisteredUserMessage` | `""` | Reply when an unregistered user tries a command |
| `unrecognisedCommandMessage` | `""` | Reply when a user sends an unknown command |

## Delegated Administration

Grant admin powers for specific groups to non-admin users:

```json
{
  "telegram": {
    "delegatedAdministration": {
      "GROUP_ID": ["USER_ID_1", "USER_ID_2"]
    }
  }
}
```

## Stickers

Telegram supports animated sticker alerts using webp images:

```json
{
  "general": {
    "stickerUrl": "https://raw.githubusercontent.com/bbdoc/tgUICONS/main/Shuffle"
  }
}
```

## Group vs Channel Tracking

- **Groups**: Admins can register groups with `/channel add`. Tracking commands in the group send results there.
- **Channels**: Telegram channels (broadcast-only) can receive alerts via webhook-style posting.

## Finding IDs

Send `/identify` to the bot:

- In a **DM**: returns your user ID
- In a **group**: returns the group ID (negative number)

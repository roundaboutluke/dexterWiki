# Area Security

Area security restricts which users can track in which geofence areas, based on Discord roles or Telegram group membership.

## Enabling Area Security

```json
{
  "areaSecurity": {
    "enabled": true,
    "strictLocations": false
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `enabled` | `false` | Enable area-based access controls |
| `strictLocations` | `false` | Also restrict distance-based tracking to allowed areas |

## Defining Communities

Communities map areas to access requirements:

```json
{
  "areaSecurity": {
    "enabled": true,
    "communities": {
      "downtown": {
        "discord": {
          "allowedRoles": ["ROLE_ID_1"],
          "allowedChannels": ["CHANNEL_ID_1"]
        },
        "telegram": {
          "allowedGroups": ["GROUP_ID_1"]
        },
        "areas": ["Downtown", "City Centre"]
      },
      "suburbs": {
        "discord": {
          "allowedRoles": ["ROLE_ID_2"]
        },
        "areas": ["North Suburbs", "South Suburbs", "East Suburbs"]
      }
    }
  }
}
```

### Community Fields

| Field | Description |
|-------|-------------|
| `areas` | Array of geofence area names this community controls |
| `discord.allowedRoles` | Discord role IDs that grant access |
| `discord.allowedChannels` | Discord channel IDs where this community's commands work |
| `telegram.allowedGroups` | Telegram group IDs that grant access |

## How It Works

1. When a user tries to add an area to their profile, Dexter checks if the area belongs to a community
2. If it does, Dexter checks if the user has the required role (Discord) or is a member of the required group (Telegram)
3. If the user doesn't have access, the command is rejected with a message

### Strict Locations

When `strictLocations` is `true`, distance-based tracking is also restricted. A user's location must fall within an area they have access to, or distance-based tracking is denied.

## Example

A setup with two communities — "City" requires a Discord role, "Rural" is open to everyone in a specific channel:

```json
{
  "areaSecurity": {
    "enabled": true,
    "communities": {
      "city": {
        "discord": {
          "allowedRoles": ["123456789"]
        },
        "areas": ["City Centre", "City North", "City South"]
      },
      "rural": {
        "discord": {
          "allowedChannels": ["987654321"]
        },
        "areas": ["Village A", "Village B", "Countryside"]
      }
    }
  }
}
```

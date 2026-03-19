# Admin Commands

Admin commands are text-based commands available only to users listed in the `discord.admins` or `telegram.admins` configuration. These commands use the `!` prefix and are not available as slash commands.

## Command Reference

| Command | Description |
|---------|-------------|
| `!channel add` | Register a Discord channel for bot-managed alerts |
| `!channel remove` | Unregister a channel |
| `!webhook add <url>` | Register a Discord webhook target for alerts |
| `!webhook remove <url>` | Unregister a webhook target |
| `!area add <name>` | Add a geofenced area for tracking |
| `!area remove <name>` | Remove a geofenced area |
| `!community` | Manage community settings and area security |
| `!userlist` | List registered users |
| `!enable` | Enable a disabled user |
| `!disable` | Disable a user |
| `!unregister` | Remove a user's registration and all their data |
| `!backup` | Export a user's filters |
| `!restore` | Import filters from a backup |
| `!apply` | Apply a predefined filter set to a user or channel |
| `!broadcast` | Send a message to all registered users |
| `!version` | Show the running Dexter version |
| `!script` | Run a batch of commands from a script |

## Channel Management

Channels allow the bot to post alerts into a shared Discord channel rather than via DM. An admin registers a channel, then sets up tracking filters for that channel.

```
!channel add
```

Run this command inside the Discord channel you want to register. Once added, you can use tracking commands (e.g. `!track`, `!raid`) in that channel to configure what alerts are posted there.

```
!channel remove
```

Removes the channel registration and all associated filters.

## Webhook Management

Webhooks allow Dexter to post alerts to a Discord webhook URL, which is useful for channels where the bot does not need full membership.

```
!webhook add https://discord.com/api/webhooks/...
```

## Area Management

Areas define geographic regions for tracking. They correspond to geofence files configured on the server.

```
!area add MyTown
!area remove MyTown
```

When area security is enabled (`areaSecurity.enabled`), admins control which areas each community can access.

## User Management

```
!userlist
```

Lists all registered users with their status. Admin users can see additional details.

```
!enable @user
!disable @user
```

Enable or disable a specific user's alerts without removing their filters.

```
!unregister @user
```

Permanently remove a user's registration and all their tracking data.

## Backup and Restore

```
!backup @user
!restore @user
```

Export or import a user's complete filter configuration. Useful for migrating users or recovering from issues.

## Broadcast

```
!broadcast Your message here
```

Send a message to all registered users. Use sparingly.

## Apply

```
!apply <preset> @user
```

Apply a predefined set of tracking filters to a user or channel. Presets are configured on the server.

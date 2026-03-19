# Creating a Discord Bot

## Create the Application

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications) and click **New Application**
2. Give your bot a name (e.g. "Dexter") and a profile picture
3. Click **Create**

## Create the Bot User

1. In your application, go to the **Bot** section
2. Click **Add Bot** → **Yes, do it!**
3. Click **Reset Token** to reveal your bot token — copy and save it securely

!!! danger "Keep your token secret"
    Never share your bot token or commit it to source control. Anyone with the token can control your bot.

## Enable Required Intents

In the **Bot** section, enable these **Privileged Gateway Intents**:

- [x] **Server Members Intent** — required for role checking
- [x] **Presence Intent** — optional but recommended
- [x] **Message Content Intent** — required for legacy `!` prefix commands

## Invite the Bot to Your Server

1. Go to the **OAuth2** → **URL Generator** section
2. Under **Scopes**, select `bot` and `applications.commands`
3. Under **Bot Permissions**, select:
    - Send Messages
    - Manage Messages (for auto-delete)
    - Embed Links
    - Attach Files
    - Read Message History
    - Use External Emojis
    - Manage Webhooks (if using webhook-based channel posting)
4. Copy the generated URL and open it in your browser
5. Select your server and click **Authorize**

## Get Your IDs

Enable **Developer Mode** in Discord (Settings → Advanced → Developer Mode), then right-click to copy IDs:

- **Guild ID** — right-click your server name
- **Channel ID** — right-click the registration channel
- **Your User ID** — right-click your username

## Configure Dexter

Add to your `config/local.json`:

```json
{
  "discord": {
    "enabled": true,
    "token": ["YOUR_BOT_TOKEN"],
    "guilds": ["YOUR_GUILD_ID"],
    "channels": ["REGISTRATION_CHANNEL_ID"],
    "admins": ["YOUR_USER_ID"]
  }
}
```

## Test It

1. Start Dexter
2. Go to the registration channel and type `/start`
3. You should receive a welcome DM from the bot

## Quick Tracking Test

Use slash commands to test basic tracking:

- `/profile location set` — set your location
- `/pokemon pokemon:Pikachu` — track Pikachu spawns
- `/filters show` — verify your filters

When done testing:

- `/filters remove type:pokemon` — remove test filters

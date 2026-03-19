# Creating a Telegram Bot

## Talk to @BotFather

Telegram has a bot for creating bots. Find and message [@BotFather](https://t.me/botfather):

```
You: /newbot

BotFather: Alright, a new bot. How are we going to call it?
           Please choose a name for your bot.

You: DexterBot

BotFather: Done! Congratulations on your new bot.
           Use this token to access the HTTP API:
           462xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Save the token — you'll need it for `local.json`.

## Find Your User ID

There are several ways to find your Telegram user ID:

1. Start Dexter without admins configured, then send `/identify` to the bot — it will reply with your ID
2. Register in the channel with `/poracle` and check the `humans` table in the database
3. Interact with the bot and check `https://api.telegram.org/bot<YOUR_TOKEN>/getUpdates`

## Create a Registration Group

1. Create a new Telegram group
2. Invite your Dexter bot to the group
3. Send `/identify` in the group to get the group's ID (a negative number)

## Configure Dexter

Add to your `config/local.json`:

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

## Test It

1. Start Dexter
2. In the registration group, send `/poracle` to register
3. Open a DM with the bot and click **Start** (or visit `https://t.me/YourBotName?start`)
4. You should receive a welcome message

## Group-Based Tracking

Admins can register any group that the bot is a member of using the `/channel add` command. After that, tracking commands in the group will send results to that group.

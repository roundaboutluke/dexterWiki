# Quest Digest

Quest digest batches quest notifications into a single summary message instead of sending individual alerts for each quest.

## How It Works

1. During quiet hours (or a configured digest window), quest alerts are collected instead of sent immediately
2. When the digest window ends, all collected quests are compiled into a single message
3. The digest is delivered as one notification, giving users a clean morning overview of available quests

## Use Cases

- **Morning overview**: Set quiet hours overnight, enable quest digest, and receive a single summary of all quests found during the night
- **Reduce noise**: Instead of dozens of individual quest alerts, get one organised summary

## Configuration

Quest digest is configured per-profile through the scheduling system. See [Scheduling & Profiles](../configuration/scheduling.md) for setup details.

Users configure this via the `/profile` slash command or through legacy `!` prefix commands.

## Digest Content

The digest includes:

- Quest location (pokestop name and coordinates)
- Reward type and details (Pokemon encounter, item, stardust, candy, mega energy)
- AR/No-AR status
- Map links (if configured)

The format of the digest is controlled by DTS templates, just like regular alerts.

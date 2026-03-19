# Commands

Dexter uses Discord **slash commands** as the primary interface. Type `/` in any channel where the bot is present to see all available commands with autocomplete and descriptions.

## Slash Commands

All tracking commands create filters that tell Dexter what to notify you about. Each command supports options that refine your filter -- for example, minimum IV, distance, team, or specific Pokemon.

| Command | Description |
|---------|-------------|
| [`/pokemon`](pokemon.md) | Track wild Pokemon spawns |
| [`/raid`](raid.md) | Track raid bosses, raid levels, or eggs |
| [`/quest`](quest.md) | Track quest rewards (Pokemon, items, stardust, candy, mega energy) |
| [`/maxbattle`](maxbattle.md) | Track Dynamax and Gigantamax battles |
| [`/rocket`](rocket.md) | Track Team Rocket invasions |
| [`/gym`](gym.md) | Track gym team or slot changes |
| [`/nest`](nest.md) | Track nesting Pokemon |
| [`/weather`](weather.md) | Track weather conditions |
| [`/lure`](lure.md) | Track lure modules |
| [`/fort`](fort.md) | Track Pokestop and Gym changes (new, removed, renamed) |
| [`/pokestop-event`](pokestop-event.md) | Track Pokestop events |
| [`/profile`](profile.md) | Manage profiles, location, areas, and quiet hours |
| [`/filters`](filters.md) | Show or remove your active filters |
| [`/info`](info.md) | Look up Pokemon, moves, items, weather, and more |
| `/start` | Enable alerts |
| `/stop` | Disable alerts |
| `/language` | Change your notification language |
| `/help` | Show help for slash commands |

## Common Options

Most tracking commands share these options:

| Option | Description |
|--------|-------------|
| `distance` | Maximum distance in meters from your saved location |
| `clean` | Auto-delete the notification after the event expires |
| `template` | Use a named alert template (may be hidden by admin) |
| `profile` | Add the filter to a specific profile instead of the active one |

## Legacy Text Commands

Dexter also supports text-based commands with the `!` prefix (e.g. `!track`, `!raid`, `!quest`). These provide the same functionality as slash commands but without autocomplete or guided flows. Slash commands are recommended for new users.

Admin-only text commands such as `!channel`, `!webhook`, `!area`, and others are documented in [Admin Commands](admin.md).

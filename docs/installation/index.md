# Installation

Dexter can be installed from source or via Docker. Both methods require a MySQL/MariaDB database and a Discord or Telegram bot token.

Choose your installation method:

- [From Source](source.md) — build and run the Go binary directly
- [Docker](docker.md) — run in a container

After installation, follow these guides in order:

1. [Create a Discord Bot](discord-bot.md) or [Create a Telegram Bot](telegram-bot.md)
2. [Basic Configuration](configuration.md) — database, scanner, notification service
3. [Scanner Setup](scanner.md) — configure your webhook provider
4. [Data Generator](data-generator.md) — optional manual data refresh

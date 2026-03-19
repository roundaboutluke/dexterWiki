# Dexter Documentation

Dexter is a high-performance notification system for Pokemon GO, written in Go. It receives webhook data from scanners (such as Golbat or RDM) and sends fully customisable alerts to Discord and Telegram.

## Features

- **Discord & Telegram** — supports both simultaneously
    - Discord: direct messages, bot-based channel posting, and webhooks
    - Telegram: direct messages, group posting, and channels
- **Slash commands** — full Discord slash command support with autocomplete, localisation, and guided flows
- **High performance** — built in Go with goroutine-based concurrency; handles 20M+ events/day
    - Per-route rate management with automatic back-off
    - Automatic user timeout and suspension for overly broad tracking
- **Flexible subscriptions** — track by area or distance, with multiple profiles (e.g. Pikachu IV90+ 250m from home, IV100 within area, level 5 raids within 100m)
- **Scheduling** — active/quiet hours with automatic profile switching by time and day of week
- **Quest digest** — batch quest notifications into a single summary instead of individual alerts
- **Raid RSVP** — raid alerts update in-place with RSVP details instead of creating separate posts
- **Max Battle tracking** — dedicated alerts for Dynamax battles
- **Changed Pokemon alerts** — detect and alert when a spawn's Pokemon changes mid-lifespan
- **Quest AR/No-AR** — track and distinguish AR vs No-AR quest variants
- **Auto-deleting messages** — expired alerts are automatically cleaned up
- **Fully configurable alert templates** — multiple DTS (Delivery Template System) styles with Handlebars helpers
- **Multi-language** — translations for most major languages; users can switch language per-profile
- **PvP tracking** — internal PvP rank calculations with configurable leagues and level caps
- **Weather forecast** — AccuWeather integration with smart forecasting and weather-change alerts
- **Static maps** — tileservercache, Google Maps, Mapbox, and more with configurable templates
- **Area security** — per-community access controls with Discord role or Telegram group membership gates
- **Auto-reload** — game data (monsters, moves, items, grunts) refreshes automatically every 6 hours
- **Campfire links** — integrated Campfire support for raid coordination
- **Multiple Discord servers** — run across multiple guilds with a single instance

## Getting Started

1. [Install Dexter](installation/index.md) — from source or Docker
2. [Create your bot](installation/discord-bot.md) — Discord or Telegram
3. [Configure basics](installation/configuration.md) — database, scanner, notification service
4. [Set up tracking](commands/index.md) — start receiving alerts

## Requirements

- **Go** 1.25+ (for building from source)
- **MySQL / MariaDB** database
- **Webhook provider** — Golbat, RDM, or MAD
- **Discord bot token** and/or **Telegram bot token**

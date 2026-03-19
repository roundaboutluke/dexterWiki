# Scheduling & Profiles

Dexter supports multiple user profiles with automatic time-based switching. Each profile can have its own location, areas, distance, and template — and you define **active hours** to control when each profile fires alerts.

## Profiles

Each user can have multiple named profiles, each with its own:

- **Location** — home, office, etc.
- **Areas** — which geofence areas to track
- **Distance** — tracking radius
- **Active hours** — when this profile sends alerts
- **Template** — which DTS template to use

### Managing Profiles

Use the `/profile` slash command or legacy !profile command.

## Active Hours

Users set **active hours** — the time windows when they want to receive alerts. Outside of any active hours window, Dexter enters "quiet mode" and suppresses alerts.

This is the opposite of setting "quiet hours": you define when you **do** want alerts, and everything else is quiet.

### How It Works

1. Each profile has its own active hours schedule
2. The scheduler checks every minute whether the current time falls within any profile's active hours
3. If a match is found, that profile becomes the active profile
4. If no profile's active hours match, Dexter enters quiet mode (profile 0) — alerts are suppressed

!!! tip "Timezone awareness"
    The scheduler uses your profile's saved location to determine your local timezone. Make sure you've set a location with `/profile location set`.

### Setting Active Hours

Use /profile to configure schedules and profiles interactively in Discord.

### Schedule Format

Active hours are defined as day + time ranges:

- **Days**: `mon`, `tue`, `wed`, `thu`, `fri`, `sat`, `sun`, `weekday`, `weekend`
- **Time range**: `HH:MM-HH:MM` (24-hour format)

Examples:

- `mon 09:00-17:00` — Monday 9am to 5pm
- `weekday 08:00-18:00` — All weekdays 8am to 6pm
- `weekend 10:00-22:00` — Saturday and Sunday 10am to 10pm

### Scheduler Disabled Mode

When the scheduler is disabled, Dexter uses your **preferred profile** at all times — no time-based switching occurs. This is the default state for users who don't setup scheduling.

## Quest Digest

During quiet mode (outside active hours), quest alerts would normally be dropped. With **quest digest** enabled, Dexter batches quest alerts instead and delivers them as a single summary message when your next active hours window begins.

This is useful for getting a morning overview of available quests without being pinged overnight.

The quest digest is enabled per-profile and works alongside the scheduling system.

## Example Setups

### Home & Office

1. **Home** profile: set your home location, 500m radius, active hours `weekday 18:00-23:00` and `weekend 08:00-23:00`
2. **Office** profile: set your office location, 200m radius, active hours `weekday 09:00-17:00`
3. Each profile tracks different areas at different distances
4. Dexter automatically switches between them throughout the day

### Sleep Mode

1. Single profile with active hours `07:00-23:00` every day
2. Enable quest digest on the profile
3. Raids and spawns overnight are silently dropped
4. Quest alerts batch up and arrive as a digest at 07:00

### Weekend Warrior

1. **Weekday** profile: small radius, only rare spawns, active `weekday 08:00-22:00`
2. **Weekend** profile: large radius, broader tracking, active `weekend 08:00-23:00`
3. Different templates for different alert detail levels

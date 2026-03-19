# /profile

Manage your Dexter profile. This command opens a guided flow to configure your active profile, saved location, tracked areas, and quiet hours.

## Usage

```
/profile
```

This command takes no options. It launches an interactive menu where you can:

- **Switch profile** -- change which profile is active
- **Set location** -- save your home or preferred location for distance-based tracking
- **Manage areas** -- add or remove geographic areas for area-based tracking
- **Quiet hours** -- configure times when notifications are silenced
- **View profile details** -- see your current settings

## Profiles

Profiles let you maintain separate sets of filters. For example, you might have one profile for your home area and another for your workplace. Each tracking command accepts an optional `profile` option to add the filter to a specific profile.

## Location

Your saved location is used by any filter that specifies a `distance` option. Set it to your primary playing location for the most relevant distance-based alerts.

## Quiet Hours

Quiet hours suppress notifications during specified time windows. Notifications that occur during quiet hours are silently discarded -- they are not queued for later delivery.

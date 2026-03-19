# Data Generator

Dexter uses game data files for monster names, moves, items, grunts, quests, types, and translations. These are managed automatically but can also be refreshed manually.

## Automatic Refresh

On first startup, Dexter downloads all required data files. It then refreshes them automatically **every 6 hours** to pick up game updates (new Pokemon, moves, events, etc.).

No configuration is needed — this happens automatically.

## Manual Refresh

To regenerate data files without restarting Dexter, use the standalone generator tool:

```bash
go run ./cmd/dexter-generate
```

### Flags

| Flag | Description |
|------|-------------|
| `-latest` | Fetch the latest grunts data from remote sources |

### Example

```bash
# Standard regeneration
go run ./cmd/dexter-generate

# With latest grunt data
go run ./cmd/dexter-generate -latest
```

The generator writes files to the `data/` directory in your working directory. Dexter picks them up on next reload cycle.

## Data Files

The following files are generated:

| File | Contents |
|------|----------|
| Monsters | Pokemon names, forms, types, stats |
| Moves | Move names and types |
| Items | Item names and IDs |
| Grunts | Team Rocket grunt/leader types and lineups |
| Quests | Quest reward types |
| Types | Pokemon type names |
| Translations | Locale-specific translations |

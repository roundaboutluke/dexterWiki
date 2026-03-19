# /info

Look up game data including Pokemon stats, moves, items, weather, rarity, shiny availability, and translations.

## Usage

```
/info type:Pokemon pokemon:Mewtwo
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `type` | Choice | Category to look up. Leave blank for a guided flow. |
| `pokemon` | String | Pokemon name (used when type is `Pokemon`). Supports autocomplete. |

The `type` option accepts the following values:

| Value | Description |
|-------|-------------|
| `Pokemon` | Pokemon stats, types, and available forms |
| `Moves` | Move details and stats |
| `Items` | Item information |
| `Weather` | Weather conditions and boosted types |
| `Rarity` | Pokemon rarity tiers |
| `Shiny` | Shiny availability for Pokemon |
| `Translate` | Translate Pokemon or move names between languages |

## Examples

Look up Mewtwo's stats and forms:

```
/info type:Pokemon pokemon:Mewtwo
```

Browse available items:

```
/info type:Items
```

Check shiny availability:

```
/info type:Shiny
```

Look up weather boost information:

```
/info type:Weather
```

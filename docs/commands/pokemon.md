# /pokemon

Track wild Pokemon spawns. Dexter will send you a notification whenever a matching Pokemon appears within your configured distance or area.

## Usage

```
/pokemon pokemon:Pikachu min_iv:90 distance:500
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `pokemon` | String | Pokemon name or number. Supports autocomplete. |
| `form` | String | Specific form (e.g. Alolan, Galarian). Supports autocomplete. |
| `min_iv` | Integer (0--100) | Minimum IV percentage |
| `max_iv` | Integer (0--100) | Maximum IV percentage |
| `min_cp` | Integer | Minimum CP |
| `max_cp` | Integer | Maximum CP |
| `min_level` | Integer (1--50) | Minimum Pokemon level |
| `max_level` | Integer (1--50) | Maximum Pokemon level |
| `min_atk` | Integer (0--15) | Minimum attack IV |
| `max_atk` | Integer (0--15) | Maximum attack IV |
| `min_def` | Integer (0--15) | Minimum defense IV |
| `max_def` | Integer (0--15) | Maximum defense IV |
| `min_sta` | Integer (0--15) | Minimum stamina IV |
| `max_sta` | Integer (0--15) | Maximum stamina IV |
| `size` | Choice | Size filter: `all`, `xxs`, `xs`, `m`, `xl`, `xxl` |
| `gender` | Choice | Gender filter: `All`, `Male`, `Female` |
| `pvp_league` | Choice | PvP league: `little`, `great`, `ultra` (requires `pvp_ranks`) |
| `pvp_ranks` | Integer | Number of PvP ranks to include (requires `pvp_league`) |
| `min_time` | Integer | Minimum seconds remaining before despawn |
| `distance` | Integer | Maximum distance in meters from your saved location |
| `clean` | Flag | Auto-delete the notification after the Pokemon despawns |
| `template` | String | Named alert template to use |
| `profile` | String | Profile to add this filter to |

## Examples

Track 100 IV Ditto anywhere in your area:

```
/pokemon pokemon:Ditto min_iv:100
```

Track Great League PvP rank 1 Medicham within 1 km:

```
/pokemon pokemon:Medicham pvp_league:great pvp_ranks:1 distance:1000
```

Track all XXL Pokemon within 500 meters:

```
/pokemon size:xxl distance:500
```

Track any Pokemon with 0/0/0 IVs:

```
/pokemon max_iv:0 max_atk:0 max_def:0 max_sta:0
```

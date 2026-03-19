# /quest

Track field research quest rewards. This command uses subcommands to filter by reward type.

## Subcommands

### /quest pokemon

Track quests that reward a Pokemon encounter.

```
/quest pokemon pokemon:Chansey distance:1000
```

| Option | Type | Description |
|--------|------|-------------|
| `pokemon` | String | Reward Pokemon name or number. Supports autocomplete. |
| `ar` | Choice | AR mapping filter: `Any`, `No AR`, `With AR` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after expiration |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

### /quest item

Track quests that reward an item.

```
/quest item item:Rare Candy min_amount:3
```

| Option | Type | Description |
|--------|------|-------------|
| `item` | String | Item name. Supports autocomplete. |
| `min_amount` | Integer | Minimum item quantity |
| `ar` | Choice | AR mapping filter: `Any`, `No AR`, `With AR` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after expiration |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

### /quest stardust

Track quests that reward stardust.

```
/quest stardust min_amount:1000
```

| Option | Type | Description |
|--------|------|-------------|
| `min_amount` | Integer | Minimum stardust amount |
| `ar` | Choice | AR mapping filter: `Any`, `No AR`, `With AR` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after expiration |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

### /quest candy

Track quests that reward candy for a specific Pokemon.

```
/quest candy pokemon:Noibat
```

| Option | Type | Description |
|--------|------|-------------|
| `pokemon` | String | Pokemon whose candy is rewarded. Supports autocomplete. |
| `min_amount` | Integer | Minimum candy quantity |
| `ar` | Choice | AR mapping filter: `Any`, `No AR`, `With AR` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after expiration |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

### /quest mega-energy

Track quests that reward mega energy for a specific Pokemon.

```
/quest mega-energy pokemon:Charizard min_amount:50
```

| Option | Type | Description |
|--------|------|-------------|
| `pokemon` | String | Pokemon whose mega energy is rewarded. Supports autocomplete. |
| `min_amount` | Integer | Minimum mega energy amount |
| `ar` | Choice | AR mapping filter: `Any`, `No AR`, `With AR` |
| `distance` | Integer | Maximum distance in meters |
| `clean` | Flag | Auto-delete after expiration |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## AR Filter

The `ar` option filters quests based on whether they require AR mapping:

- **Any** -- match all quests regardless of AR status (default)
- **No AR** -- only quests that do not require AR scanning
- **With AR** -- only quests that require AR scanning

## Examples

Track all Rare Candy quests with at least 3 candies, within 2 km:

```
/quest item item:Rare Candy min_amount:3 distance:2000
```

Track non-AR stardust quests awarding 1500 or more:

```
/quest stardust min_amount:1500 ar:No AR
```

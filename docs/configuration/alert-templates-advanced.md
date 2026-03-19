# Alert Templates -- Advanced

This page covers Dexter's Handlebars helpers, conditional logic, custom maps, partials, and complex template patterns. For the basics of DTS structure and template variables, see [Alert Templates](alert-templates.md).

## Handlebars helpers reference

Dexter registers a comprehensive set of Handlebars helpers beyond the built-in `{{#if}}`, `{{#each}}`, and `{{#unless}}`. All helpers can be used as inline expressions or as block helpers where noted.

### Comparison helpers

These helpers work both inline (returning a truthy/empty string) and as block helpers (with `{{else}}` support).

| Helper | Usage | Description |
|--------|-------|-------------|
| `eq` / `equals` | `{{#eq iv '100.00'}}...{{/eq}}` | Strict equality (string comparison) |
| `is` | `{{#is gender 1}}...{{/is}}` | Same as `eq` |
| `isnt` / `ne` / `neq` / `notEq` / `notEquals` | `{{#isnt formName 'Normal'}}...{{/isnt}}` | Not equal |
| `gt` | `{{#gt level 30}}...{{/gt}}` | Greater than |
| `gte` | `{{#gte iv 90}}...{{/gte}}` | Greater than or equal |
| `lt` | `{{#lt cp 100}}...{{/lt}}` | Less than |
| `lte` | `{{#lte size 1}}...{{/lte}}` | Less than or equal |
| `compare` | `{{#compare rank '<=' 10}}...{{/compare}}` | General comparison with operator |
| `not` | `{{#not confirmedTime}}...{{/not}}` | Negation |
| `isFalsey` | `{{#isFalsey value}}...{{/isFalsey}}` | True when value is falsy |
| `contains` / `includes` | `{{#contains areas 'downtown'}}...{{/contains}}` | Check if string/array/map contains a value |
| `startsWith` | `{{#startsWith name 'Mega'}}...{{/startsWith}}` | String prefix check |
| `endsWith` | `{{#endsWith name 'X'}}...{{/endsWith}}` | String suffix check |

**The `compare` helper** accepts these operators: `==`, `!=`, `<`, `<=`, `>`, `>=`:

```handlebars
{{#compare gruntRewardsList.first.chance '==' 100}}
  Guaranteed: {{#forEach gruntRewardsList.first.monsters}}{{this.name}}{{/forEach}}
{{/compare}}
{{#compare gruntRewardsList.first.chance '<' 100}}
  {{gruntRewardsList.first.chance}}%: ...
{{/compare}}
```

### Logic helpers

| Helper | Usage | Description |
|--------|-------|-------------|
| `or` | `{{#or pvpGreat pvpUltra}}...{{/or}}` | Logical OR (two arguments) |
| `and` | `{{#and confirmedTime boosted}}...{{/and}}` | Logical AND (two arguments) |

These work as block helpers with `{{else}}` support, or inline returning truthy/empty.

### Numeric helpers

| Helper | Usage | Description |
|--------|-------|-------------|
| `round` | `{{round iv}}` | Round to nearest integer |
| `numberFormat` / `toFixed` | `{{numberFormat iv 2}}` | Format to N decimal places |
| `add` | `{{add level 5}}` | Addition |
| `subtract` | `{{subtract cp 100}}` | Subtraction |
| `minus` | `{{minus lureTypeId 500}}` | Subtraction (alias) |
| `multiply` | `{{multiply weight 2.2}}` | Multiplication |
| `divide` | `{{divide cp 10}}` | Division |
| `mod` | `{{mod pokemonId 3}}` | Modulus |
| `sum` | `{{sum atk def sta}}` | Sum multiple values |
| `ceil` | `{{ceil iv}}` | Round up |
| `floor` | `{{floor iv}}` | Round down |
| `abs` | `{{abs value}}` | Absolute value |
| `min` | `{{min atk def}}` | Minimum of two values |
| `max` | `{{max atk def}}` | Maximum of two values |
| `addCommas` | `{{addCommas stardust}}` | Format number with commas (e.g. `1,234,567`) |
| `pad0` | `{{pad0 pokemonId}}` | Zero-pad to 3 digits (e.g. `021`) |

`pad0` accepts an optional second argument for pad width: `{{pad0 pokemonId 4}}` gives `0021`.

### String helpers

| Helper | Usage | Description |
|--------|-------|-------------|
| `uppercase` / `upper` / `toUpperCase` | `{{uppercase name}}` | Convert to uppercase |
| `lowercase` / `lower` / `toLowerCase` | `{{lowercase nameEng}}` | Convert to lowercase |
| `capitalize` | `{{capitalize weatherNextName}}` | Capitalise first letter |
| `pvpSlug` | `{{pvpSlug fullName}}` | Convert name to PVP-safe slug format |
| `concat` | `{{concat pokemonId '_' formId}}` | Concatenate strings (e.g. `5_0`) |
| `replace` / `replaceAll` | `{{replace name " " "_"}}` | Replace all occurrences |
| `replaceFirst` | `{{replaceFirst name "Mega " ""}}` | Replace first occurrence only |
| `split` | `{{split areas ","}}` | Split string into array |
| `join` | `{{join matched ", "}}` | Join array into string |
| `length` | `{{length pvpGreat}}` | Length of array, map, or string |
| `default` | `{{default formName "Normal"}}` | Return fallback if value is falsy |
| `json` / `stringify` | `{{json someObject}}` | Serialise value as JSON string |

#### pvpSlug

The `pvpSlug` helper converts Pokemon names into URL-safe identifiers suitable for PVP lookup sites. It lowercases the name, replaces spaces and special characters with underscores, converts gender symbols (e.g. Nidoran), and title-cases each segment:

```handlebars
{{pvpSlug fullName}}
{{!-- "Nidoran ♀" becomes "Nidoran_Female" --}}
{{!-- "Galarian Stunfisk" becomes "Galarian_Stunfisk" --}}
```

### Pokemon data helpers

| Helper | Usage | Description |
|--------|-------|-------------|
| `pokemonName` | `{{pokemonName 25}}` | Look up pokemon name by ID (localised) |
| `pokemonNameEng` | `{{pokemonNameEng 25}}` | Pokemon name by ID (English) |
| `pokemonNameAlt` | `{{pokemonNameAlt 25}}` | Pokemon name by ID (alt language) |
| `pokemonForm` | `{{pokemonForm 65}}` | Form name by form ID (localised) |
| `pokemonFormEng` | `{{pokemonFormEng 65}}` | Form name by form ID (English) |
| `pokemonFormAlt` | `{{pokemonFormAlt 65}}` | Form name by form ID (alt language) |
| `pokemonBaseStats` | `{{pokemonBaseStats 25}}` | Base stats object for a pokemon ID |
| `calculateCp` | `{{calculateCp baseStats 20 15 15 15}}` | Calculate CP from base stats, level, and IVs |
| `moveName` | `{{moveName move_1}}` | Move name by ID (localised) |
| `moveNameEng` | `{{moveNameEng move_1}}` | Move name by ID (English) |
| `moveNameAlt` | `{{moveNameAlt move_1}}` | Move name by ID (alt language) |
| `moveType` | `{{moveType move_1}}` | Move type name by move ID |
| `moveEmoji` | `{{moveEmoji move_1}}` | Move type emoji by move ID |
| `translateAlt` | `{{translateAlt "Fire"}}` | Translate text to alt language |
| `getEmoji` | `{{{getEmoji 'great-league'}}}` | Look up a custom emoji by name |

#### calculateCp

Calculates the CP for a pokemon given its base stats, level, and individual IVs:

```handlebars
{{!-- Perfect IV at level 20 (normal catch) --}}
{{calculateCp baseStats 20 15 15 15}}

{{!-- Perfect IV at level 25 (weather boosted catch) --}}
{{calculateCp baseStats 25 15 15 15}}

{{!-- Look up stats by pokemon ID, then calculate --}}
{{calculateCp (pokemonBaseStats 150) 20 15 15 15}}
```

#### getPowerUpCost

Calculates the stardust and candy cost to power up from one level to another:

```handlebars
{{!-- Inline: returns "X Stardust and Y Candies" --}}
{{getPowerUpCost 20 40}}

{{!-- Block: access individual values --}}
{{#getPowerUpCost ../level this.level}}
  Stardust: {{addCommas stardust}} | Candy: {{candy}}{{#compare xlCandy '>' 0}} | XL: {{xlCandy}}{{/compare}}
{{/getPowerUpCost}}
```

#### pokemon block helper

The `{{#pokemon}}` block helper looks up full pokemon data and exposes it as context variables:

```handlebars
{{#pokemon pokemonId formId}}
  {{fullName}} - Types: {{typeEmoji}}
  {{#if hasEvolutions}}Can evolve{{/if}}
{{/pokemon}}
```

Available inside the block: `name`, `nameEng`, `formName`, `fullName`, `fullNameEng`, `emoji`, `typeEmoji`, `typeName`, `hasEvolutions`, `baseStats`.

### Special helpers

#### ex

The `{{#ex}}` block helper checks whether a gym is EX-eligible:

```handlebars
{{#ex}}(EX Eligible){{/ex}}
```

It checks `ex`, `is_exclusive`, and `exclusive` fields from the data.

#### forEach

An enhanced loop helper that provides `isFirst` and `isLast` context flags:

```handlebars
{{#forEach gruntRewardsList.first.monsters}}
  {{this.name}}{{#unless isLast}}, {{/unless}}
{{/forEach}}
```

## Conditional blocks

### Basic if/else

```handlebars
{{#if confirmedTime}}
  Verified despawn: {{time}}
{{else}}
  Despawn time unknown
{{/if}}
```

### Nested conditions

```handlebars
{{#if userHasPvpTracks}}
  {{#or pvpGreat pvpUltra pvpLittle}}
    {{#if pvpGreat}} GL#{{pvpGreatBest.rank}}{{/if}}
    {{#if pvpUltra}} UL#{{pvpUltraBest.rank}}{{/if}}
  {{/or}}
{{else}}
  {{round iv}}% ({{atk}}/{{def}}/{{sta}})
{{/if}}
```

### Combining with sub-expressions

Handlebars sub-expressions (parenthesised helpers) can be used as arguments:

```handlebars
{{#or (lte size 1) (gte size 5)}}
  Size: {{sizeName}}
{{/or}}

{{#if (eq level 8)}}
  Shadow Raid
{{/if}}
```

### Inline conditionals

Many comparison helpers return a truthy string when used inline (without a block), so they can be placed directly in text:

```handlebars
{{fullName}} {{#eq iv '100.00'}}💯{{/eq}} {{boostWeatherEmoji}}
```

## The `@include` directive

Any string value in a DTS template can reference an external file:

```json
"description": "@include mon.txt"
```

Dexter loads the file from the `config/dts/` directory and renders it as a Handlebars template with the same context data. This keeps complex templates manageable.

Example `config/dts/mon.txt`:

```handlebars
CP {{cp}} | Lvl {{level}}
{{#if streetName}}📍 {{{addr}}}{{/if}}
{{{quickMoveEmoji}}} {{quickMoveName}} {{{chargeMoveEmoji}}} {{chargeMoveName}}

{{#each pvpGreat}}{{#if this.rank}}{{#compare this.rank '<=' ../pvpUserRanking}}
GL: {{this.fullName}} #{{this.rank}} ({{this.cp}}CP Lvl. {{this.levelWithCap}})
{{/compare}}{{/if}}{{/each}}

Disappears <t:{{disappear_time}}:R>
[Google]({{{googleMapUrl}}}) | [Apple]({{{appleMapUrl}}})
```

## Custom DTS dictionary (Custom Maps)

Dexter supports a mapping system that lets you define custom lookup tables and use them in your templates. Place JSON files in `config/customMaps/`.

### Defining a map

Each map file contains a JSON object (or array of objects) with a `name` and a `map` of key-value pairs:

```json
{
  "name": "raidCounters",
  "map": {
    "144": {
      "trainers": "2",
      "counters": "Rampardos, Rhyperior, Terrakion"
    },
    "145": {
      "trainers": "3",
      "counters": "Shadow Mamoswine, Rhyperior, Mamoswine"
    }
  }
}
```

### Using a map

**Simple value lookup** (when the map value is a plain string):

```handlebars
{{map 'myMapName' someVariable}}
```

**Block form** (when the map value is an object with sub-fields):

```handlebars
{{#map 'raidCounters' pokemonId}}
  Trainers needed: {{trainers}}
  Best counters: {{{counters}}}
{{/map}}
```

**map2** works the same way but tries a second key as fallback:

```handlebars
{{#map2 'raidCounters' pokemonId formId}}...{{/map2}}
```

### Language-specific maps

Maps can include a `language` field. Dexter will prefer a map entry whose language matches the user's locale, falling back to entries without a language:

```json
[
  {
    "name": "areaDescriptions",
    "language": "en",
    "map": { "downtown": "City Centre" }
  },
  {
    "name": "areaDescriptions",
    "language": "de",
    "map": { "downtown": "Innenstadt" }
  }
]
```

### Practical map examples

**Area name mapping** -- convert geofence names into user-friendly descriptions:

```json
{
  "name": "arealist",
  "map": {
    "zone1": "North Side Park",
    "zone2": "Harbour District"
  }
}
```

```handlebars
{{#each matched}}{{map 'arealist' this}}{{/each}}
```

**Time-of-day emoji:**

```json
{
  "name": "timeEmoji",
  "map": {
    "00:00": "🌙", "01:00": "🌙", "06:00": "🌅",
    "12:00": "☀️", "18:00": "🌆", "21:00": "🌙"
  }
}
```

```handlebars
{{map 'timeEmoji' weatherChangeTime}}
```

## Custom emoji

Dexter loads custom emoji definitions from `config/emoji.json`. This file maps emoji names to platform-specific strings:

```json
{
  "discord": {
    "great-league": "<:great_league:123456789>",
    "ultra-league": "<:ultra_league:123456789>",
    "stardust": "<:stardust:123456789>",
    "rarecandy": "<:rare_candy:123456789>",
    "rarecandyxl": "<:rare_candy_xl:123456789>"
  }
}
```

Use them in templates with `{{{getEmoji 'great-league'}}}` (triple braces since emoji strings may contain special characters).

## Complex template examples

### Monster with PVP, weather warning, and power-up costs

```handlebars
{{fullName}} {{genderData.emoji}} {{shinyPossibleEmoji}}
{{#eq iv '100.00'}} 💯{{/eq}}
{{#if userHasPvpTracks}}
  {{#if pvpGreat}} GL#{{pvpGreatBest.rank}}{{/if}}
  {{#if pvpUltra}} UL#{{pvpUltraBest.rank}}{{/if}}
{{else}}
  {{round iv}}% ({{atk}}/{{def}}/{{sta}})
{{/if}}

CP {{cp}} | Lvl {{level}}
{{#if streetName}}📍 {{{addr}}}{{/if}}
{{#or (lte size 1) (gte size 5)}}📐 {{sizeName}}{{/or}}

{{{quickMoveEmoji}}} {{quickMoveName}} {{{chargeMoveEmoji}}} {{chargeMoveName}}

{{#if weatherChange}}
  ⚠️ {{weatherCurrentEmoji}} Weather may change {{weatherNextEmoji}} ({{capitalize weatherNextName}})
{{/if}}

{{#each pvpGreat}}{{#if this.rank}}{{#compare this.rank '<=' ../pvpUserRanking}}
{{{getEmoji 'great-league'}}} {{this.fullName}} #{{this.rank}} ({{this.cp}}CP Lvl. {{this.levelWithCap}})
{{#getPowerUpCost ../level this.level}}
  {{{getEmoji 'stardust'}}}{{addCommas stardust}} & {{{getEmoji 'rarecandy'}}}{{candy}}
  {{#compare xlCandy '>' 0}}{{{getEmoji 'rarecandyxl'}}}{{xlCandy}}{{/compare}}
{{/getPowerUpCost}}
[PvPoke](https://pvpoke.com/rankings/all/1500/overall/{{lowercase this.nameEng}}/)
{{/compare}}{{/if}}{{/each}}

Disappears <t:{{disappear_time}}:R>
🗺️ [Google]({{{googleMapUrl}}}) | [Apple]({{{appleMapUrl}}})
```

### Invasion with conditional display types

```handlebars
{{#if (eq displayTypeId 8)}}🦎 Kecleon
{{else if (eq displayTypeId 7)}}🪙 Gold Stop
{{else if (eq displayTypeId 9)}}🏆 Showcase
{{else}}🚀 {{gruntType}} {{genderData.emoji}}
{{/if}} @ {{{pokestopName}}}

⏰ Ends: {{time}} (<t:{{incidentExpiration}}:R>)

{{#if (lte displayTypeId 6)}}
🥊 Type: {{gruntType}} {{{gruntTypeEmoji}}}{{genderData.emoji}}

🏆 Rewards:
{{#compare gruntRewardsList.first.chance '==' 100}}
  {{#forEach gruntRewardsList.first.monsters}}{{this.name}}{{#unless isLast}}, {{/unless}}{{/forEach}}
{{/compare}}
{{#compare gruntRewardsList.first.chance '<' 100}}
  {{gruntRewardsList.first.chance}}%: {{#forEach gruntRewardsList.first.monsters}}{{this.name}}{{#unless isLast}}, {{/unless}}{{/forEach}}
  {{gruntRewardsList.second.chance}}%: {{#forEach gruntRewardsList.second.monsters}}{{this.name}}{{#unless isLast}}, {{/unless}}{{/forEach}}
{{/compare}}
{{/if}}

🗺 [Google]({{{googleMapUrl}}}) | [Apple]({{{appleMapUrl}}})
```

### Fort update with edit type detection

```handlebars
{{#if isRemoval}}🗑️{{/if}}
{{#if isEdit}}📝{{/if}}
{{#if isNew}}🆕{{/if}}
{{#eq fortType 'pokestop'}}📍{{else}}🏟{{/eq}}
{{{name}}} | {{areas}}

{{#if isEditName}}
📛 Name: {{{newName}}} (was: {{{oldName}}})
{{/if}}
{{#if isEditDescription}}
📖 Description: {{{newDescription}}} (was: {{{oldDescription}}})
{{/if}}
{{#if isEditLocation}}
🛰️ Location: [New]({{{googleMapUrl}}}) [Old](https://maps.google.com/?q={{oldLatitude}},{{oldLongitude}})
{{/if}}
{{#if isEditImgUrl}}
🖼️ Image: [New]({{{newImageUrl}}}) [Old]({{{oldImageUrl}}})
{{/if}}
{{#if isRemoval}}
{{name}} has been removed
{{/if}}
{{#if isNew}}
{{#eq fortType 'pokestop'}}Pokestop{{else}}Gym{{/eq}} appeared
{{#isnt name 'unknown'}} - {{{name}}}{{/isnt}}
{{/if}}
```

### Raid with catch CP and counters from custom map

```handlebars
{{fullName}} {{genderData.emoji}} {{shinyPossibleEmoji}} | {{{gymName}}}{{#ex}} (EX){{/ex}}
⏰ Ends: {{time}} (<t:{{end}}:R>)

🥊 {{quickMoveName}} / {{chargeMoveName}}
💀 Weaknesses: {{{weaknessEmoji}}}

📊 Catch CP:
  Normal: {{calculateCp baseStats 20 10 10 10}}-{{calculateCp baseStats 20 15 15 15}}
  Boosted: {{calculateCp baseStats 25 10 10 10}}-{{calculateCp baseStats 25 15 15 15}} {{boostingWeathersEmoji}}

{{#map 'raidCounters' pokemonId}}
👥 Trainers: {{trainers}}
⚔️ Counters: {{{counters}}}
{{/map}}

🗺 [Google]({{{googleMapUrl}}}) | [Apple]({{{appleMapUrl}}})
```

## Partials

Dexter loads `.hbs` files from the `config/dts/` directory as Handlebars partials. You can reference them with the standard partial syntax:

```handlebars
{{> myPartialName}}
```

This is different from `@include` -- partials are registered at startup and available by name across all templates, while `@include` is a per-field file reference processed at render time.

## Debugging templates

If a template fails to render, Dexter logs the error. Common issues:

- **Missing triple braces on URLs** -- `{{imgUrl}}` will HTML-encode ampersands in the URL, breaking it. Use `{{{imgUrl}}}`.
- **Unclosed blocks** -- every `{{#if}}` needs `{{/if}}`, every `{{#each}}` needs `{{/each}}`.
- **Wrong variable name** -- check the variable tables in [Alert Templates](alert-templates.md) for the exact names. Variable names are case-sensitive.
- **Using `@include` with wrong path** -- the file must exist in `config/dts/` and the directive must be `@include filename.txt` (with a space, no quotes).

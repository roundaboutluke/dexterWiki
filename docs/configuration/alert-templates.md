# Alert Templates (DTS)

DTS stands for **Dynamic Template Strings** -- the system that controls how Dexter formats every alert message it sends. Each alert type (monsters, raids, eggs, quests, invasions, lures, nests, weather changes, gym updates, fort updates, and max battles) has its own template that you can fully customise.

Templates use [Handlebars](https://handlebarsjs.com/) syntax. Variables are wrapped in `{{ }}` and rendered at send time with live data from the webhook event.

## File structure

The main DTS configuration lives in `config/dts.json`. Dexter ships a default version in `config/defaults/dts.json` -- if you do not have your own `dts.json`, the default is copied into `config/` on first run.

You can also split templates across multiple files by placing additional `.json` files in the `config/dts/` directory. Dexter loads and merges all of them, so you can organise by language, alert type, or platform.

Additionally, any template string field can reference an external text file with the `@include` directive:

```json
"description": "@include mon.txt"
```

Dexter will look for `mon.txt` inside the `config/dts/` folder, load its contents, and render them as a Handlebars template. This is useful for keeping complex description blocks readable and version-controlled separately.

## Anatomy of a DTS entry

Each entry in `dts.json` (or a supplementary file) is a JSON object with these top-level fields:

```json
{
  "id": "standard",
  "type": "monster",
  "language": "en",
  "default": true,
  "platform": "discord",
  "template": {
    ...
  }
}
```

| Field | Description |
|-------|-------------|
| `id` | Template identifier. Users can select a specific template with `template<id>` on tracking commands (e.g. `!track everything iv100 templatehundo`). |
| `type` | The alert type this template applies to. See [Alert types](#alert-types) below. |
| `language` | Language code (e.g. `en`, `de`, `fr`). Dexter matches templates whose language matches the user's configured locale, falling back to entries with no language set. |
| `default` | When `true`, this template is used if no specific template ID is matched. The first entry with `default: true` for a given type/platform/language wins. |
| `platform` | Either `discord` or `telegram`. |
| `template` | The message payload. Structure depends on platform. |

## Alert types

| Type | Description |
|------|-------------|
| `monster` | Pokemon with IV data available |
| `monsterNoIv` | Pokemon without IV data (unscanned) |
| `monsterchange` | A spawn changed species (with IV) |
| `monsterchangeNoIv` | A spawn changed species (without IV) |
| `raid` | Active raid boss |
| `egg` | Raid egg (before hatching) |
| `quest` | Field research quest |
| `invasion` | Team Rocket invasion / Kecleon / Gold Stop / Showcase |
| `lure` | Active lure module |
| `nest` | Pokemon nest |
| `gym` | Gym team change or slot availability |
| `weatherchange` | Weather change affecting tracked pokemon |
| `fort-update` | Pokestop or gym added, removed, or edited |
| `maxbattle` | Dynamax / max battle at a Power Spot |
| `greeting` | Welcome message sent to newly registered users |
| `help` | Help message shown via the help command |
| `questdigest` | Summary of quests missed during quiet hours |

## Platform template structures

### Discord

Discord templates use the [embed object format](https://discord.com/developers/docs/resources/message#embed-object). A typical template:

```json
"template": {
  "embed": {
    "color": "{{color}}",
    "title": "{{fullName}} {{genderData.emoji}}",
    "url": "{{{reactMapUrl}}}",
    "description": "Your message content here",
    "timestamp": "{{nowISO}}",
    "footer": {
      "text": "My Bot"
    },
    "thumbnail": {
      "url": "{{{imgUrl}}}"
    },
    "image": {
      "url": "{{{staticMap}}}"
    }
  }
}
```

For webhook channels (as opposed to bot DMs), you can also set `username` and `avatar_url` at the top level of the template to override the webhook identity per message:

```json
"template": {
  "username": "{{name}}",
  "avatar_url": "{{{imgUrl}}}",
  "embed": { ... }
}
```

Embeds can also include `fields` as an array and an `author` block.

### Telegram

Telegram templates use a flat structure:

```json
"template": {
  "content": "Your message text with **Markdown**",
  "parse_mode": "Markdown",
  "sticker": "{{{stickerUrl}}}",
  "location": true,
  "webpage_preview": true
}
```

| Field | Description |
|-------|-------------|
| `content` | The message body. Supports Markdown or HTML depending on `parse_mode`. |
| `parse_mode` | `Markdown` or `HTML`. |
| `sticker` | URL for a sticker image sent before the message. |
| `location` | `true` to include a Telegram location pin. |
| `webpage_preview` | `true` to allow link previews. Useful for embedding static map images. |
| `send_order` | Array to reorder message parts, e.g. `["sticker", "location", "text"]`. Default order is sticker, text, location. |

!!! tip "Static maps in Telegram"
    To embed a map image, include a zero-width link at the start of your content:
    ```
    "content": "[\u200A]({{{staticMap}}}) Your message here..."
    ```
    Set `webpage_preview` to `true` so Telegram renders the image.

## Triple braces

Handlebars HTML-escapes values by default when you use `{{value}}`. For URLs and fields containing special characters (like `<`, `>`, `&`), use triple braces `{{{value}}}` to output the raw unescaped value. This is essential for:

- `{{{imgUrl}}}`
- `{{{staticMap}}}`
- `{{{googleMapUrl}}}`
- `{{{appleMapUrl}}}`
- `{{{reactMapUrl}}}`
- `{{{pokestopName}}}` / `{{{gymName}}}` (may contain special characters)

## Template variables by alert type

### Common variables (available on most alert types)

These location and map variables are available on nearly all alert types:

| Variable | Description |
|----------|-------------|
| `{{latitude}}` | Latitude of the location |
| `{{longitude}}` | Longitude of the location |
| `{{addr}}` | Formatted address string |
| `{{streetNumber}}` | Street number |
| `{{streetName}}` | Street name |
| `{{zipcode}}` | Postal / zip code |
| `{{city}}` | City name |
| `{{state}}` | State / region name |
| `{{stateCode}}` | 2-letter state code |
| `{{country}}` | Country name |
| `{{countryCode}}` | 2-letter country code |
| `{{neighbourhood}}` | Neighbourhood name |
| `{{flag}}` | Country flag emoji |
| `{{areas}}` | Comma-separated matched geofence area names |
| `{{matched}}` | Matched areas as an array (for iteration) |
| `{{distance}}` | Distance in metres from user location to the alert |
| `{{bearing}}` | Compass bearing from user location |
| `{{bearingEmoji}}` | Directional arrow emoji |
| `{{now}}` | Current timestamp (formatted) |
| `{{nowISO}}` | Current timestamp in ISO 8601 |
| `{{{staticMap}}}` | Static map image URL |
| `{{{googleMapUrl}}}` | Google Maps link |
| `{{{appleMapUrl}}}` | Apple Maps link |
| `{{{wazeMapUrl}}}` | Waze Maps link |
| `{{{reactMapUrl}}}` | ReactMap / web map link |

---

### Monster (`monster` / `monsterNoIv`)

| Variable | Description |
|----------|-------------|
| `{{pokemonId}}` / `{{id}}` | Pokemon ID number |
| `{{name}}` | Pokemon name (localised) |
| `{{nameEng}}` | Pokemon name (English) |
| `{{fullName}}` | Name including form (localised) |
| `{{formName}}` | Form name (localised) |
| `{{formNameEng}}` | Form name (English) |
| `{{formId}}` | Form ID number |
| `{{generation}}` | Generation number |
| `{{generationName}}` | Generation name (localised) |
| `{{generationRoman}}` | Roman numeral generation (I, II, III...) |
| `{{disguisePokemonName}}` | Disguise name (Ditto) |
| `{{encounterId}}` | Scanner encounter ID |
| `{{time}}` | Despawn time (formatted) |
| `{{tthh}}` | Hours until despawn |
| `{{tthm}}` | Minutes until despawn |
| `{{tths}}` | Seconds until despawn |
| `{{disappear_time}}` | Unix timestamp of despawn (for Discord `<t:>` formatting) |
| `{{confirmedTime}}` | Whether despawn time is verified |
| `{{iv}}` | IV percentage (2 decimal places) |
| `{{cp}}` | Combat Power |
| `{{level}}` | Pokemon level |
| `{{atk}}` | Attack IV (0-15) |
| `{{def}}` | Defence IV (0-15) |
| `{{sta}}` | Stamina IV (0-15) |
| `{{weight}}` | Weight in kg |
| `{{height}}` | Height |
| `{{size}}` | Size rating (1=XXS, 5=XXL) |
| `{{sizeName}}` | Size name |
| `{{gender}}` | Gender ID (0=unset, 1=male, 2=female, 3=genderless) |
| `{{genderData.name}}` | Gender name |
| `{{genderData.emoji}}` | Gender emoji |
| `{{quickMoveId}}` | Quick move ID |
| `{{quickMoveName}}` | Quick move name (localised) |
| `{{quickMoveNameEng}}` | Quick move name (English) |
| `{{quickMoveEmoji}}` | Quick move type emoji |
| `{{chargeMoveId}}` | Charge move ID |
| `{{chargeMoveName}}` | Charge move name (localised) |
| `{{chargeMoveNameEng}}` | Charge move name (English) |
| `{{chargeMoveEmoji}}` | Charge move type emoji |
| `{{color}}` | Embed colour (primary type) |
| `{{ivColor}}` | Embed colour based on IV perfection |
| `{{{imgUrl}}}` | Pokemon image URL |
| `{{{stickerUrl}}}` | Sticker image URL |
| `{{typeName}}` | Pokemon type (localised) |
| `{{emojiString}}` | Type emoji string |
| `{{rarityName}}` | Rarity tier (localised) |
| `{{rarityNameEng}}` | Rarity tier (English) |
| `{{shinyPossible}}` | `true`/`false` shiny availability |
| `{{shinyPossibleEmoji}}` | Sparkle emoji if shiny possible |
| `{{catchBase}}` | Base catch rate % |
| `{{catchGreat}}` | Great Ball catch rate % |
| `{{catchUltra}}` | Ultra Ball catch rate % |

**Weather variables (monster):**

| Variable | Description |
|----------|-------------|
| `{{boosted}}` | Whether the Pokemon is weather boosted |
| `{{boostWeatherName}}` | Name of boosting weather |
| `{{boostWeatherEmoji}}` | Emoji for boosting weather |
| `{{gameWeatherName}}` | Current in-game weather name |
| `{{gameWeatherEmoji}}` | Current in-game weather emoji |
| `{{weatherChange}}` | Indicates a weather change may occur |
| `{{weatherCurrentName}}` | Current weather name |
| `{{weatherCurrentEmoji}}` | Current weather emoji |
| `{{weatherNextName}}` | Next weather forecast name |
| `{{weatherNextEmoji}}` | Next weather forecast emoji |
| `{{weatherChangeTime}}` | Time of weather change (HH:00) |

**PVP variables (monster):**

PVP data is available as arrays you iterate over:

| Variable | Description |
|----------|-------------|
| `{{pvpLittle}}` | Array of Little League PVP entries |
| `{{pvpGreat}}` | Array of Great League PVP entries |
| `{{pvpUltra}}` | Array of Ultra League PVP entries |
| `{{pvpLittleBest}}` | Best Little League entry |
| `{{pvpGreatBest}}` | Best Great League entry |
| `{{pvpUltraBest}}` | Best Ultra League entry |
| `{{userHasPvpTracks}}` | Whether the user has PVP tracking active |
| `{{pvpUserRanking}}` | User's configured max PVP rank to display |

Each PVP entry (inside `{{#each pvpGreat}}` etc.) exposes:

| Field | Description |
|-------|-------------|
| `this.fullName` | Full name including form and mega |
| `this.fullNameEng` | English version |
| `this.rank` | PVP rank |
| `this.cp` | CP at that rank |
| `this.level` | Level at that rank |
| `this.cap` | Level cap (e.g. 50 or 51) |
| `this.levelWithCap` | Combined level/cap display |
| `this.percentage` | Stat product percentage |
| `this.evolution` | `true` if mega evolution |
| `this.baseStats` | Base stats (for `calculateCp`) |
| `this.name` / `this.nameEng` | Pokemon name |
| `this.monsterName` | Just the pokemon name |
| `this.formName` | Just the form name |

**PVP example:**

```handlebars
{{#each pvpGreat}}{{#if this.rank}}{{#compare this.rank '<=' ../pvpUserRanking}}
Great League: {{this.fullName}} #{{this.rank}} @{{this.cp}}CP (Lvl. {{this.levelWithCap}})
{{/compare}}{{/if}}{{/each}}
```

**Monster change variables (`monsterchange` / `monsterchangeNoIv`):**

In addition to the standard monster variables for the new spawn, these are available:

| Variable | Description |
|----------|-------------|
| `{{oldFullName}}` | Previous pokemon's full name |
| `{{oldIv}}` | Previous pokemon's IV |
| `{{oldIvKnown}}` | Whether the old IV was known |
| `{{oldCp}}` | Previous pokemon's CP |

---

### Raid (`raid`)

| Variable | Description |
|----------|-------------|
| `{{pokemonId}}` / `{{id}}` | Pokemon ID |
| `{{name}}` | Boss name (localised) |
| `{{fullName}}` | Boss name with form |
| `{{{gymName}}}` | Gym name |
| `{{level}}` | Raid level / tier |
| `{{time}}` | Raid end time |
| `{{tthh}}` | Hours until raid ends |
| `{{tthm}}` | Minutes until raid ends |
| `{{tths}}` | Seconds until raid ends |
| `{{end}}` | Unix timestamp of raid end |
| `{{hatchTime}}` | Hatch time |
| `{{formId}}` | Form ID |
| `{{formName}}` | Form name |
| `{{cp}}` | Raid boss CP |
| `{{quickMoveName}}` | Quick move name |
| `{{chargeMoveName}}` | Charge move name |
| `{{quickMoveEmoji}}` | Quick move type emoji |
| `{{chargeMoveEmoji}}` | Charge move type emoji |
| `{{teamName}}` | Gym's controlling team |
| `{{description}}` | Gym description |
| `{{{gymUrl}}}` | Gym image URL |
| `{{gymColor}}` | Embed colour for gym team |
| `{{ex}}` | EX-eligible gym (truthy/falsy) |
| `{{genderData.emoji}}` | Boss gender emoji |
| `{{shinyPossibleEmoji}}` | Sparkle if shiny possible |
| `{{baseStats}}` | Base stats object (for `calculateCp`) |
| `{{{weaknessEmoji}}}` | Type weakness emojis |
| `{{boostingWeathersEmoji}}` | Weather boost emojis |
| `{{{imgUrl}}}` | Boss image URL |

**Catch CP calculation:**

```handlebars
Normal: {{calculateCp baseStats 20 10 10 10}}-{{calculateCp baseStats 20 15 15 15}}
Boosted: {{calculateCp baseStats 25 10 10 10}}-{{calculateCp baseStats 25 15 15 15}}
```

**RSVP data (if enabled):**

```handlebars
{{#if rsvps}}{{#each rsvps}}
{{time}} <t:{{timeSlot}}:R> Going: {{goingCount}} Maybe: {{maybeCount}}
{{/each}}{{/if}}
```

---

### Egg (`egg`)

| Variable | Description |
|----------|-------------|
| `{{{gymName}}}` | Gym name |
| `{{level}}` | Egg level / tier |
| `{{levelName}}` | Level name (e.g. "Mega Raid", "5-Star Raid") |
| `{{time}}` | Raid end time |
| `{{hatchTime}}` | Hatch time |
| `{{tthh}}` | Hours until hatch |
| `{{tthm}}` | Minutes until hatch |
| `{{tths}}` | Seconds until hatch |
| `{{teamName}}` | Gym's controlling team |
| `{{teamEmoji}}` | Team emoji |
| `{{description}}` | Gym description |
| `{{gymColor}}` | Embed colour for gym team |
| `{{ex}}` | EX-eligible gym |
| `{{{gymUrl}}}` | Gym image URL |
| `{{{imgUrl}}}` | Egg image URL |

---

### Quest (`quest`)

| Variable | Description |
|----------|-------------|
| `{{{pokestopName}}}` | Pokestop name |
| `{{pokestopUrl}}` | Pokestop image URL |
| `{{{questString}}}` | Quest task description (e.g. "Battle in 3 raids") |
| `{{{rewardString}}}` | Reward description |
| `{{monsterNames}}` | Reward pokemon names (if applicable) |
| `{{itemNames}}` | Reward item names (if applicable) |
| `{{dustAmount}}` | Stardust reward amount |
| `{{itemAmount}}` | Item reward quantity |
| `{{energyAmount}}` | Mega energy reward amount |
| `{{energyMonstersNames}}` | Pokemon for mega energy reward |
| `{{isShiny}}` | Whether the pokemon reward is shiny |
| `{{with_ar}}` | Whether AR scan is required before spinning |
| `{{{imgUrl}}}` | Reward image URL |
| `{{{stickerUrl}}}` | Reward sticker URL |

---

### Invasion (`invasion`)

| Variable | Description |
|----------|-------------|
| `{{{pokestopName}}}` | Pokestop name |
| `{{gruntType}}` | Invasion type (e.g. Rock, Dragon, Mixed) |
| `{{gruntTypeId}}` | Invasion type ID |
| `{{gruntTypeColor}}` | Colour for the grunt type |
| `{{gruntTypeEmoji}}` | Grunt type emoji |
| `{{gruntName}}` | Grunt name |
| `{{gruntRewards}}` | Formatted reward string |
| `{{gruntRewardsList}}` | Structured reward data (see below) |
| `{{genderData.name}}` | Grunt gender name |
| `{{genderData.emoji}}` | Grunt gender emoji |
| `{{gender}}` | Grunt gender ID |
| `{{displayTypeId}}` | Display type (<=6 = grunt, 7 = gold stop, 8 = Kecleon, 9 = showcase) |
| `{{time}}` | Invasion end time |
| `{{incidentExpiration}}` | Unix timestamp of expiration |
| `{{tthh}}` | Hours remaining |
| `{{tthm}}` | Minutes remaining |
| `{{tths}}` | Seconds remaining |
| `{{{imgUrl}}}` | Invasion image URL |
| `{{pokestopUrl}}` | Pokestop image URL |

**Reward list structure:**

```handlebars
{{#compare gruntRewardsList.first.chance '==' 100}}
  {{#forEach gruntRewardsList.first.monsters}}{{this.name}}{{#unless isLast}}, {{/unless}}{{/forEach}}
{{/compare}}
{{#compare gruntRewardsList.first.chance '<' 100}}
  {{gruntRewardsList.first.chance}}%: {{#forEach gruntRewardsList.first.monsters}}{{this.name}}{{#unless isLast}}, {{/unless}}{{/forEach}}
  {{gruntRewardsList.second.chance}}%: {{#forEach gruntRewardsList.second.monsters}}{{this.name}}{{#unless isLast}}, {{/unless}}{{/forEach}}
{{/compare}}
```

---

### Lure (`lure`)

| Variable | Description |
|----------|-------------|
| `{{{pokestopName}}}` | Pokestop name |
| `{{lureTypeName}}` | Lure type (localised) |
| `{{lureTypeNameEng}}` | Lure type (English) |
| `{{lureTypeId}}` | Lure type ID |
| `{{lureTypeEmoji}}` | Lure type emoji |
| `{{lureTypeColor}}` | Lure colour |
| `{{time}}` | Lure end time |
| `{{tthh}}` | Hours remaining |
| `{{tthm}}` | Minutes remaining |
| `{{tths}}` | Seconds remaining |
| `{{{imgUrl}}}` | Lure image URL |
| `{{pokestopUrl}}` | Pokestop image URL |

---

### Nest (`nest`)

| Variable | Description |
|----------|-------------|
| `{{{nestName}}}` | Nest name |
| `{{pokemonId}}` | Pokemon ID |
| `{{name}}` | Pokemon name (localised) |
| `{{nameEng}}` | Pokemon name (English) |
| `{{pokemonSpawnAvg}}` | Average spawns per hour |
| `{{pokemonCount}}` | Total pokemon count |
| `{{resetDate}}` | Date nest last reset |
| `{{disappearDate}}` | Predicted nest end date |
| `{{color}}` | Embed colour (type-based) |
| `{{emojiString}}` | Type emoji |
| `{{shinyPossibleEmoji}}` | Sparkle if shiny possible |
| `{{boostingWeathersEmoji}}` | Weather boost emojis |
| `{{{imgUrl}}}` | Pokemon image URL |

---

### Gym (`gym`)

| Variable | Description |
|----------|-------------|
| `{{{gymName}}}` | Gym name |
| `{{teamName}}` | Controlling team name |
| `{{color}}` | Team colour |
| `{{slotsAvailable}}` | Number of open slots |
| `{{inBattle}}` | Whether the gym is under attack |
| `{{{gymUrl}}}` | Gym image URL |

---

### Weather Change (`weatherchange`)

| Variable | Description |
|----------|-------------|
| `{{weatherName}}` | New weather name |
| `{{weatherEmoji}}` | New weather emoji |
| `{{oldWeatherName}}` | Previous weather name |
| `{{oldWeatherEmoji}}` | Previous weather emoji |
| `{{weatherCurrent}}` | Current weather ID |
| `{{weatherCurrentName}}` | Current weather name |
| `{{weatherCurrentEmoji}}` | Current weather emoji |
| `{{weatherNext}}` | Forecast weather ID |
| `{{weatherNextName}}` | Forecast weather name |
| `{{weatherNextEmoji}}` | Forecast weather emoji |
| `{{weatherChangeTime}}` | Change time (HH:00) |
| `{{activePokemons}}` | Array of affected tracked pokemon |

**Active pokemon iteration:**

```handlebars
{{#each activePokemons}}
**{{this.name}}** {{#isnt this.formName 'Normal'}} {{this.formName}}{{/isnt}} - {{round this.iv}}% - {{this.cp}}CP
{{/each}}
```

---

### Fort Update (`fort-update`)

| Variable | Description |
|----------|-------------|
| `{{{name}}}` | Fort name |
| `{{fortType}}` | `pokestop` or `gym` |
| `{{isNew}}` | Whether this is a newly added fort |
| `{{isRemoval}}` | Whether this fort was removed |
| `{{isEdit}}` | Whether this is an edit to an existing fort |
| `{{isEditName}}` | Name was changed |
| `{{isEditDescription}}` | Description was changed |
| `{{isEditLocation}}` | Location was changed |
| `{{isEditImgUrl}}` | Image was changed |
| `{{{newName}}}` | New name (if changed) |
| `{{{oldName}}}` | Previous name (if changed) |
| `{{{newDescription}}}` | New description |
| `{{{oldDescription}}}` | Previous description |
| `{{oldLatitude}}` | Previous latitude (if moved) |
| `{{oldLongitude}}` | Previous longitude (if moved) |
| `{{{newImageUrl}}}` | New image URL |
| `{{{oldImageUrl}}}` | Previous image URL |
| `{{description}}` | Fort description |
| `{{{imgUrl}}}` | Fort image URL |
| `{{areas}}` | Matched area names |

---

### Max Battle (`maxbattle`)

| Variable | Description |
|----------|-------------|
| `{{fullName}}` | Pokemon name |
| `{{levelName}}` | Battle level name |
| `{{{stationName}}}` | Power Spot station name |
| `{{quickMoveName}}` | Quick move name |
| `{{chargeMoveName}}` | Charge move name |
| `{{time}}` | Battle end time |
| `{{tthm}}` | Minutes remaining |
| `{{tths}}` | Seconds remaining |
| `{{color}}` | Embed colour |
| `{{{imgUrl}}}` | Pokemon image URL |

---

## Multiple template styles

You can define multiple templates for the same alert type by giving them different `id` values. Users select a template on their tracking commands with the `template` parameter:

```
!track everything iv100 templatehundo
!track everything iv0 templatenundo
```

Example -- a compact "hundo" template alongside the standard one:

```json
[
  {
    "id": "standard",
    "type": "monster",
    "default": true,
    "platform": "discord",
    "template": {
      "embed": {
        "title": "{{fullName}} {{genderData.emoji}} {{round iv}}% ({{atk}}/{{def}}/{{sta}})",
        "description": "CP {{cp}} | Lvl {{level}}\nDespawns: {{time}} ({{tthm}}m {{tths}}s)\n[Google]({{{googleMapUrl}}})",
        "thumbnail": { "url": "{{{imgUrl}}}" },
        "image": { "url": "{{{staticMap}}}" }
      }
    }
  },
  {
    "id": "hundo",
    "type": "monster",
    "default": false,
    "platform": "discord",
    "template": {
      "embed": {
        "color": "#FFD700",
        "title": "💯 HUNDO {{fullName}} 💯",
        "description": "CP {{cp}} | Lvl {{level}}\n{{time}} ({{tthm}}m left)\n[Map]({{{googleMapUrl}}})",
        "thumbnail": { "url": "{{{imgUrl}}}" }
      }
    }
  }
]
```

## Customising templates

### Tips

1. **Start from the defaults.** Copy `config/defaults/dts.json` to `config/dts.json` and modify from there.
2. **Use `@include` for long descriptions.** Place shared template fragments in `config/dts/` as `.txt` files.
3. **Test with the Discord embed visualiser.** The [Embed Visualizer](https://leovoel.github.io/embed-visualizer/) helps you preview how your embed will look.
4. **Use triple braces for URLs.** Always use `{{{url}}}` for any field containing a URL to avoid HTML encoding.
5. **Conditional blocks save space.** Use `{{#if}}` to only show fields when data exists (e.g. weather boost, PVP ranks).

### Example: minimal Telegram monster template

```json
{
  "id": "standard",
  "type": "monster",
  "language": "en",
  "default": true,
  "platform": "telegram",
  "template": {
    "content": "[\u200A]({{{staticMap}}}) **{{fullName}}** {{genderData.emoji}} {{round iv}}% ({{atk}}/{{def}}/{{sta}})\nCP {{cp}} | Lvl {{level}}\nDespawns: {{time}} ({{tthm}}m {{tths}}s)\n[Google]({{{googleMapUrl}}}) | [Apple]({{{appleMapUrl}}})",
    "parse_mode": "Markdown",
    "sticker": "{{{stickerUrl}}}",
    "location": true,
    "webpage_preview": true
  }
}
```

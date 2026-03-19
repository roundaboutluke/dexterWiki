# Multi-Language Support

Dexter supports multiple languages for both bot responses and Discord slash command localisation.

## Setting the Default Language

```json
{
  "general": {
    "locale": "en"
  }
}
```

Available built-in locales: `en`, `de`, `fr`, `ru`, `it`, `pl`, `nb-no`, `se`

## Allowing Users to Switch Language

Users can change their language with the `/language` slash command. Available languages are auto-discovered from locale files, or you can explicitly list them:

```json
{
  "general": {
    "availableLanguages": {
      "en": "English",
      "de": "Deutsch",
      "fr": "Français"
    }
  }
}
```

If `availableLanguages` is empty (`{}`), Dexter auto-discovers available languages from `locale/*.json` files.

## Locale Files

Locale files live in the `locale/` directory:

- `locale/en.json` — English translations
- `locale/de.json` — German translations
- `locale/fr.json` — French translations
- etc.

### Slash Command Localisation

Discord slash commands are localised separately in `locale/slash/` files:

- `locale/slash/de.json` — German slash command names and descriptions
- `locale/slash/fr.json` — French slash command names and descriptions
- etc.

These translate command names, option names, descriptions, and choice labels in the Discord UI.

## Custom Translations

You can override specific translation strings by creating a custom locale file:

```
config/custom.en.json
config/custom.de.json
```

Custom files are merged on top of the built-in translations, so you only need to include the keys you want to change.

## Secondary Language

A secondary language can be configured for "alt" translation fields in DTS templates:

```json
{
  "locale": {
    "language": "en"
  }
}
```

This allows templates to use both the user's language and a secondary language (e.g. showing Pokemon names in both the local language and English).

## Date & Time Formatting

```json
{
  "locale": {
    "timeformat": "en-gb",
    "time": "LTS",
    "date": "L"
  }
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `timeformat` | `"en-gb"` | Locale for date/time formatting |
| `time` | `"LTS"` | Time format string |
| `date` | `"L"` | Date format string |

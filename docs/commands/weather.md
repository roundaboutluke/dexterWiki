# /weather

Track weather conditions at your saved location or a specified place. Dexter will notify you when the weather changes to a condition you are tracking.

## Usage

```
/weather condition:Windy
```

## Options

| Option | Type | Description |
|--------|------|-------------|
| `condition` | String | Weather condition (e.g. Sunny, Rainy, Windy). Supports autocomplete. |
| `location` | String | Location name or coordinates. Leave blank to use your saved location. |
| `clean` | Flag | Auto-delete after the weather period expires |
| `template` | String | Named alert template |
| `profile` | String | Profile to add this filter to |

## Examples

Track windy weather at your saved location:

```
/weather condition:Windy
```

Track snow weather at a specific location:

```
/weather condition:Snow location:Central Park, New York
```

Track rainy weather with auto-cleanup:

```
/weather condition:Rainy clean:Enable
```

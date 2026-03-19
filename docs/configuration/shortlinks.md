# Shortlinks

Dexter can shorten URLs in alert messages using various URL shortening services.

## Configuration

```json
{
  "general": {
    "shortlinkProvider": "hideuri",
    "shortlinkProviderURL": "",
    "shortlinkProviderKey": "",
    "shortlinkProviderDomain": ""
  }
}
```

## Providers

### hideuri (default)

Free, no-registration URL shortener:

```json
{
  "general": {
    "shortlinkProvider": "hideuri"
  }
}
```

### Shlink

Self-hosted URL shortener:

```json
{
  "general": {
    "shortlinkProvider": "shlink",
    "shortlinkProviderURL": "https://your-shlink-instance.com",
    "shortlinkProviderKey": "YOUR_API_KEY"
  }
}
```

### YOURLS

Self-hosted URL shortener:

```json
{
  "general": {
    "shortlinkProvider": "yourls",
    "shortlinkProviderURL": "https://your-yourls-instance.com",
    "shortlinkProviderKey": "YOUR_API_KEY"
  }
}
```

## Map Frontend Links

Dexter can generate direct links to map frontends for alert locations:

```json
{
  "general": {
    "reactMapURL": "https://your-reactmap.com",
    "diademURL": "https://your-diadem.com",
    "rdmURL": "https://your-rdm.com",
    "rocketMadURL": "https://your-rocketmad.com"
  }
}
```

These are available in DTS templates as `{{{reactMapUrl}}}`, `{{{diademUrl}}}`, etc.

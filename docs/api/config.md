# Configuration API

## Get Configuration

Returns configuration values used by web interfaces.

```http
GET /api/config/poracleWeb
```

### Response

```json
{
  "status": "ok",
  "version": "1.0.0",
  "locale": "en",
  "providerURL": "http://10.0.0.1:7070",
  "staticKey": ["YOUR_KEY"],
  "pvpFilterMaxRank": 5000,
  "pvpFilterGreatMinCP": 1000,
  "pvpFilterUltraMinCP": 2000,
  "defaultTemplateName": 1,
  "admins": {
    "discord": ["USER_ID"],
    "telegram": ["USER_ID"]
  },
  "maxDistance": 0,
  "everythingFlagPermissions": "allow-any"
}
```

# Scanner Setup

Dexter receives Pokemon GO data via webhooks from your scanner. Configure your scanner to send webhooks to Dexter's endpoint.

## Webhook Endpoint

By default, Dexter listens at:

```
http://127.0.0.1:3030
```

If Dexter is on a different machine, replace `127.0.0.1` with the appropriate IP address.

## Golbat

Golbat is the recommended scanner. In your Golbat config, add Dexter as a webhook destination:

```toml
[[webhooks]]
url = "http://127.0.0.1:3030"
types = ["pokemon", "pokemon_iv", "raid", "quest", "invasion", "weather", "gym", "gym_details", "pokestop", "nest", "fort"]
```

## RDM

In the RDM dashboard under **Settings → Webhooks**:

1. Click **Add New Webhook**
2. Set the URL to `http://127.0.0.1:3030`
3. Enable the event types you want to track
4. Save

## MAD

In your MAD `config.ini`:

```ini
webhook
webhook_url: http://127.0.0.1:3030
quest_webhook_flavor: poracle
```

!!! note "Quest format"
    MAD requires `quest_webhook_flavor: poracle` for quest webhooks to work correctly. This format is also compatible with other tools like Pokealarm.

## Scanner Database (optional)

If you want Dexter to look up pokestop names from your scanner's database, configure the scanner database connection in `local.json`:

```json
{
  "database": {
    "scannerType": "golbat",
    "scanner": {
      "host": "127.0.0.1",
      "database": "golbat",
      "user": "readonlyuser",
      "password": "password",
      "port": 3306
    }
  }
}
```

## Verifying Webhooks

After starting both your scanner and Dexter, check Dexter's logs. You should see incoming webhook events being processed. If not:

- Verify the URL and port are correct
- Check firewall rules between the scanner and Dexter
- Ensure the scanner is actively finding Pokemon/raids/etc.

# Raid RSVP

Dexter supports in-place raid post updates with RSVP functionality. When a raid hatches or RSVP details change, the existing alert message is **edited** rather than sending a new message.

## How It Works

1. An egg alert is sent when a raid egg appears
2. When the raid hatches (boss revealed), the same message is **updated** with the boss details
3. If RSVP data is available, the message continues to update with participant counts

This keeps channels clean — one message per raid instead of separate egg + boss + RSVP posts.

## Configuration

RSVP support is enabled through the raid tracking options. Users can enable RSVP updates when setting up raid tracking:

```
/raid boss pokemon:Mewtwo rsvp:true
/raid level level:5 rsvp:true
/raid egg level:5 rsvp:true
```

## DTS Templates

Raid templates can include RSVP-specific fields. See [Alert Templates](../configuration/alert-templates.md) for the available template variables for raids.

## Requirements

- RSVP data must be provided by your webhook source (scanner)
- The bot needs **Manage Messages** permission in Discord to edit its own messages
- Works with both DM and channel alerts

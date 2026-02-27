---
name: bark
description: Send Bark (day.app) push notifications via HTTP. Use when asked to push a short message/alert to iPhone/iPad via Bark/day.app, including scheduled digests and quick “push to phone” actions.
---

# Bark (day.app)

Send a push notification via a Bark endpoint (day.app).

## Setup

- Store your Bark base URL (with trailing slash) in:
  - `~/.openclaw-secrets/bark.url` with a single line: `BARK_URL=https://api.day.app/<key>/`
  - file permissions should be `600`.

## Send a push

Prefer POST (avoids URL length limits):

```bash
~/bin/bark-send --title "Title" --body "Message" --group "openclaw"
```

## Notes

- Keep button text short; put details in the message body.
- Avoid sending secrets/tokens.

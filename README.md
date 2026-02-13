# Support Inbox — Slack App Manifest

Slack app manifest for deploying the [Support Inbox](https://github.com/hillside-gitacc/slack-support-inbox) app.

## What's Included

`manifest.json` defines the full Slack app configuration:

- **Bot scopes:** `channels:history`, `channels:read`, `channels:join`, `chat:write`, `reactions:read`, `reactions:write`, `users:read`
- **Event subscriptions:** `app_home_opened`, `message.channels`, `reaction_added`, `reaction_removed`
- **Features:** App Home (Home Tab), Socket Mode, Interactivity
- **Bot user:** "Support Inbox" (always online)

## Deployment

### Option 1: Create from manifest via UI

1. Go to [api.slack.com/apps](https://api.slack.com/apps)
2. Click **Create New App** → **From a manifest**
3. Select your workspace
4. Paste the contents of `manifest.json`
5. Review and click **Create**

### Option 2: Create from manifest via API

```bash
# Export your app configuration token
export SLACK_CONFIG_TOKEN=xoxe-...

# Create the app
curl -X POST https://slack.com/api/apps.manifest.create \
  -H "Authorization: Bearer $SLACK_CONFIG_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"manifest\": $(cat manifest.json | jq -Rs .)}"
```

### Option 3: Update an existing app

```bash
export SLACK_CONFIG_TOKEN=xoxe-...
export APP_ID=A0123456789

curl -X POST https://slack.com/api/apps.manifest.update \
  -H "Authorization: Bearer $SLACK_CONFIG_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"app_id\": \"$APP_ID\", \"manifest\": $(cat manifest.json | jq -Rs .)}"
```

## Post-Deployment Steps

After creating the app from the manifest:

1. **Generate an App-Level Token** — Go to Settings → Basic Information → App-Level Tokens → Generate Token. Add the `connections:write` scope. This gives you the `xapp-...` token.
2. **Install to workspace** — Go to Settings → Install App → Install to Workspace. This gives you the `xoxb-...` bot token.
3. **Configure the app** — Set `SLACK_BOT_TOKEN` and `SLACK_APP_TOKEN` in the Support Inbox app's `.env` file.

## Validating the Manifest

```bash
export SLACK_CONFIG_TOKEN=xoxe-...

curl -X POST https://slack.com/api/apps.manifest.validate \
  -H "Authorization: Bearer $SLACK_CONFIG_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"manifest\": $(cat manifest.json | jq -Rs .)}"
```

A successful response returns `{"ok": true}`.

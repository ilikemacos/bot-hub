# Bot Hub

A single-page dashboard for sending messages to my Grok Bots (dr eggbot and Nr. Wyne) from the web.

Live: https://ilikemacos.github.io/bot-hub/

## How it works

The page POSTs directly from the browser to each bot's Grok Bot webhook:

- Body: `{"id": "<uuid>", "text": "<message>", "action": "chat"}`
- Headers: `Content-Type: application/json`, `Authorization: Bearer <token>`, `X-Automation-Key: <key>`

There's no relay or server. Replies show up in each bot's chat in the Grok Bot app.

## Setup

1. Open the page and pick a bot tab.
2. Open **Settings** and enter the webhook URL, the bearer token, and the webhook key.
3. Click **Save**. These values are stored only in your browser's localStorage. Nothing secret is in this repo.

If sending fails with a "browser blocked the request" message, the webhook endpoint isn't allowing cross-origin (CORS) requests from this page.

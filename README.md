# Bot Hub

One web page for messaging my Grok Bots: dr eggbot, Nr. Wyne and Nr. Wrenby.

Live: https://ilikemacos.github.io/bot-hub/

## How it works

The page is static (GitHub Pages). There's no relay or server. The browser POSTs straight to each bot's Grok Bot webhook:

- Headers: `Content-Type: application/json` and `Authorization: Bearer <key>`
- Body: `{"id": "<uuid>", "text": "<message>", "action": "chat"}`

Replies arrive in each bot's chat in the Grok Bot app, not on this page.

## Setup

1. Open the page and pick a bot.
2. The first time, paste that bot's webhook key into the inline field (with or without `Bearer `). Use **change key** to replace it.

Keys and message history are stored only in your browser's localStorage. No keys are in this repo. The webhook URLs in the page aren't secret on their own.

## Status messages

- **Sent ✓**: the webhook accepted the message.
- **Wrong key (401/403)**: re-enter the key.
- **Couldn't confirm**: a network or CORS error hid the response. The message may still have gone through.

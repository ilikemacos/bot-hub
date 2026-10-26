# Bot Hub

One web chat for my Grok Bots: dr eggbot, Nr. Wyne and Nr. Wrenby.

Live: https://ilikemacos.github.io/bot-hub/

## How it works

- This page is static (GitHub Pages). It holds no secrets.
- It talks to a small relay (`relay.js`) on my box. The relay is published only on my tailnet with `tailscale serve`.
- The relay holds each bot's webhook URL and key and forwards messages to the bot's Grok Bot webhook. Each bot's routine writes replies back to the relay, and the page polls for them every 3 seconds.

Relay endpoints (no token needed; the relay is reachable only on my tailnet):

| Method | Path | Purpose |
|---|---|---|
| GET | `/health` | Status and bot list |
| POST | `/send` | `{"bot": "<slug>", "text": "..."}` |
| GET | `/replies?bot=<slug>&since=<ms>` | Replies newer than `since` |

Bot slugs: `dr-eggbot`, `nr-wyne`, `nr-wrenby`.

## Relay setup (on the box)

1. Run the relay: `node relay.js` (listens on port 8787). Webhook URLs live in `config.json`, which is never committed. Keys come from environment variables.
2. Publish it on your tailnet: `sudo tailscale serve --bg --https=443 http://127.0.0.1:8787`
3. Check it with `tailscale serve status` and `curl https://<host>.<tailnet>.ts.net/health`.

## Using the page

1. Connect your device to Tailscale.
2. Open https://ilikemacos.github.io/bot-hub/ and pick a bot. No setup needed.

The page uses the default relay `https://bot-hub.tail952f67.ts.net` (tailnet-only). The small **advanced** link in the sidebar lets you override the relay URL or add an optional access token, saved only in this browser's localStorage. Chat history is stored there too.

If the page says it can't reach the relay, check that this device is on Tailscale and the relay is running.

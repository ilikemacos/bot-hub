# Bot Hub

One web chat for my Grok Bots: dr eggbot, Nr. Wyne and Nr. Wrenby.

Live: https://ilikemacos.github.io/bot-hub/

## How it works

- This page is static (GitHub Pages). It holds no secrets.
- It talks to a small relay (`relay.js`) on my box. The relay is published only on my tailnet with `tailscale serve`.
- The relay holds each bot's webhook URL and key and forwards messages to the bot's Grok Bot webhook. Each bot's routine writes replies back to the relay, and the page polls for them every 3 seconds.

Relay endpoints (all except `/health` need the `X-Access-Token` header):

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
2. Open the page and click **Settings**. Enter the relay URL (`https://<host>.<tailnet>.ts.net`) and the access token.
3. These values are saved only in this browser's localStorage. Chat history is stored there too.

If the page says it can't reach the relay, check that this device is on Tailscale and the relay is running.

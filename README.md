# Invidious on Railway

One-click deploy of [Invidious](https://github.com/iv-org/invidious) — a privacy-respecting, open-source front-end for YouTube — on [Railway](https://railway.com). Watch and search YouTube without ads, tracking, JavaScript, or a Google account.

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/invidious?utm_medium=integration&utm_source=button&utm_campaign=invidious)

Live template: <https://railway.com/deploy/invidious>

## What gets deployed

| Service | Image | Role |
|---|---|---|
| **Invidious** | `quay.io/invidious/invidious:latest` | Web front-end on a public HTTPS domain (port 3000) |
| **Companion** | `quay.io/invidious/invidious-companion:latest` | Internal-only stream extractor (port 8282) |
| **Postgres** | `postgres:16-alpine` | Stores accounts, subscriptions, watch history, playlists. 5 GB volume. |

All three services run on Railway's private IPv6 network. Only Invidious is publicly exposed. Shared secrets (`HMAC_KEY`, `INVIDIOUS_COMPANION_KEY`, `POSTGRES_PASSWORD`) are auto-generated per deploy via Railway's `${{secret(N)}}` template functions; cross-service references are resolved at deploy time.

## Videos won't play after deploy?

Most common issue and not a bug in the template. YouTube blocks Railway's datacenter IPs from validating proof-of-origin (PO) tokens. Symptom: homepage works, but watch pages show *"Companion is starting. Please wait until a valid potoken is found."*

Quickest fix: add a `YOUTUBE_SESSION_COOKIES` env var to the **Companion** service, using cookies exported from a logged-in burner Google account. Three rules from yt-dlp's [cookie export guide](https://github.com/yt-dlp/yt-dlp/wiki/Extractors#exporting-youtube-cookies):

1. Log in to YouTube in a private/incognito window
2. **Log out before closing the window** — YouTube rotates cookies on session end and yours stop working otherwise
3. Use a dedicated burner Google account; this approach can get accounts banned

Format the cookies as a single header string (`SID=...; HSID=...; SSID=...; APISID=...; SAPISID=...; LOGIN_INFO=...`) and paste into `YOUTUBE_SESSION_COOKIES`. Redeploy Companion. Watch pages should work within a minute or two.

If you have a residential proxy, an alternative is `PROXY=http://user:pass@host:port` on Companion. Datacenter proxies will be blocked the same way Railway's IPs are.

Full background: [Invidious YouTube errors explained](https://docs.invidious.io/youtube-errors-explained/).

## Customizing

Edit `INVIDIOUS_CONFIG` on the Invidious service. It's a YAML document — see the [Invidious config reference](https://docs.invidious.io/configuration/). Don't edit the `db:`, `hmac_key:`, or `invidious_companion:` blocks — they're populated by `${{...}}` references that resolve at deploy.

To pin image versions instead of `:latest`, change the source image tag on each service (e.g. `quay.io/invidious/invidious:2026.04.01`) and redeploy.

## Where to file issues

| Issue | Where |
|---|---|
| Template wiring — Railway-specific config, references, secrets, healthcheck | This repo |
| Invidious bugs — UI, search, accounts, settings | [iv-org/invidious](https://github.com/iv-org/invidious/issues) |
| Companion bugs — video playback, PO tokens | [iv-org/invidious-companion](https://github.com/iv-org/invidious-companion/issues) |
| Railway platform | [Railway support](https://railway.com/help) |

This template is just the deployment recipe. Application bugs go upstream.

## License

Template wiring in this repo: MIT. Invidious and invidious-companion have their own licenses — see upstream projects.

## Credits

- [Invidious](https://github.com/iv-org/invidious) and [invidious-companion](https://github.com/iv-org/invidious-companion) by iv-org
- [yt-dlp cookie export guide](https://github.com/yt-dlp/yt-dlp/wiki/Extractors#exporting-youtube-cookies) — operational reference for safe cookie exports

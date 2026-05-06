---
name: Template issue
about: Report a problem with how the Invidious Railway template is wired (services, variables, healthchecks, references)
labels: ["template"]
---

> **Before filing here, please confirm:**
>
> - [ ] This is not an Invidious application bug (UI, search, accounts, settings) — those go to [iv-org/invidious](https://github.com/iv-org/invidious/issues)
> - [ ] This is not a video-playback / PO-token issue — those usually fix with `YOUTUBE_SESSION_COOKIES` or a `PROXY` (see README) or are upstream bugs in [iv-org/invidious-companion](https://github.com/iv-org/invidious-companion/issues)
> - [ ] This is not a Railway platform issue (build failures, network blips, billing) — those go to [Railway support](https://railway.com/help)
>
> If your issue *is* one of the above, please file it upstream — this repo only covers the template wiring (service definitions, variables, healthcheck path, cross-service references).

## What went wrong

<!-- A clear description of the problem. -->

## What you expected

<!-- What should have happened instead. -->

## Reproduction

1. Deployed the template at `https://railway.com/deploy/invidious` on YYYY-MM-DD
2. ...
3. Observed ...

## Diagnostics

- **Service that failed**: Invidious / Companion / Postgres
- **Deployment status** (from the Railway dashboard): SUCCESS / FAILED / CRASHED
- **Relevant log excerpt** (last ~30 lines from the failing service):

```
paste logs here
```

- **Variables affected** (names only, never paste values):

## Anything else

<!-- Optional: screenshots, related upstream issues, what you've tried. -->

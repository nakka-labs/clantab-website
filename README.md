# clantab-website

Marketing + App Store support site for **ClanTab**, deployed to
`clantab.nakka.dev`.

Plain static HTML/CSS, no build step, no JS, no backend, no forms —
matches the `nakka-dev-website` convention. Deploy target: Cloudflare
Pages (root directory = repo root, no build command).

## Pages

| Path | Purpose |
|---|---|
| `/` | Home — hero, differentiators, features, how-it-works, screenshots |
| `/support.html` | Support + FAQ (required by App Store review) |
| `/privacy.html` | Privacy Policy (required by App Store review) |
| `/terms.html` | Terms of Service |
| `/contact.html` | Contact |

## Content sources

`support.html` and `privacy.html` here are adapted from
`clantab-ios/docs/support.html` and `clantab-ios/docs/privacy-policy.md`.
**Those files in `clantab-ios` stay the source of truth** — when either
changes there, re-sync the corresponding page here by hand. This isn't
automated yet; see `WEBSITE_PLAN.md`.

Screenshots (`/screenshots/`) and the wordmark/icon (`/brand/`) are
copied from `clantab-ios/docs/screenshots/` and `docs/branding/` —
re-copy if those are refreshed.

## Local preview

No build step — open `index.html` directly in a browser, or:

```
python3 -m http.server 8080
```

See `WEBSITE_PLAN.md` for what's left before this replaces the current
GitHub Pages support/privacy URLs in App Store Connect.

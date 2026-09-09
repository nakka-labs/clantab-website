# clantab-website — plan & handoff checklist

Scope locked 2026-09-09: new repo (this one), full v1 (Home, Support,
Privacy, Terms, Contact), built from real app content — not
placeholders. Supersedes the generic template in `nakka-dev-website`'s
`clantab-website-plan.md` (kept there as the original brainstorm).

## Done

- [x] Repo scaffolded at `~/Dev/clantab-website`, git-initialized.
- [x] Home page — hero, 3 real differentiators (no ads/tracking, no
      payment processing, exact-to-the-paisa), 4 feature cards, 4-step
      how-it-works, 5-screenshot gallery, honest platform-availability
      line (no fake "available now" — app isn't submitted yet per
      `clantab-ios/CHECKLIST.md`).
- [x] Support page — ported verbatim from `clantab-ios/docs/support.html`
      (FAQ, contact, GitHub issues link), re-skinned into this site's
      shared nav/footer.
- [x] Privacy page — ported verbatim from
      `clantab-ios/docs/privacy-policy.md` (dated 2026-09-08).
- [x] Terms of Service — **new, drafted from scratch** for this task:
      no payment processing, Apple/Google-only accounts, shared
      link/code access model, UGC moderation, liability limits,
      governing law India. Not legally reviewed — fine to ship for a
      free, no-revenue, no-payments app, but flagging that explicitly.
- [x] Contact page.
- [x] Real assets: 5 screenshots + wordmark (light/dark) copied from
      `clantab-ios/docs/`; app icon copied from
      `App/ClanTab/Assets.xcassets/AppIcon.appiconset/icon-1024.png`
      and resized into `favicon.ico` / `apple-touch-icon.png` /
      `brand/favicon-32.png` via ImageMagick.
- [x] `robots.txt`, `sitemap.xml`, `_headers` (same strict CSP as
      `nakka-dev-website`, no external scripts, no fonts), `404.html`.
- [x] Nakka-Labs footer credit (reuses `nakka-dev-website`'s wordmark
      asset) for cross-site consistency per `DESIGN_BIBLE.md` §9.

## Explicitly deferred / known gaps

- [ ] **App Store badge.** Home currently shows a "Coming soon to the
      App Store" pending badge, not a real download badge/link — the
      app isn't submitted yet (`clantab-ios/CHECKLIST.md` "Ship-blocking
      — App Store submission track" still has open items). Swap in the
      real Apple badge + App Store URL once it's live. Don't ship a
      fake badge before then — the original plan doc calls this out as
      "instant rejection territory if screenshotted."
- [ ] **OG image.** Using the square `brand/icon-1024.png` as a
      placeholder `og:image` — works, but a proper 1200×630 `og.png`
      (same idea as `nakka-dev-website/og.png`) would look better in
      link previews. Not blocking.
- [ ] **Content sync is manual.** `support.html` / `privacy.html` here
      are copies, not generated from `clantab-ios/docs/`. If the
      privacy policy or FAQ changes there, it has to be hand-ported
      here too. Worth automating (a shared source + build step) if this
      drifts more than once or twice — not worth the complexity yet for
      a single marketing site.
- [ ] **No `gh` or `wrangler` CLI available on this machine** (checked
      2026-09-09) — the three steps below need the dashboard.

## Manual steps to actually go live

1. [ ] **Create the GitHub repo.** `nakka-labs/clantab-website`, public
   (matches the rest of the portfolio — nothing here is sensitive).
   Push this local repo to it. (Not yet confirmed done as of 2026-09-09
   — the Cloudflare project below exists, confirm the repo push too.)
2. [x] **Cloudflare project connected + deployed.** Done 2026-09-09.
   Note: Cloudflare has unified Pages into Workers — connecting a repo
   here creates a **Worker** (Workers Static Assets), not a classic
   "Pages project". Same result (static file hosting), different
   dashboard: Workers & Pages → `clantab-website` → tabs are
   Overview/Metrics/Deployments/Bindings/Observability/**Domains**/Access/Settings.
3. [x] **Custom domain — live.** Done 2026-09-09 at
   `clantab-website` → **Domains** tab → **+ Add Domain** → select the
   `nakka.dev` zone → **Subdomain** field.
   **Root cause of the "nothing happens" failure**: that Subdomain
   field only wants the label (`clantab`) — it already appends
   `.nakka.dev` after you pick the zone. Typing the full
   `clantab.nakka.dev` there builds an invalid double-suffixed
   hostname and the dialog fails silently on Add/Continue, no error
   shown. Typing just `clantab` worked immediately — DNS + SSL
   provisioned in under a minute, verified live at
   `https://clantab.nakka.dev/`.
4. **Once live, update `clantab-ios/docs/appstore/metadata.md`:**
   - Support URL → `https://clantab.nakka.dev/support.html`
   - Privacy Policy URL → `https://clantab.nakka.dev/privacy.html`
   (both currently point at `nakka-labs.github.io/clantab-ios/...` —
   fine to leave until this site is actually live, but must change
   before App Store submission since those are the URLs App Review
   will open).
5. **Decide the fate of the GitHub Pages site**
   (`nakka-labs.github.io/clantab-ios`) — retire it, or leave it as a
   fallback/redirect. Not urgent.

## Universal Links for the iOS app (added 2026-09-09)

`clantab-ios` share links are `https://clantab.nakka.dev/g/<id>?token=<t>`.
For a device with the app installed to open them directly:

- [x] **AASA file** — `/.well-known/apple-app-site-association` (+ a root
      copy and a `_redirects` 200-rewrite fallback in case Workers Static
      Assets doesn't serve the dot-path), `Content-Type: application/json`
      via `_headers`. Scoped `applinks: /g/*` so only invite links open the
      app; every marketing page stays web. App ID `UK652GNPP7.com.clantab.app`.
- [ ] **Worker Route** `clantab.nakka.dev/g/*` → the `clantab` API Worker
      (dashboard: Workers & Pages → `clantab` → Settings → Domains & Routes →
      Add Route), so a recipient *without* the app lands on the Worker's
      "Open in ClanTab" fallback page rather than this site's 404. If a Route
      can't coexist with this site's Custom Domain on the same hostname,
      fall back to a static `g/index.html` + `_redirects` here instead.
- [ ] Owner: enable the **Associated Domains** capability on the
      `com.clantab.app` App ID (Apple Developer portal), then a TestFlight
      build + a real-device tap test. Tracked in `clantab-ios/CHECKLIST.md`.

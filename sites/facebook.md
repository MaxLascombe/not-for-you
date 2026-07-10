# Facebook — block values

The site-specific values to plug into the method in the [main README](../README.md). Each heading maps to a Part there.

## What to block vs keep

- **iPhone (Tier 1):** `facebook.com` fully blocked.
  ⚠️ **Messenger is a separate domain** (`messenger.com`) and a separate app. Blocking Facebook does **not** block Messenger. Decide: if you want the phone fully dark, add `messenger.com` to Part 2 as well; if Messenger is how people reach you, leave it out.
- **Mac (Tier 2):** kill the **News Feed, Reels, Watch, and Stories**; keep **Messenger**, and optionally **Marketplace** and **Events**.
  Like Instagram (and unlike LinkedIn), Facebook's feed lives at the site **root** (`facebook.com/`), so it can't be path-blocked without taking everything with it — it needs **cosmetic hiding**.

## Part 2 — iPhone website block (NEVER ALLOW list)

Add both:
- `https://www.facebook.com`
- `https://facebook.com`

Optionally, to kill Messenger on the phone too:
- `https://www.messenger.com`
- `https://messenger.com`

## Part 3 — iPhone app block

- Category: **Social** — the same category as Instagram. **If you already added a Social App Limit for Instagram, Facebook is already covered.** No new limit needed.
- ⚠️ The Social category also catches the **Messenger** app. If you want to keep Messenger on your phone, you'll need an individual app limit for Facebook instead (needs the app installed to pick it, and resets on reinstall — so lean on Parts 1–2).

## Part 4 — Mac feed-killer

**Cosmetic filters** (AdGuard / uBO user rules) — hide the News Feed, Stories, and the Reels/Watch nav:
```
www.facebook.com##div[role="feed"]
www.facebook.com##a[href^="/watch"]
www.facebook.com##a[href^="/reel"]
```
_`div[role="feed"]` is the important one. Facebook's CSS class names are randomly generated and change constantly, but it tags the News Feed with the ARIA `role="feed"` attribute for screen readers — which makes this selector far more durable than a class-based one._

**Path blocks** (LeechBlock NG) — kill the Reels/Watch/Stories players:
```
*facebook.com/watch*
*facebook.com/reel*
*facebook.com/stories*
```

**Keep reachable** (do NOT block): `/messages`, `/marketplace`, `/events`, and the whole `messenger.com` domain.

## Part 5 — whole-domain values (for browsers you DON'T use for Facebook)

- **URLBlocklist:** `facebook.com`, `*://*.facebook.com/*` (add `messenger.com`, `*://*.messenger.com/*` only if you're killing Messenger too)
- **Safari Restricted / iPhone NEVER ALLOW:** `https://www.facebook.com`, `https://facebook.com`
- **NextDNS / hosts (nuclear):** `facebook.com`, `www.facebook.com`, `m.facebook.com`, `connect.facebook.net`
  ⚠️ **Never** `fbcdn.net` — shared Meta infra; breaks Messenger and WhatsApp.
  ⚠️ Blocking `connect.facebook.net` also breaks **"Log in with Facebook"** on third-party sites. Drop it from the list if you rely on that anywhere.
  ⚠️ A whole-domain block on `facebook.com` can break `messenger.com`, which leans on Facebook endpoints for auth. If Messenger matters, use the Part 4 feed-killer instead of a domain block.

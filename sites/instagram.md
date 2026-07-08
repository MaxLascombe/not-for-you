# Instagram — block values

The site-specific values to plug into the method in the [main README](../README.md). Each heading maps to a Part there.

## What to block vs keep

- **iPhone (Tier 1):** everything — Instagram is fully dark.
- **Mac (Tier 2):** kill the **feed, Reels, and Explore**; keep **DMs** (`/direct`).
  Instagram's feed lives at the site **root** (`instagram.com/`), so it can't be path-blocked without taking DMs with it — it needs **cosmetic hiding**.

## Part 2 — iPhone website block (NEVER ALLOW list)

Add both:
- `https://www.instagram.com`
- `https://instagram.com`

## Part 3 — iPhone app block

- Category: **Social**. Instagram sits under Social in Screen Time's App Limits, so the category limit catches it even with the app uninstalled and survives a reinstall.

## Part 4 — Mac feed-killer

**Cosmetic filters** (AdGuard / uBO user rules) — hide the home feed and the Explore/Reels nav:
```
www.instagram.com##main[role="main"] > div:has(article)
www.instagram.com##a[href="/explore/"]
www.instagram.com##a[href^="/reels/"]
```

**Path blocks** (LeechBlock NG) — kill the full-screen Reels/Explore players:
```
instagram.com/reels
instagram.com/explore
instagram.com/reels/*
instagram.com/explore/*
```
Leave `instagram.com/direct` **out** — that's DMs.

_Instagram reshuffles its markup often. If the feed reappears, right-click it → Inspect and update the `article` selector above._

## Part 5 — whole-domain values (for browsers you DON'T use for Instagram)

- **URLBlocklist:** `instagram.com`, `*://*.instagram.com/*`
- **Safari Restricted / iPhone NEVER ALLOW:** `https://www.instagram.com`, `https://instagram.com`
- **NextDNS / hosts (nuclear):** `instagram.com`, `www.instagram.com`, `i.instagram.com`, `api.instagram.com`, `graph.instagram.com`, `platform.instagram.com`, `cdninstagram.com` — **never** `fbcdn.net` (shared Meta infra; breaks Messenger/WhatsApp).

# Bluesky — block values

The site-specific values to plug into the method in the [main README](../README.md). Each heading maps to a Part there.

## What to block vs keep

- **iPhone (Tier 1):** everything — Bluesky is fully dark.
- **Mac (Tier 2):** kill the **Following / Discover feeds and the Feeds directory**; keep **DMs** (`/messages`), notifications, profiles, and search.
  Like Instagram, Bluesky's feed lives at the site **root** (`bsky.app/`), so it can't be path-blocked without taking DMs with it — it needs **cosmetic hiding**.

## Part 2 — iPhone website block (NEVER ALLOW list)

Add:
- `https://bsky.app`

_There's no `www.` variant — `bsky.app` is the canonical domain, so one entry covers it._

## Part 3 — iPhone app block

- Category: **Social** — same category as Instagram. **If you already added a Social App Limit for Instagram, Bluesky is already covered.** Delete the app anyway; the Part 2 website block catches the browser route.

## Part 4 — Mac feed-killer

**Cosmetic filters** (AdGuard / uBO user rules) — hide the home feeds (Following, Discover, and any pinned custom feed) and the feed-tab bar:
```
bsky.app##div[data-testid$="FeedPage"]
bsky.app##div[data-testid="homeScreenFeedTabs"]
bsky.app##a[href="/feeds"]
```
_Bluesky is built with React Native Web, so its CSS class names are machine-generated and useless for filtering — but it tags UI regions with `data-testid` attributes for its own test suite (`followingFeedPage`, `customFeedPage`, …), which the `$=` ends-with selector catches in one rule. Test IDs are more durable than class names, but if the feed reappears, right-click it → Inspect and grab the new `data-testid`._

**Path blocks** (LeechBlock NG) — kill the Feeds directory and custom-feed pages:
```
bsky.app/feeds*
bsky.app/profile/*/feed/*
```
Set LeechBlock's "redirect to" for this block set to `https://bsky.app/messages` — so opening Bluesky drops you into your DMs, not the scroll.

**Keep reachable** (do NOT block): `/messages` (DMs), `/notifications`, `/profile/` (profiles), `/search`.

## Part 5 — whole-domain values (for browsers you DON'T use for Bluesky)

- **URLBlocklist:** `bsky.app`, `*://*.bsky.app/*`
- **Safari Restricted / iPhone NEVER ALLOW:** `https://bsky.app`
- **NextDNS / hosts (nuclear):** `bsky.app`, `public.api.bsky.app`, `cdn.bsky.app`, `video.bsky.app`, `bsky.social`
  ⚠️ Blocking only `bsky.app` still leaves **third-party clients** (deck.blue, Graysky, etc.) working — they talk to the API directly. Adding `public.api.bsky.app` and `bsky.social` (the default account host) closes that door too.
  ⚠️ If your account lives on a **custom PDS** or you log in with a custom-domain handle, `bsky.social` isn't your host — the block list still works for the official app/site, but third-party clients would need your PDS domain added instead.

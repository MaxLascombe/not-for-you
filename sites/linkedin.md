# LinkedIn — block values

The site-specific values to plug into the method in the [main README](../README.md). Each heading maps to a Part there.

## What to block vs keep

- **iPhone (Tier 1):** everything — LinkedIn is fully blocked.
- **Mac (Tier 2):** kill the **home feed**; keep **Messaging, Jobs, your network, and profiles**.
  Unlike Instagram, LinkedIn's feed lives at its own path — `linkedin.com` redirects to `/feed/` — so you can **block it by path**, which is more robust than cosmetic hiding (no selector to babysit).

## Part 2 — iPhone website block (NEVER ALLOW list)

Add both:
- `https://www.linkedin.com`
- `https://linkedin.com`

## Part 3 — iPhone app block

- LinkedIn is usually classified under **Business**, *not* Social — so the Social category limit from the Instagram setup will **not** catch it.
- When you add the App Limit, check which category LinkedIn actually appears under in the picker and tick that one. If you'd rather not limit *all* Business apps, add an **individual limit for LinkedIn** instead.
  ⚠️ _An individual-app limit needs the app installed to pick it and resets if you reinstall — so pair it with deleting the app and the Part 2 website block, which do the real work._

## Part 4 — Mac feed-killer

The scroll is the home feed at `/feed/` (the site root redirects there). Block the path and bookmark the parts you keep.

**Path blocks** (LeechBlock NG) — kill the feed:
```
linkedin.com/feed
linkedin.com/feed/*
```
Set LeechBlock's "redirect to" for this block set to `https://www.linkedin.com/messaging/` — so opening LinkedIn drops you into your messages, not the feed.

**Keep reachable** (do NOT block): `/messaging/`, `/jobs/`, `/mynetwork/`, `/in/` (profiles), `/notifications/`, `/search/`.

**Cosmetic backstop** (AdGuard / uBO) — in case you land on the feed some other way, hide the scroll and the "News / Add to feed" right rail:
```
www.linkedin.com##.scaffold-finite-scroll
www.linkedin.com##div.feed-shared-update-v2
```
_LinkedIn renames its CSS classes periodically. If the feed reappears, right-click it → Inspect and update these selectors._

## Part 5 — whole-domain values (for browsers you DON'T use for LinkedIn)

- **URLBlocklist:** `linkedin.com`, `*://*.linkedin.com/*`
- **Safari Restricted / iPhone NEVER ALLOW:** `https://www.linkedin.com`, `https://linkedin.com`
- **NextDNS / hosts (nuclear):** `linkedin.com`, `www.linkedin.com`, `licdn.com`

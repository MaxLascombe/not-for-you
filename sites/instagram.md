# Instagram — block values

The site-specific values to plug into the method in the [main README](../README.md). Each heading maps to a Part there.

## What to block vs keep

- **iPhone (Tier 1):** everything — Instagram is fully dark.
- **Mac (Tier 2):** kill the **feed, Reels, and the Explore grid**; keep **DMs** (`/direct`) **and search**.
  Instagram's feed lives at the site **root** (`instagram.com/`), so it can't be path-blocked without taking DMs with it — it needs **cosmetic hiding**.

**On keeping search:** yes, and the split is cleaner than it looks. Instagram bundles two different things under one icon — **search** (you type a name, you get that person) and **Explore** (a grid of posts from strangers, picked by the algorithm). They're separate surfaces: Explore is a page you navigate to at `/explore/`, while search is an **overlay panel** that opens over whatever page you're already on and doesn't navigate anywhere until you click a result. So blocking the `/explore/` page leaves the search panel fully working — you can open Instagram on your DMs, hit search, find an account, and go straight to their profile, never touching the grid.

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

_The `a[href="/explore/"]` rule hides the **Explore** link in the nav. The **Search** item above it is a separate button that opens the search panel — it isn't an `/explore/` link, so it survives untouched. That's the whole trick: one nav entry goes, the other stays._

**Path blocks** (LeechBlock NG) — kill the Reels player and the Explore grid, while letting search results through:
```
instagram.com/reels
instagram.com/explore
+instagram.com/explore/search
+instagram.com/explore/tags
+instagram.com/explore/locations
```
Set this block set's **"Redirect to this URL instead"** to `https://www.instagram.com/direct/inbox/` — so a stray Explore click drops you in your DMs rather than on a block page.

Why the `+` lines: LeechBlock matches by **prefix**, so a bare `instagram.com/explore` would swallow search results too — they live *under* `/explore/`. A `+` marks an exception, and exceptions beat blocks, so those three paths stay reachable while the `/explore/` grid itself does not. (Prefix matching also means you don't need the old `/*` variants — `instagram.com/reels` already covers `/reels/anything`.)

⚠️ **Verify the search paths rather than trusting them.** Instagram doesn't document these URLs and reshuffles them without warning; the three above are the shapes as of writing. In your 4.4 test, run a real search, press Enter for the full-results page, and open a hashtag. If either lands on the block page, copy the URL from the address bar and add its path as another `+` line.

Leave `instagram.com/direct` **out** of the block list entirely — that's DMs.

_Instagram reshuffles its markup often. If the feed reappears, right-click it → Inspect and update the `article` selector above._

_If **search results** ever come back empty-looking, suspect the feed rule rather than the path blocks: `main[role="main"] > div:has(article)` is unscoped, so it fires on every page, and it would hide a results grid that happens to use `article` too. On AdGuard you can pin it to the site root only by prefixing the rule with `[$path=/^\/$/]`. uBO has no `$path` equivalent — there, drop the rule and rely on the redirect instead._

**Keep reachable** (do NOT block): `/direct` (DMs), the search panel, `/explore/search`, `/explore/tags`, and profile pages.

**4.4 test for this site:** open Instagram → **no feed**, DMs work. ✅ Click **Search**, type a name → the panel returns results, clicking one opens the profile. ✅ Try `/explore/` directly → bounced to your DM inbox. ✅

## Part 5 — whole-domain values (for browsers you DON'T use for Instagram)

- **URLBlocklist:** `instagram.com`, `*://*.instagram.com/*`
- **Safari Restricted / iPhone NEVER ALLOW:** `https://www.instagram.com`, `https://instagram.com`
- **NextDNS / hosts (nuclear):** `instagram.com`, `www.instagram.com`, `i.instagram.com`, `api.instagram.com`, `graph.instagram.com`, `platform.instagram.com`, `cdninstagram.com` — **never** `fbcdn.net` (shared Meta infra; breaks Messenger/WhatsApp).

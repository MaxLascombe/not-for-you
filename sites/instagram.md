# Instagram — block values

The site-specific values to plug into the method in the [main README](../README.md). Each heading maps to a Part there.

## What to block vs keep

- **iPhone (Tier 1):** everything — Instagram is fully dark.
- **Mac (Tier 2):** kill the **feed, Reels, and the Explore grid**; keep **DMs** (`/direct`) **and search**.
  Instagram resists path blocking harder than any other site here. Its feed lives at the site **root** (`instagram.com/`), so blocking that path takes DMs with it; its Explore grid shares a page with search, so blocking *that* path takes search. Both therefore need **cosmetic hiding**, and only Reels — which has a URL of its own — gets path-blocked.

**On keeping search — why this site needs a different tool.** Search and Explore are **the same page**. You go to `/explore/`, and before you type anything it shows the algorithmic grid of posts from strangers; once you type, the same page shows your results. There's no separate search URL to allow and no explore URL to block.

That kills the path-blocking approach outright: block `/explore/` and you block the search box along with the grid. So Instagram's Explore is handled **entirely with cosmetic rules** — the page stays reachable, and we delete its *contents* while leaving the search field standing.

The distinction that makes it surgical is **what each state actually renders**:

| On `/explore/` | What it is | Verdict |
|---|---|---|
| Search field, recent searches | Search | **Keep** |
| Grid of post / reel thumbnails | The algorithmic Explore feed | **Hide** |
| Rows of accounts and hashtags | Search results | **Keep** |

Explore is a **grid of post thumbnails**; results are **rows of accounts**. Those are different elements, so a rule that hides post thumbnails on that one page removes the discovery grid and leaves search — field, recents, and results — completely intact.

## Part 2 — iPhone website block (NEVER ALLOW list)

Add both:
- `https://www.instagram.com`
- `https://instagram.com`

## Part 3 — iPhone app block

- Category: **Social**. Instagram sits under Social in Screen Time's App Limits, so the category limit catches it even with the app uninstalled and survives a reinstall.

## Part 4 — Mac feed-killer

**Cosmetic filters** (AdGuard / uBO user rules) — the home feed and the Reels nav:
```
www.instagram.com##main[role="main"] > div:has(article)
www.instagram.com##a[href^="/reels/"]
```

⚠️ **Do not hide `a[href="/explore/"]`.** An earlier version of this guide did — but that nav link is now your route to the search box. Hiding it hides search. (If your window is wide enough that the sidebar shows a *separate* Search item alongside Explore, then hiding the Explore link is safe and worth doing.)

**Explore-grid rules — page-scoped, so they only fire on `/explore/`.** Both extensions can restrict a cosmetic rule to one URL; the syntax differs, so use the block for the extension you installed in 4.1.

**AdGuard** (Chrome / Arc) — the `[$path=…]` prefix:
```
[$path=/^\/explore\/?(\?|$)/]www.instagram.com##div:has(> a[href^="/p/"])
[$path=/^\/explore\/?(\?|$)/]www.instagram.com##div:has(> a[href^="/reel/"])
```

**uBO** (Firefox) — the `:matches-path()` operator, same regex:
```
www.instagram.com##div:has(> a[href^="/p/"]):matches-path(/^\/explore\/?(\?|$)/)
www.instagram.com##div:has(> a[href^="/reel/"]):matches-path(/^\/explore\/?(\?|$)/)
```

Reading the rule: `div:has(> a[href^="/p/"])` is "a div whose **direct child** is a link to a post" — that's one thumbnail tile. The `>` matters. Without it, the selector also matches every *ancestor* that contains a post link anywhere inside, which walks up the tree and takes the search field down with it.

**Verified against the live DOM (2026-08-11).** Reading Explore while logged in confirmed both halves of the design:

- A tile is an `<a href="/p/…">` whose **direct parent is a `div`** — so `div:has(> a[href^="/p/"])` matches, at exactly one thumbnail. Ancestors go: `a` → `div.html-div…` (tile) → `div.xeuugli x6ikm8r…` (cell) → `div.x121lspk…` (row).
- **Once you type a search, the page contains no `/p/` links at all.** Results are accounts and hashtags, not post thumbnails. So these rules physically cannot hide your search results — there's nothing there for them to match.

That second point is the load-bearing one. It's why hiding at the *tile* level is safe, and why widening the rule to a big container would be the thing that breaks search.

**If hidden tiles leave blank gaps**, climb one rung at a time — each adds one `> div` and each was confirmed present:

| Level | Selector | Hides |
|---|---|---|
| Tile *(start here)* | `div:has(> a[href^="/p/"])` | one thumbnail |
| Cell | `div:has(> div > a[href^="/p/"])` | the thumbnail's cell |
| Row | `div:has(> div > div > a[href^="/p/"])` | the whole row, no gap left |

_The `x…` class names above are machine-generated and change between deploys — they're recorded to show which level is which, not to be pasted into a rule. The tile link also carried a `_a6hd` class; Instagram's underscore classes churn less than the `x…` ones, so `a._a6hd` is a last-resort anchor if href matching ever stops working._

Reading the scope: the regex matches `/explore`, `/explore/`, and `/explore/?anything`, but **not** `/explore/tags/…`. Both extensions match the query string as well as the path, hence the `(\?|$)`. So hashtag pages keep their grids — they're a search result you asked for, not a feed handed to you. If you'd rather kill those too, widen the regex to `/^\/explore/`.

This holds up whether or not your results change the URL — which is deliberate, because that turned out to be the one thing the DOM check *didn't* settle. If Instagram navigates to a distinct results URL, the page scope excludes it. If it renders results in place at `/explore/`, the scope applies but finds no post tiles to hide. Search survives on both branches, so the question stops mattering.

So why keep the page scope at all, if results have no tiles to hide? Because **profile pages and hashtag pages are full of `/p/` links** — they're post grids too. Unscoped, these rules would blank every profile you visit. The scope is what confines them to Explore.

**Path blocks** (LeechBlock NG) — Reels only:
```
instagram.com/reels
```
Set this block set's **"Redirect to this URL instead"** to `https://www.instagram.com/direct/inbox/`, so a stray Reels click drops you in your DMs rather than on a block page. LeechBlock matches by **prefix**, so `instagram.com/reels` already covers `/reels/anything` — the old `/*` variants aren't needed.

⚠️ **Do not path-block `instagram.com/explore`.** That's the whole point of this section: search lives there. The grid is handled cosmetically above.

Leave `instagram.com/direct` out of the block list too — that's DMs.

_The tile rules were checked against the live DOM on 2026-08-11 (see above); the home-feed rule (`article`) hasn't been re-checked since. Instagram reshuffles its markup often, and Explore is login-walled — so when something breaks, don't guess, re-run the probe below in your own session._

### Deriving the exact selector yourself (30 seconds)

Open `instagram.com/explore/`, press **⌥⌘J** for the console, and paste:

```js
(() => {
  const a = document.querySelector('main a[href^="/p/"], main a[href^="/reel/"]');
  const path = location.pathname + location.search;
  if (!a) return { path, tiles: 'none — expected once you have typed a search' };
  const chain = []; let el = a;
  for (let i = 0; i < 6 && el; i++, el = el.parentElement) {
    const cls = typeof el.className === 'string' && el.className.trim()
      ? '.' + el.className.trim().split(/\s+/).join('.') : '';
    chain.push(`${i === 0 ? 'TILE ' : `parent ${i}: `}${el.tagName.toLowerCase()}${cls}`);
  }
  return { path, tile: a.getAttribute('href'), chain };
})()
```

It prints the tile link and its ancestor chain. `parent 1` is what `div:has(> a[href^="/p/"])` targets — if that isn't a single thumbnail, climb the ladder above until it is.

Then **type a search and run it again**. Expect `tiles: none` — that's the healthy answer, and the check that matters: it proves results contain nothing these rules can hide. If instead you get a chain back, results now render as post tiles too and the rules would swallow them, so tighten the scope before trusting the setup. The `path` it reports on that second run also tells you whether results get their own URL.

_Instagram's class names are machine-generated and change between deploys — read them as "which level am I at," never as something to paste into a rule._

**Keep reachable** (do NOT block): `/direct` (DMs), `/explore/` itself (search), `/explore/tags/…`, and profile pages.

**4.4 test for this site:** open Instagram → **no feed**, DMs work. ✅ Go to **Explore/Search** → the search field is there and the grid below it is **gone**. ✅ Type a name → results appear and clicking one opens the profile. ✅ Open a profile → their posts still show (proving the tile rule stayed scoped to `/explore/`). ✅

## Part 5 — whole-domain values (for browsers you DON'T use for Instagram)

- **URLBlocklist:** `instagram.com`, `*://*.instagram.com/*`
- **Safari Restricted / iPhone NEVER ALLOW:** `https://www.instagram.com`, `https://instagram.com`
- **NextDNS / hosts (nuclear):** `instagram.com`, `www.instagram.com`, `i.instagram.com`, `api.instagram.com`, `graph.instagram.com`, `platform.instagram.com`, `cdninstagram.com` — **never** `fbcdn.net` (shared Meta infra; breaks Messenger/WhatsApp).

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

Reading the scope: the regex matches `/explore`, `/explore/`, and `/explore/?anything`, but **not** `/explore/tags/…`. Both extensions match the query string as well as the path, hence the `(\?|$)`. So hashtag pages keep their grids — they're a search result you asked for, not a feed handed to you. If you'd rather kill those too, widen the regex to `/^\/explore/`.

This holds up whether or not your results change the URL. If Instagram navigates to a distinct results URL, the scope excludes it. If it renders results in place at `/explore/`, the rules still only remove *post thumbnails*, so account and hashtag rows show through. Either way search survives — which is the reason the rule targets tiles rather than the container they sit in.

**Path blocks** (LeechBlock NG) — Reels only:
```
instagram.com/reels
```
Set this block set's **"Redirect to this URL instead"** to `https://www.instagram.com/direct/inbox/`, so a stray Reels click drops you in your DMs rather than on a block page. LeechBlock matches by **prefix**, so `instagram.com/reels` already covers `/reels/anything` — the old `/*` variants aren't needed.

⚠️ **Do not path-block `instagram.com/explore`.** That's the whole point of this section: search lives there. The grid is handled cosmetically above.

Leave `instagram.com/direct` out of the block list too — that's DMs.

_Instagram reshuffles its markup often. Both the feed rule (`article`) and the tile rules (`a[href^="/p/"]`) are written against its structure as of this guide, and Explore is behind a login wall, so they can't be verified from outside your own session._

### Deriving the exact selector yourself (30 seconds)

Instagram's Explore is login-walled, so the only place its real markup exists is your logged-in browser. Rather than guess when a rule misfires, read it directly: open `instagram.com/explore/`, press **⌥⌘J** for the console, and paste:

```js
(() => {
  const a = document.querySelector('main a[href^="/p/"], main a[href^="/reel/"]');
  if (!a) return 'No post tiles found — is the grid actually on screen?';
  const chain = []; let el = a;
  for (let i = 0; i < 6 && el; i++, el = el.parentElement) {
    const cls = typeof el.className === 'string' && el.className.trim()
      ? '.' + el.className.trim().split(/\s+/).join('.') : '';
    chain.push(`${i === 0 ? 'TILE ' : `parent ${i}: `}${el.tagName.toLowerCase()}${cls}`);
  }
  return { path: location.pathname + location.search, tile: a.getAttribute('href'), chain };
})()
```

It prints the tile link and its ancestor chain. `parent 1` is what `div:has(> a[href^="/p/"])` targets — if that isn't a single thumbnail, use the level that is. Then **type a search and run it again**: if `path` changes, your results live at their own URL and the page scope already excludes them; if it stays `/explore/`, the tile-level rule is what's keeping your results visible, so don't widen it to a container.

_Instagram's class names are machine-generated and change between deploys — read them as "which level am I at," never as something to paste into a rule._

_If the hidden tiles leave a page of blank gaps, the grid is reserving space for rows that are now empty. Move one level up — `div:has(> div > a[href^="/p/"])` — to take the row with them._

**Keep reachable** (do NOT block): `/direct` (DMs), `/explore/` itself (search), `/explore/tags/…`, and profile pages.

**4.4 test for this site:** open Instagram → **no feed**, DMs work. ✅ Go to **Explore/Search** → the search field is there and the grid below it is **gone**. ✅ Type a name → results appear and clicking one opens the profile. ✅ Open a profile → their posts still show (proving the tile rule stayed scoped to `/explore/`). ✅

## Part 5 — whole-domain values (for browsers you DON'T use for Instagram)

- **URLBlocklist:** `instagram.com`, `*://*.instagram.com/*`
- **Safari Restricted / iPhone NEVER ALLOW:** `https://www.instagram.com`, `https://instagram.com`
- **NextDNS / hosts (nuclear):** `instagram.com`, `www.instagram.com`, `i.instagram.com`, `api.instagram.com`, `graph.instagram.com`, `platform.instagram.com`, `cdninstagram.com` — **never** `fbcdn.net` (shared Meta infra; breaks Messenger/WhatsApp).

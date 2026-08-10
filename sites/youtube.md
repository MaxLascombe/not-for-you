# YouTube — block values

The site-specific values to plug into the method in the [main README](../README.md). Each heading maps to a Part there.

## What to block vs keep

- **Mac (Tier 2):** kill the **algorithmic home page and Shorts**; keep **Subscriptions, search, watch pages, channels, and playlists**. Opening `youtube.com` **lands you on your Subscriptions feed** — the chronological, you-chose-this list — instead of the recommendation wall.
- **iPhone (Tier 1):** your call, and it's **binary there** — iOS has no extensions in the app or in Safari's content filter, so there's no way to kill just the home feed. It's all of YouTube or none of it. If YouTube is a scroll trap on your phone, go fully dark with Parts 2–3. If you keep it, you're keeping the home feed with it.

YouTube is the odd one out in this repo. Everywhere else the thing worth keeping is messaging, so the pattern is "hide one feed, keep the rest." Here the thing worth keeping is **the videos themselves**, and the thing to kill is the *front door*. So this guide inverts the approach: **default-deny the whole domain, then allow back an explicit list of intentional destinations.** Anything YouTube invents next — a new feed, a new tab — is blocked by default instead of needing a new rule.

## Part 2 — iPhone website block (NEVER ALLOW list) — only if going fully dark

Add all three:
- `https://www.youtube.com`
- `https://youtube.com`
- `https://youtu.be` — the share-link shortener. Skip it and every link someone texts you still opens.

## Part 3 — iPhone app block — only if going fully dark

- Category: **Entertainment**. ⚠️ Broader than the other sites' categories — it also catches Netflix, Disney+, Prime Video, and friends. If you don't want those limited too, use an **individual app limit** on YouTube instead (per README 3.1: needs the app installed to pick it, and resets on reinstall — so it leans harder on Parts 1–2).

## Part 4 — Mac feed-killer

**Path blocks (LeechBlock NG) — this is the main event here, not the cosmetic filters.**

Block set → "Sites to block":

```
youtube.com
+youtube.com/feed/subscriptions
+youtube.com/watch
+youtube.com/results
+youtube.com/@
+youtube.com/channel/
+youtube.com/playlist
+youtube.com/feed/history
+youtube.com/feed/playlists
+youtube.com/feed/library
```

Then set that block set's **"Redirect to this URL instead"** to:

```
https://www.youtube.com/feed/subscriptions
```

Net effect: the home page — plus Shorts, Trending, Explore, and any other algorithmic surface not named above — silently drops you on **Subscriptions**. The videos, search, and channels you went there for are untouched.

⚠️ **Keep `+youtube.com/feed/subscriptions` in the list.** It's the redirect target; remove it and the redirect lands on a blocked page and bounces forever.

Three things about LeechBlock's matching that make the above work (verified against its source, not just the docs):

- **Exceptions beat blocks.** A URL is blocked only if it matches a block pattern *and* no `+` pattern. So the bare `youtube.com` line is safe to write even though it looks like it blocks everything.
- **Patterns match by prefix, and can include paths.** `+youtube.com/watch` covers `/watch?v=…&list=…`; `+youtube.com/@` covers every `/@handle` page and its sub-tabs.
- **`www.` is optional.** LeechBlock normalises it, so `youtube.com` and `www.youtube.com` are the same rule. **Subdomains are not** — unless you tick "Match subdomains," `music.youtube.com` and `m.youtube.com` are untouched by this set. Tick it if YouTube Music is also a problem for you; leave it off if Music is the point.

**Cosmetic filters** (AdGuard / uBO user rules) — blank the home feed and hide the Home/Shorts entries in the sidebar, so the SPA has nothing to show during the beat before the redirect fires:

```
www.youtube.com##ytd-browse[page-subtype="home"]
www.youtube.com##ytd-guide-entry-renderer:has(a#endpoint[href="/"])
www.youtube.com##ytd-mini-guide-entry-renderer:has(a[href="/"])
www.youtube.com##ytd-guide-entry-renderer:has(a#endpoint[href^="/shorts"])
www.youtube.com##ytd-mini-guide-entry-renderer:has(a[href^="/shorts"])
```

_These match on `href`, not on the visible label, so they survive a UI language change — `:has(a[title="Home"])` would break the moment YouTube renders in French._

**Optional but recommended — kill the "Up next" rail on watch pages.** This is the surface that quietly turns one deliberate video into five:

```
www.youtube.com##ytd-watch-next-secondary-results-renderer
```

_Leave it out if you use the autoplay queue deliberately. Adding it is the difference between "I watched the thing I came for" and "it's 1am."_

**Keep reachable** (do NOT block): `/feed/subscriptions`, `/watch`, `/results` (search), `/@…` and `/channel/…`, `/playlist`, `/feed/history`.

**4.4 test for this site:** open `youtube.com` → you land on **Subscriptions**. ✅ Search something → results load. ✅ Open a video, open a channel → both fine. ✅ Try `youtube.com/shorts` → redirected away. ✅
_Embeds on other sites keep working — LeechBlock only inspects top-level page loads, not iframes, so YouTube videos embedded in docs and courses are unaffected._

## Part 5 — whole-domain values (for browsers you DON'T use for YouTube)

- **URLBlocklist:** `youtube.com`, `*://*.youtube.com/*`, `youtu.be`, `*://*.youtu.be/*`
- **Safari Restricted / iPhone NEVER ALLOW:** `https://www.youtube.com`, `https://youtube.com`, `https://youtu.be`
- **NextDNS / hosts (nuclear):** `youtube.com`, `www.youtube.com`, `m.youtube.com`, `youtu.be`, `youtube-nocookie.com`, `youtubei.googleapis.com` (the app/API backend)
  ⚠️ **Never** block `googlevideo.com` or `ytimg.com`. That's shared delivery infrastructure — killing it breaks every YouTube embed on *every other site* (courses, docs, blogs), not just youtube.com.

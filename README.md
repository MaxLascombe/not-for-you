# Quitting Instagram — the setup that actually holds

A free, self-binding block for Instagram (website + app) on iPhone and Mac. Built as a **commitment device**: the goal isn't just to block Instagram, it's to make it genuinely hard for **future-you at midnight** to unblock it.

---

## The mental model (read this first)

Two facts drive the whole design:

1. **Your iPhone is where the addiction lives.** It's in your pocket, your bed, your idle moments — that's where nearly all the relapse risk is. The Mac is *deliberate* use (you have to sit down and open a browser), so it's a much smaller risk.
2. **iOS and macOS aren't equal.** iOS has one clean, broad, **partner-lockable** website block that covers every browser and survives a VPN. macOS has nothing that strong for free — which is why the Mac side is friction, not a fortress. Don't fight that; work with it.

So the strategy is two tiers:

| | **Tier 1 — iPhone** | **Tier 2 — Mac** |
|---|---|---|
| Role | **The real lock** | **Friction, not a fortress** |
| Instagram access | **None** — fully blocked | **DMs only** — feed killed, messaging kept |
| How | Screen Time website + app block, **locked by your partner** | Feed-killer browser extension + one intentional door |
| Undo-resistance | Hard — needs a passcode you don't have | Soft — you *can* undo it, but it's annoying enough to stop impulse |

And the one truth no tool changes: **the real lock is your accountability partner.** Every free layer is undoable by someone with admin rights to their own devices. The thing that actually holds is a person who won't hand you the passcode. Pick that person well — everything else is drywall around that one load-bearing wall.

> ⚠️ **Golden rule:** don't set or lock any passcode until the very end (Part 6). Configure and **test** everything first. If you lock too early and something's broken, you're stuck.

**You'll need:** ~30–40 min, your iPhone, your Mac, and your **partner reachable** for Part 6 (the final lock).

---

# TIER 1 — iPhone: the real lock

## Part 1 — Prep (5 min)

**1.1 Confirm Find My is on** (this is what blocks USB unlock tools).
- ☐ Settings → tap your **name** → **Find My** → **Find My iPhone** → confirm the top toggle is **green/ON**.
  → You should see: "Find My iPhone: On".

**1.2 Confirm the Instagram app is deleted.**
- ☐ Swipe down, search "Instagram". If the app appears, press and hold → **Remove App** → **Delete App** → **Delete**.
  → You should see: no Instagram app left (only the App Store result).

---

## Part 2 — Block the website ⭐ the single most important step

_This is the winner: one setting blocks Instagram in **every browser at once**, it's **VPN-proof**, and it's the one thing you can **partner-lock**._

**2.1 Open Screen Time.**
- ☐ Settings → **Screen Time**. If you see **Turn On Screen Time**, tap it → **Continue** → **This is My iPhone**. (Do NOT set a passcode if asked — skip it for now.)

**2.2 Turn on restrictions.**
- ☐ Tap **Content & Privacy Restrictions** → toggle the top switch **green/ON**.
  → You should see: the list below stops being greyed out.

**2.3 Open Web Content.**
- ☐ Tap **Content Restrictions** → **Web Content**.
  → You should see: Unrestricted Access / **Limit Adult Websites** / Allowed Websites Only.

**2.4 Switch to Limit Adult Websites** (this unlocks the Never Allow list).
- ☐ Tap **Limit Adult Websites**.
  → You should see: a checkmark, plus new **ALWAYS ALLOW** and **NEVER ALLOW** sections.

**2.5 Add Instagram to NEVER ALLOW.**
- ☐ Under **NEVER ALLOW** → **Add Website** → type `https://www.instagram.com` → save.
- ☐ **Add Website** again → type `https://instagram.com` → save.
  → You should see: both entries under NEVER ALLOW.

**2.6 TEST IT NOW** (before anything is locked).
- ☐ Safari → `instagram.com` → **blocked** ("You cannot browse this page"). ✅
- ☐ Any other browser (Chrome, Arc) → `instagram.com` → **also blocked** (the filter is system-wide). ✅
- ☐ If it loads: re-check Part 2.5 — the entries must be under **NEVER ALLOW**, not Always Allow.

---

## Part 3 — Block the app from opening (5 min)

_Uses an **App Limit on the Social category** — blocks Instagram (and other feeds) from opening, survives reinstalls, needs no age restriction, and doesn't remove the App Store._

**3.1 Create the limit.**
- ☐ Settings → Screen Time → **App Limits** → **Add Limit**.
- ☐ Tick the **Social** category.
  _The category list is Apple's built-in classification, **not** your installed-apps list — so it's selectable even with Instagram uninstalled, and it auto-applies the moment any social app is downloaded._
  ⚠️ _Don't use the "tick just Instagram" approach — an individual-app limit needs the app installed to select it **and** gets wiped on reinstall. The category limit avoids both._
- ☐ **Next** → set the time to **1 minute** (the minimum) → turn **ON** **Block at End of Limit** ← this makes it a wall, not a nudge → **Add**.
  → You should see: the Social limit under App Limits.

**3.2 Verify.**
- ☐ If Instagram is installed, open it for a minute → you hit a **"Limit Reached"** screen. Once locked (Part 6) there's **no "Ignore Limit" escape**. ✅
  _The app can still be **downloaded**, but the category limit blocks it from **opening**, even after reinstall. The website stays blocked separately via Part 2._

---

# TIER 2 — Mac: kill the feed, keep your DMs

_On the Mac you want to **still message people**, so a whole-domain block is off the table — it would take your DMs with it. The catch worth understanding: **nothing at the DNS or OS level can tell your feed apart from your DMs.** Same site, same domain, same login — the only place that distinction exists is *inside the page*. So feed-only blocking has to be a **browser extension**, which makes this tier the weakest link as a lock (two clicks to disable). That's fine: the iPhone is the real lock, and the whole point of hiding the feed is that even when you open Instagram to message someone, **there's no feed to fall into.**_

## Part 4 — Feed-killer extension ⭐ the Mac layer (10 min)

_Pick the **one browser you'll use for Instagram DMs** (e.g. Arc) and set it up there. Two pieces: hide the home feed (cosmetic filter) and block Reels/Explore by path. Your DMs at `/direct` stay fully working._

**4.1 Install the extension.**
- ☐ **Chrome / Arc / other Chromium:** install **AdGuard** (classic uBlock Origin no longer runs on Chromium's MV3, and uBlock Origin Lite can't do custom cosmetic rules).
- ☐ **Firefox:** install **uBlock Origin** instead.

**4.2 Hide the home feed (cosmetic filters).**
- ☐ AdGuard → **Settings → User rules** (uBO → **Dashboard → My filters**) → add:
```
www.instagram.com##main[role="main"] > div:has(article)
www.instagram.com##a[href="/explore/"]
www.instagram.com##a[href^="/reels/"]
```
  _First line hides the scrolling feed; the other two hide the Explore and Reels nav links so you don't wander in._

**4.3 Block Reels/Explore by path (LeechBlock NG).**
- ☐ Install **LeechBlock NG** → Block Set 1 → "Sites to block":
```
instagram.com/reels
instagram.com/explore
instagram.com/reels/*
instagram.com/explore/*
```
- ☐ Leave `instagram.com/direct` **out** — that's your DMs.
  _The cosmetic rules hide these; this blocks the full-screen Reels/Explore players too, even if you paste a direct link._

**4.4 Test.**
- ☐ `instagram.com` → loads, but **no feed** (empty home). ✅
- ☐ `instagram.com/reels` → **blocked / redirected**. ✅
- ☐ `instagram.com/direct` → **DMs work**, you can message people. ✅
- ☐ If the feed reappears later: Instagram changed its markup — the `article` selector in 4.2 needs a tweak (see troubleshooting).

**4.5 (Optional) Make the extension harder to remove.**
- ☐ Force-install it via policy so it isn't a two-click uninstall. Still not partner-lockable — it's friction, not a wall. See Optional add-ons.

---

## Part 5 — One door in: fully block Instagram in every *other* browser (recommended, 5 min)

_Part 4 gives you one browser where DMs work and the feed is dead. To stop yourself drifting into a full Instagram anywhere else, block it completely in the browsers you **didn't** choose — so there's exactly one intentional door._

**5.1 Safari** (this one is **partner-lockable** in Part 6) — Apple menu → **System Settings** → **Screen Time** (same Apple ID as iPhone; **Turn On** if off, no passcode yet) → **Content & Privacy** → toggle **ON** → **Content Restrictions** → **Web Content** → **Limit Adult Websites** → **Customize…** → under **Restricted**, add `https://www.instagram.com` and `https://instagram.com` → **Done**.

**5.2 Other Chromium browsers you don't use for IG** — block them with a hard admin page via the **URLBlocklist** add-on (see Optional add-ons). Don't apply it to your Part 4 browser.

**5.3 Test.**
- ☐ **Safari** → `instagram.com` → **blocked**. ✅
- ☐ Your **Part 4 browser** → `/direct` → **DMs still work**. ✅

---

# Part 6 — Hand the keys to your partner 🔒 do this LAST

_Only start once Tiers 1 and 2 are tested and working. **This is the step that makes it self-binding.** Your partner does the typing; you look away._

**6.1 (Recommended) Family Sharing with your partner as organizer** — closes the factory-reset escape.
- ☐ Settings → your **name** → **Family Sharing** → set up with your **partner as organizer** (or join their family). _If this is too much, skip to 6.2 — you still get a strong lock, just without reset-proofing._

**6.2 Lock Screen Time on the iPhone — partner types the code.**
- ☐ Settings → **Screen Time** → **Lock Screen Time Settings**.
- ☐ **Hand the phone to your partner.** They enter a 4-digit code — **you look away, you must not see it** — and re-enter to confirm.
- ☐ When it asks for **Screen Time Passcode Recovery** (an Apple ID), your partner enters **THEIR Apple ID**, or taps **Cancel** if using Family Sharing.
  → ⚠️ Do NOT enter *your* Apple ID here — that would let you reset the code yourself via "Forgot Passcode?".

**6.3 Lock Screen Time on the Mac** (same code).
- ☐ System Settings → **Screen Time** → **Lock Screen Time Settings** → partner enters the **same code**.

**6.4 FINAL TEST — you, without the code.**
- ☐ iPhone: Settings → Screen Time → Content & Privacy → try to set **Web Content** back to Unrestricted → it **demands the passcode you don't have**. ✅
- ☐ iPhone Safari **and** another browser → `instagram.com` → blocked. ✅
- ☐ Open Instagram (or its App Limit in Settings) → can't raise/remove the limit; app hits the "Limit Reached" wall. ✅
- ☐ Mac Safari → `instagram.com` → blocked. ✅
- ☐ Your Part 4 browser → feed is empty, `/direct` → DMs work. ✅ (This one isn't partner-locked — it's the door you chose to keep.)

If those pass — **you're done. The off switch now lives with your partner.**

---

## Optional add-ons (only if you want them)

These are extra friction or nice-to-haves. **None of them is the lock** — Parts 2, 3, and 6 are. Skip unless you have a specific reason.

<details>
<summary><b>Hard-block Instagram in your <i>non-DM</i> browsers (URLBlocklist)</b></summary>

This is Part 5.2 — the "one door in" enforcement for the Chromium browsers you **don't** use for DMs. It shows an instant `ERR_BLOCKED_BY_ADMINISTRATOR` page. Replace the bundle ID with the browser's:

- **Chrome:** `com.google.Chrome` · **Arc:** `company.thebrowser.Browser` · **Edge:** `com.microsoft.Edge`

```
defaults write <BUNDLE_ID> URLBlocklist -array \
  "instagram.com" \
  "*://*.instagram.com/*"
```

Fully quit and reopen the browser, then check `chrome://policy` (or `arc://policy`) → **Reload policies**.
⚠️ **Do NOT run this on your Part 4 browser** — it blocks the whole domain, DMs included.
_To undo: `defaults delete <BUNDLE_ID> URLBlocklist`, restart the browser._
_Arc's policy support is partial — verify it actually took in `arc://policy`._
_To make it un-toggleable (friction, not a partner-lock), write it under `/Library/Managed\ Preferences/<BUNDLE_ID>` with `sudo` — only removable by deleting that plist as admin._

</details>

<details>
<summary><b>Force-install the feed-killer so it can't be uninstalled in two clicks</b></summary>

Makes the Part 4 extension resist a casual disable. Friction only — not partner-lockable.

- **Chromium:** add the extension's ID to the `ExtensionInstallForcelist` policy (`defaults write <BUNDLE_ID> ExtensionInstallForcelist -array "<EXT_ID>;https://clients2.google.com/service/update2/crx"`). A force-installed extension can't be toggled off from `chrome://extensions`.
- Grab `<EXT_ID>` from the extension's `chrome://extensions` card. Verify at `chrome://policy` → Reload policies.

</details>

<details>
<summary><b>Changed your mind — you want DMs gone too (whole-domain block)</b></summary>

If keeping DMs turns out to be the crack that lets you back in, drop the "keep DMs" goal and block the whole domain across every browser at once with **NextDNS** (free, OS-level, catches the app APIs too):

- Sign up at **nextdns.io**, add to the **Denylist**: `instagram.com`, `www.instagram.com`, `i.instagram.com`, `api.instagram.com`, `graph.instagram.com`, `platform.instagram.com`, `cdninstagram.com`. ⚠️ **Not** `fbcdn.net` (shared Meta infra — breaks Messenger/WhatsApp).
- Install the macOS profile, turn on **Block bypass methods**, and point each browser's **Secure DNS** at your NextDNS DoH URL (else DNS-over-HTTPS routes around it).
- This **kills DMs** — that's the whole point of this option. It replaces Part 4 rather than adding to it.

</details>

---

## Quick troubleshooting

- **Website not blocked after Part 2.5?** The entries are probably under "Always Allow" instead of "Never Allow." Re-do 2.4–2.5.
- **iPhone: blocked in Safari but not Chrome/Arc?** The filter is system-wide — make sure Content & Privacy Restrictions (2.2) is actually ON.
- **Mac: the feed came back (Part 4)?** Instagram changed its HTML. Open the home page, right-click the feed → Inspect, find the container element, and update the `##main[role="main"] > div:has(article)` rule in 4.2 to match. This is the built-in cost of the cosmetic approach — it needs the occasional tweak.
- **Mac: DMs stopped working?** You probably applied a whole-domain block (URLBlocklist / NextDNS / hosts) to your Part 4 browser. Those don't do feed-only — remove the block from that one browser.
- **Locked too early and something's broken?** Your partner unlocks with the code, you fix it, they re-lock. (This is why Part 6 comes last.)

---

## The one honest limitation

Two soft spots, by design:

1. **The human link.** If you persuade your partner to give you the iPhone code, it's undone in 30 seconds. The tech is genuinely hard to beat now — the iPhone block even survives a VPN. The weak point is the conversation at midnight. **Choose a partner who will say no.**
2. **The Mac door you chose to keep.** Because you want DMs, the Mac tier is a browser extension — two clicks to disable. That's an accepted tradeoff, not a bug: the feed being hidden means the DM browser isn't a scrolling trap, and the iPhone (where the addiction actually lives) stays fully locked. If you find yourself disabling the extension to get the feed back, that's the signal to switch to the whole-domain block (Optional add-ons) and give up DMs on the laptop.

If you ever catch yourself beating even the iPhone lock — deleting profiles, factory-resetting — the nuclear option below removes those toggles from your reach entirely. Most people never need it.

---

## Appendix — The Nuclear Option (supervised iPhone + locked DNS)

This is the free-but-heavy path for when the tiers above aren't enough — when you want the DNS block **impossible to toggle off in Settings** and the whole profile **impossible to delete without a password your partner holds**. It's what an employer or school does to a managed phone, applied to yourself as a commitment device.

**The cost is real:** supervising a phone **erases it** (full wipe + restore), set up from a Mac with Apple's free tools. Do this only if the partner-locked Screen Time setup has proven too easy to talk your way around.

### What supervision buys you that Screen Time can't

| Capability | Screen Time (Tiers 1–2) | Supervised profile |
|---|---|---|
| Block instagram.com | ✅ (VPN-proof) | ✅ |
| Block the app's APIs via DNS | ⚠️ DNS is toggleable in Settings | ✅ **DNS off-switch greyed out** ("Prohibit Disablement") |
| Stop profile removal | ❌ | ✅ **Password-protected removal** |
| Survive "Reset Network Settings" | ❌ | ✅ (Restrictions can disable erase) |
| Undo path | Partner-held passcode | Partner-held **removal password** + wipe |

### What you need (all free)

- A **Mac**, **Apple Configurator** (Mac App Store), **iMazing Profile Editor** (for the DNS payload with "Prohibit Disablement"), and your **NextDNS** config. ~45–60 min plus a phone wipe.

### Step 1 — Supervise the iPhone (this wipes it)

1. Back up first (iCloud or Finder) — supervision **erases** the phone.
2. Apple Configurator → connect by cable → right-click device → **Prepare** → **Manual**, **Do not enroll in MDM**, tick **Supervise devices**.
3. Let it erase and reprovision, then restore your backup.
4. Confirm: Settings → General → VPN & Device Management shows "This iPhone is supervised."

### Step 2 — Build the locked DNS profile

In **iMazing Profile Editor**:

1. **DNS Settings** (DoH/DoT) payload → paste your **NextDNS DoH URL** (`https://dns.nextdns.io/<yourid>`).
2. Turn ON **"Prohibit Disablement"** — greys out the DNS off-switch in Settings.
3. **Restrictions** payload → disable **Allow Erase All Content and Settings** (blocks on-device reset) and **Allow adding VPN configurations** (stops a rogue VPN re-routing around DNS).
4. **General** payload → **Security → When removing profile → With Authorization** → set a **removal password your partner types and you never see**.

### Step 3 — Install and lock

1. Install the `.mobileconfig` on the supervised iPhone (AirDrop or Configurator).
2. Verify: profile shows as **non-removable** (removal demands the password); DNS toggle in Settings is **greyed out**. ✅
3. Verify the block on cellular (Wi-Fi off): app + Safari both fail to reach Instagram. ✅

### The residual holes (be honest)

- **A cabled DFU restore from a computer still wipes supervision.** Disabling on-device "Erase All Content" doesn't close this. Fully closing it needs **Apple Business Manager** (beyond a free personal setup). **Mitigation:** keep **Activation Lock** under **your partner's Apple ID**, so a wiped phone needs *their* credentials to reactivate.
- **Known NextDNS bug:** combining NextDNS's `.mobileconfig` with "Prohibit Disablement" can make the profile fail to load on some iOS versions. If DNS silently stops, test with the flag off, then re-enable — or use a plain DoH profile pointed at your NextDNS endpoint.
- **The human link is still the real lock.** Even here, if your partner hands over the removal password, it's over. Supervision just raises the effort from "tap a toggle" to "wipe and rebuild the phone" — that gap is the point.

### When to actually do this

The tiers above stop impulse use and survive a VPN. Reach for this **only** if you've caught yourself flipping the NextDNS toggle off or deleting profiles to sneak back in. It's the difference between a locked door and a welded one — most people never need the weld.

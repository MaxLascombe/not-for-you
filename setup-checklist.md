# Instagram Block — Setup Checklist (iPhone + Mac)

_Run top to bottom. The order matters: you configure and **verify** everything yourself first, and your partner sets the lock **last** — so you're never locked into a half-broken setup._

**You'll need:** ~30–40 min, your iPhone, your Mac, and your **accountability partner** present (in person or on a call) for Phase 4.

---

## Phase 0 — Decide the lock model (do this first)

Pick how the recovery path leaves your hands. Without this, the whole thing is a speed bump.

- [ ] **Option A (recommended): Partner holds it.** Your partner sets the Screen Time passcode and keeps it. Best if they're willing to be the Family Sharing organizer (closes the factory-reset hole).
- [ ] **Option B: Locked secondary Apple ID.** You create a second Apple ID, apply restrictions under it, hand the password to your partner / store it somewhere you can't reach. Use only if no one can hold a passcode for you.
- [ ] Confirm **Find My iPhone is ON** (Settings → your name → Find My → Find My iPhone). _This blocks USB passcode-remover tools. Leave it on._

---

## Phase 1 — iPhone: kill the website + app (you do this)

Settings → **Screen Time**. (If it says "Turn on Screen Time," do that first — no passcode yet.)

- [ ] **Block the website (the part everyone skips):**
  Screen Time → **Content & Privacy Restrictions** → toggle **ON** → **Content Restrictions** → **Web Content** → select **Limit Adult Websites** → under **NEVER ALLOW**, tap **Add Website** → enter `instagram.com`
  - [ ] Also add: `www.instagram.com`
- [ ] **Block reinstalling the app:**
  Back to Content & Privacy → **iTunes & App Store Purchases** → **Installing Apps** → set to **Don't Allow**
  _(This removes the App Store and stops redownloads. Note: turn it back to Allow temporarily any time you legitimately need a new app — your partner will hold that power after Phase 4.)_
- [ ] Make sure the **Instagram app is deleted** (you've done this).
- [ ] **Verify now (before locking):**
  - [ ] Open Safari → go to `instagram.com` → should be **blocked**.
  - [ ] Open Chrome (if installed) → `instagram.com` → should **also** be blocked (Never Allow covers all browsers).
  - [ ] Confirm the App Store icon is gone.

---

## Phase 2 — Mac: same block (you do this)

 Apple menu → **System Settings** → **Screen Time** (sign in with the same Apple ID).

- [ ] **Content & Privacy** → turn **ON** → **Content Restrictions** → **Web Content** → **Limit Adult Websites** → **Customize** → under **Restricted**, add `instagram.com`
- [ ] **Belt-and-suspenders on the Mac** (optional but cheap):
  - [ ] **hosts file** — open Terminal, run `sudo nano /etc/hosts`, add these two lines, save (Ctrl+O, Enter, Ctrl+X):
    ```
    127.0.0.1 instagram.com
    127.0.0.1 www.instagram.com
    ```
  - [ ] **uBlock Origin Lite** — install from the App Store, enable in Safari → Settings → Extensions, add `instagram.com` to its block rules. (Screen Time can lock this on.)
- [ ] **Verify:** Safari + Chrome → `instagram.com` → blocked.

---

## Phase 3 — NextDNS redundant layer (you do this, optional but recommended)

Catches the app/API traffic and adds a second net. Free tier = 300k queries/month.

- [ ] Create a free account at **nextdns.io** → you get a config ID.
- [ ] In your NextDNS config → **Denylist**, add a community Instagram blocklist (web + API + CDN domains) — or at minimum: `instagram.com`, `cdninstagram.com`, `api.instagram.com`, `graph.instagram.com`, `platform.instagram.com`.
- [ ] Turn ON **Settings → Block Bypass methods**.
- [ ] **iPhone:** install the NextDNS **.mobileconfig** profile (their site walks you through it) → works on cellular too.
- [ ] **Mac:** install the NextDNS profile or use their app.
- [ ] **Verify:** with wifi off (cellular only), try `instagram.com` → still blocked.
- [ ] _Reality check: this layer is toggleable in Settings → DNS. It's friction, not a wall. Screen Time (Phase 1) is the real lock._

---

## Phase 4 — Hand over the keys (partner does this, LAST)

Only do this once Phases 1–2 are **verified working**. This is the step that makes it self-binding.

**If Option A (partner holds passcode):**
- [ ] _(Best)_ Set up **Family Sharing** with your partner as the **organizer** (Settings → your name → Family Sharing). This binds the passcode to *their* Apple ID, closing the factory-reset/restore escape.
- [ ] On your **iPhone**: Screen Time → **Lock Screen Time Settings** → **your partner types a 4-digit passcode you never see.**
- [ ] When iOS asks for an **Apple ID to recover the passcode**, use **your partner's Apple ID** (not yours). _This is the critical detail — otherwise you can reset it yourself via "Forgot Passcode?"._
- [ ] Repeat on the **Mac**: System Settings → Screen Time → **Lock Screen Time Settings** → partner enters the same passcode.

**If Option B (secondary Apple ID):**
- [ ] Apply the Screen Time passcode, set the recovery to the **secondary Apple ID**, then hand that Apple ID's password to your partner or lock it away.

- [ ] **Final verification (you, without the passcode):**
  - [ ] Try to change the Web Content setting → it should demand the passcode you don't have. ✅
  - [ ] Try `instagram.com` on iPhone Safari, iPhone Chrome, Mac Safari → all blocked. ✅
  - [ ] Try to reinstall Instagram → App Store gone / blocked. ✅

---

## Standing rules (so it actually lasts)

- **Keep Find My ON** on the iPhone — it's what defeats USB unlock tools.
- **Don't memorize the passcode.** If you ever learn it, have your partner change it.
- **Need a new app legitimately?** Ask your partner to flip "Installing Apps → Allow" temporarily, then back.
- **Watch the NextDNS 300k/month cap** — if you blow past it, filtering pauses till reset (Screen Time still holds).
- **If you want it truly unbreakable:** the nuclear free option is supervising the iPhone via Apple Configurator (wipes the phone, but makes DNS + app blocks unremovable). Only if Phases 1–4 prove not enough.

---

## The honest gap

This setup makes Instagram genuinely hard to reach on impulse — the website is VPN-proof via Screen Time, the app can't be reinstalled, and the off switch lives with your partner. The one thing no free tool fixes: **if you talk your partner into the passcode, it's over.** The lock is only as strong as your agreement with them. Pick someone who'll say no.

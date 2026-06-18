# Instagram Block — Granular Step-by-Step Guide

_Every line is one action. The ☐ is for you to check off. After most steps there's an **→ You should see:** so you know it worked before moving on._

**Before you start:**
- Have your iPhone and Mac in front of you.
- Have your **accountability partner** reachable for **PART F** (the final lock). Do Parts A–E alone.
- Total time: ~30–45 min.

> ⚠️ **Golden rule:** Do NOT set any passcode until PART F. You configure and test everything first. If you lock too early and something's wrong, you'll be stuck.

---

## PART A — Prep (5 min)

**A1. Confirm Find My is on (this is what blocks USB unlock tools).**
- ☐ A1.1 — Open **Settings** on iPhone.
- ☐ A1.2 — Tap your **name** at the very top.
- ☐ A1.3 — Tap **Find My**.
- ☐ A1.4 — Tap **Find My iPhone**.
- ☐ A1.5 — Confirm the top toggle **Find My iPhone** is **green/ON**. If off, turn it on.
  → You should see: "Find My iPhone: On".

**A2. Confirm the Instagram app is deleted.**
- ☐ A2.1 — Swipe down on the home screen to open search, type "Instagram".
- ☐ A2.2 — If the app appears, press and hold it → **Remove App** → **Delete App** → **Delete**.
  → You should see: no Instagram app remaining (search shows only the App Store result).

---

## PART B — iPhone: block the website (10 min) ⭐ most important part

**B1. Open Screen Time.**
- ☐ B1.1 — Open **Settings**.
- ☐ B1.2 — Scroll down, tap **Screen Time** (hourglass icon).
- ☐ B1.3 — If you see a button **Turn On Screen Time**, tap it, then tap **Continue** / **This is My iPhone**. (Do NOT set a passcode if asked — skip/cancel that for now.)
  → You should see: the Screen Time menu with options like "App Limits," "Content & Privacy Restrictions."

**B2. Turn on restrictions.**
- ☐ B2.1 — Tap **Content & Privacy Restrictions**.
- ☐ B2.2 — Tap the top toggle **Content & Privacy Restrictions** to **green/ON**.
  → You should see: the list below becomes tappable (no longer greyed out).

**B3. Open Web Content.**
- ☐ B3.1 — Tap **Content Restrictions**.
- ☐ B3.2 — Tap **Web Content**.
  → You should see: three options — Unrestricted Access / Limit Adult Websites / Allowed Websites Only.

**B4. Switch to Limit Adult Websites (this unlocks the Never Allow list).**
- ☐ B4.1 — Tap **Limit Adult Websites**.
  → You should see: a blue checkmark next to it, and two new sections appear: **ALWAYS ALLOW** and **NEVER ALLOW**.

**B5. Add Instagram to Never Allow.**
- ☐ B5.1 — Under **NEVER ALLOW**, tap **Add Website**.
- ☐ B5.2 — Type: `https://www.instagram.com`
- ☐ B5.3 — Tap **Done** (keyboard) / back arrow to save.
  → You should see: `www.instagram.com` listed under NEVER ALLOW.
- ☐ B5.4 — Tap **Add Website** again.
- ☐ B5.5 — Type: `https://instagram.com`
- ☐ B5.6 — Tap **Done** to save.
  → You should see: both entries under NEVER ALLOW.

**B6. TEST IT NOW (before anything is locked).**
- ☐ B6.1 — Open **Safari**, type `instagram.com`, go.
  → You should see: "You cannot browse this page" / a blocked/restricted message. ✅
- ☐ B6.2 — If you have **Chrome**, open it, try `instagram.com`.
  → You should see: also blocked (the block covers every browser). ✅
- ☐ B6.3 — If it is NOT blocked: go back to B4–B5 and re-check the entries are under NEVER ALLOW (not Always Allow).

---

## PART C — iPhone: block the app from opening (5 min)

_This uses an **App Limit on the Social category** — it blocks Instagram (and other algorithmic feeds) from opening, survives reinstalls, and touches nothing else on your phone. No age restriction, no removing the App Store._

**C1. Create the limit.**
- ☐ C1.1 — Go to **Settings → Screen Time → App Limits**.
- ☐ C1.2 — Tap **Add Limit**.
- ☐ C1.3 — Tick the **Social** category. _(To target only Instagram instead, expand the category and tick just Instagram — but the whole-category limit is recommended: it auto-catches reinstalls and covers the other feeds you're quitting too.)_
- ☐ C1.4 — Tap **Next** (top-right).
- ☐ C1.5 — Set the time to **1 minute**. _(1 min is the minimum Screen Time allows — you'll get a ~60-second sliver per day, then it walls off. Combined with the website block, this is effectively a full block.)_
- ☐ C1.6 — Turn **ON** the **Block at End of Limit** toggle. ← this is what makes it a hard wall, not a nudge.
- ☐ C1.7 — Tap **Add**.
  → You should see: the Social limit listed under App Limits.

**C2. Verify the block.**
- ☐ C2.1 — If Instagram is installed, open it and use it for a minute.
  → You should see: a **"Limit Reached"** / time's-up screen. Once Screen Time is locked (Part F), there is **no "Ignore Limit" escape**. ✅
  - _Note: without removing the App Store, Instagram can still be **downloaded** — but the category limit blocks it from **opening**, even after a reinstall. The website stays blocked separately via Part B._
  - _Optional stricter add-on: you can also block redownloads entirely via Content & Privacy Restrictions → iTunes & App Store Purchases → Installing Apps → Don't Allow, but that removes the App Store icon for everything. Skip unless you want it._

---

## PART D — Mac: block the website (8 min)

**D1. Open Screen Time on the Mac.**
- ☐ D1.1 — Click the **Apple menu** () top-left → **System Settings**.
- ☐ D1.2 — In the sidebar, click **Screen Time**. (Sign in with the **same Apple ID** as your iPhone if prompted.)
- ☐ D1.3 — If Screen Time is off, click **Turn On Screen Time**. (No passcode yet.)

**D2. Block instagram.com.**
- ☐ D2.1 — Click **Content & Privacy**.
- ☐ D2.2 — Toggle **Content & Privacy** to **ON**.
- ☐ D2.3 — Click **Content Restrictions** (or **Store, App, & Web** depending on version) → find **Web Content** / **Access to Web Content**.
- ☐ D2.4 — Set it to **Limit Adult Websites**.
- ☐ D2.5 — Click **Customize…** (or the **Restricted** "+" button).
- ☐ D2.6 — Under **Restricted**, click **+** and add `https://www.instagram.com`, then `https://instagram.com`.
- ☐ D2.7 — Click **Done**.
  → You should see: both URLs in the Restricted list.

**D3. Test on the Mac.**
- ☐ D3.1 — Open **Safari** → `instagram.com`.
  → You should see: blocked. ✅
- ☐ D3.2 — Open **Chrome** (if installed) → `instagram.com`.
  → You should see: blocked. ✅

**D4. (Optional) hosts-file backstop on the Mac.**
- ☐ D4.1 — Open **Terminal** (Cmd+Space, type "Terminal", Enter).
- ☐ D4.2 — Type: `sudo nano /etc/hosts` and press Enter. Enter your Mac password (cursor won't move — that's normal), press Enter.
- ☐ D4.3 — Use arrow keys to go to the bottom, add these two lines:
  ```
  127.0.0.1 instagram.com
  127.0.0.1 www.instagram.com
  ```
- ☐ D4.4 — Save: press **Ctrl+O**, then **Enter**. Exit: **Ctrl+X**.
- ☐ D4.5 — Type: `sudo dscacheutil -flushcache` and press Enter (clears DNS cache).
  → You should see: back to the normal Terminal prompt, no errors.

---

## PART E — NextDNS redundant layer (10 min, optional but recommended)

_Catches app/API traffic and works on cellular. Free up to 300k queries/month._

**E1. Create the account.**
- ☐ E1.1 — On the Mac, go to **nextdns.io**, click **Try it now / Sign up** (free).
- ☐ E1.2 — Note your **Configuration ID** (a short code shown on the Setup page).

**E2. Add Instagram to the denylist.**
- ☐ E2.1 — In the NextDNS dashboard, click the **Denylist** tab.
- ☐ E2.2 — Add each of these (click Add after each):
  `instagram.com`, `www.instagram.com`, `cdninstagram.com`, `api.instagram.com`, `graph.instagram.com`, `platform.instagram.com`
- ☐ E2.3 — Click the **Settings** tab → turn ON **Block bypass methods**.

**E3. Install on iPhone (works on cellular).**
- ☐ E3.1 — On the iPhone, open the NextDNS Setup page → tap the iOS **"Download the configuration profile"** (.mobileconfig) option.
- ☐ E3.2 — Tap **Allow** when it says "This website is trying to download a configuration profile."
- ☐ E3.3 — Open **Settings** → tap **Profile Downloaded** (near the top) → **Install** (top-right) → enter your iPhone unlock passcode → **Install** → **Install**.
  → You should see: NextDNS listed under Settings → General → VPN & Device Management.

**E4. Install on Mac.**
- ☐ E4.1 — On the NextDNS Setup page on the Mac, follow the **Apple → macOS** profile download, then **System Settings → General → Device Management → Install**. (Or install their menu-bar app.)

**E5. Test on cellular.**
- ☐ E5.1 — On iPhone, turn **Wi-Fi OFF** (so you're on cellular only).
- ☐ E5.2 — Open Safari → `instagram.com`.
  → You should see: still blocked. ✅
- ☐ E5.3 — Turn Wi-Fi back on.

---

## PART F — Hand the keys to your partner (5 min) 🔒 do this LAST

_Only start this once Parts B, C, D all tested as blocked. This is the step that makes it self-binding. Your partner does the typing._

**F1. (Recommended) Set up Family Sharing with partner as organizer** — closes the factory-reset escape.
- ☐ F1.1 — Settings → tap your **name** → **Family Sharing** → **Set Up Family** and have your **partner** be the organizer, OR join their existing family. (If this is too much, skip to F2 — you still get a strong lock, just without the reset-proofing.)

**F2. Lock Screen Time on the iPhone — PARTNER TYPES THE CODE.**
- ☐ F2.1 — Settings → **Screen Time** → scroll down → tap **Lock Screen Time Settings**.
- ☐ F2.2 — **Hand the phone to your partner.** They enter a 4-digit code. **You look away — you must not see it.**
- ☐ F2.3 — They re-enter the same code to confirm.
- ☐ F2.4 — When it asks for **Screen Time Passcode Recovery** (an Apple ID), your partner enters **THEIR Apple ID** — or tap **Cancel** to skip if using Family Sharing.
  → ⚠️ Do NOT enter *your* Apple ID here — that would let you reset the code yourself via "Forgot Passcode?".

**F3. Lock Screen Time on the Mac.**
- ☐ F3.1 — System Settings → **Screen Time** → scroll to **Lock Screen Time Settings** → turn on → **partner enters the same code**.

**F4. FINAL TEST — you, without the code.**
- ☐ F4.1 — On iPhone: Settings → Screen Time → Content & Privacy → try to change **Web Content** back to "Unrestricted."
  → You should see: it **demands the passcode you don't have**. ✅
- ☐ F4.2 — Safari → `instagram.com` → blocked. ✅
- ☐ F4.3 — Chrome → `instagram.com` → blocked. ✅
- ☐ F4.4 — Open Instagram (or its App Limit in Settings) → you can't raise/remove the limit without the code, and the app hits the "Limit Reached" wall. ✅
- ☐ F4.5 — Mac Safari → `instagram.com` → blocked. ✅

If all five show ✅ — **you're done. The off switch now lives with your partner.**

---

## Quick troubleshooting

- **Website not blocked after B5?** The entries are probably under "Always Allow" instead of "Never Allow." Re-do B4–B5.
- **Block works in Safari but not Chrome?** Make sure Content & Privacy Restrictions (B2.2) is actually ON — the Web Content filter is system-wide and covers all browsers only when restrictions are on.
- **Forgot to test before locking and something's broken?** Your partner unlocks with the code, you fix it, they re-lock. (This is exactly why F comes last.)
- **NextDNS stopped filtering?** You may have hit the 300k/month free cap — it resets monthly; Screen Time still holds regardless.

---

## The one honest limitation

Everything here is solid **except** the human link: if you persuade your partner to give you the code, it's all undone in 30 seconds. The technology is genuinely hard to beat now — the website block even survives a VPN. The weak point is the conversation at midnight. Choose a partner who will say **no**.

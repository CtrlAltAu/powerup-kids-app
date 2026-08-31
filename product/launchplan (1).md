# PowerUp Kids — Launch Plan & Next Steps

*Last updated: 31 Aug 2026*

## How to use this document

This is the full to-do list for getting PowerUp Kids from "working app my family tests" to "live on the App Store and Google Play, with a real website." Attach this file to a new Claude conversation whenever you're ready to work on the next item, and say something like *"Let's work on the privacy policy — see launch-plan.md"*. A fresh Claude session will also have `CLAUDE.md` (in the same repo) for the technical/architecture context; this document is the business/launch side.

Everything below the "Already done" section is still to do, roughly in the order I'd tackle it. Items in the same numbered phase can often be worked in parallel with other phases — dependencies are called out explicitly.

---

## Already done

- The app itself: missions, Epic Feats gamification, a 1000-level curve, Daily Check-In, Transition Cue, Calm Celebrations, three mini-games (Bubble Burst, Sky Hop, Pattern Match), family + worldwide leaderboards, PDF progress reports, multi-kid support, free/premium feature gating.
- Live and testable: **https://powerup-kids-app.web.app/**, code on GitHub at **github.com/CtrlAltAu/powerup-kids-app**, a working redeploy workflow.
- A full round of real-world bug fixes from family testing (Daily Check-In, level-up messaging, icon equipping, Transition Cue).
- A UI/UX + ADHD-autism design audit, with the findings already shipped (grouped parent menu, calmer banner behavior, Calm Celebrations toggle).
- `CLAUDE.md` — project memory so any AI coding session (this one, or Claude Code in VS Code) onboards instantly.
- Claude Code installed and working in VS Code for hands-on local development.
- A real privacy policy, live at `product/privacy.html` and linked from the Parent Menu (🔒 Privacy Policy).
- A landing page, drafted and published as a private preview — real screenshots from the live app, cited research on why the approach works, and a free-vs-Premium breakdown that doesn't invent a price. Needs Tim's review before it's shared publicly.

---

## To do, in suggested order

### 1. Privacy policy + Kids Category / COPPA compliance — ✅ drafted
**Status: done, pending two small follow-ups.**

`product/privacy.html` is written and linked from the Parent Menu (🔒 Privacy Policy button), grounded in the app's real data flows (`product/FIREBASE_SETUP.md` security rules) and the specific Apple guidelines it needs to satisfy (`product/APPLE_KIDS_CATEGORY_NOTES.md` — 1.2, 1.3, 5.1.4(b)). It's deployed the moment you push `app.html`/`index.html`/`privacy.html` and redeploy — it'll be live at `https://powerup-kids-app.web.app/privacy.html`.

Still to do:
- **Set up a real contact inbox.** The policy currently points to a placeholder (`privacy@powerupkids.app`) — swap in an address you actually check before this goes live, or set up forwarding for that one.
- **Get a lawyer's eyes on it before submission.** I drafted this to a solid, well-researched standard, grounded in what the app actually does, but a privacy policy is a legal document touching children's data — that's worth a real review, not just an AI one.
- Before you submit to app review, remove the "🧪 Dev: Toggle Premium (testing only)" button from the Parent Menu — it's a paywall bypass that shouldn't ship.

### 2. Landing page (marketing website) — ✅ drafted
**Status: built and published as a private preview, waiting on your review.**

A single-page site is live at the artifact link I sent you in chat — private until you choose to share it. It includes: a plain-language explanation of what the app does and who it's for, a "why it works" section with real citations (Indiana Resource Center for Autism on transitions, CDC on behavior-therapy-first for ADHD, Tiimo/Aspect on sensory-aware design, plus an explicit honesty note that the visual-schedule research is genuinely mixed — not oversold), three feature sections built around real screenshots of the live app (Missions/Epic Feats, the Progress Report, Sky Hop), a free-vs-Premium comparison that doesn't invent a price (Premium's pricing isn't set yet — see item 5), a short FAQ, and a "notify me" mailto link instead of a fake signup form.

Still to do:
- **Review it and tell me what to change** — tone, what's emphasized, anything that doesn't sound like you.
- **Decide on hosting.** The artifact link works as-is and can be shared right now. A custom domain (~$10-15/year) is still a nice-to-have if you want `powerupkids.app` or similar instead of a claude.ai link — not required to launch.
- **Set up the same contact inbox** as item 1 — the footer currently points to `hello@powerupkids.app`, also a placeholder.
- Turn on the real App Store / Google Play links once those are live (see items 4-7).

### 3. Developer accounts
**Costs money. Start this now — the Google Play side has a hard multi-week minimum before you can go live, and it's easy to underestimate.**

#### Apple Developer Program — $99/year
1. Go to developer.apple.com and sign in with (or create) an Apple Account with two-factor authentication turned on.
2. Start enrollment as an **Individual** (simplest — no D-U-N-S number, no company paperwork) or an **Organization** if you want "PowerUp Kids" (rather than your own legal name) to show as the seller on the App Store listing.
   - Individual needs: your legal name (no nicknames — this alone causes delays), a verified email, phone, and a real street address (no P.O. boxes).
   - Organization additionally needs: a D-U-N-S number for the legal entity (free from Dun & Bradstreet, but can take 5+ business days to issue if you don't already have one), proof you have binding authority to act for it, a work email on the organization's domain, and a live public website for that organization.
3. Agree to the Apple Developer Program License Agreement and pay the $99/year fee (nonprofits, schools, and government entities can apply for a fee waiver — doesn't apply here).
4. Apple doesn't publish a fixed turnaround time; it's usually quick for Individual accounts, but can stretch to days if anything about the submitted identity needs manual review.
   - **The practical decision**: if you want the App Store to say "PowerUp Kids" as the seller rather than your own name, you need the Organization path and the D-U-N-S number — worth deciding now since the D-U-N-S lookup/issuance is the slowest part and can run in parallel with everything else.

#### Google Play Console — $25 one-time
1. Go to play.google.com/console, sign in with a Google account, and pay the one-time $25 registration fee.
2. Complete identity verification (personal or organization, similar idea to Apple's).
3. **The part that actually drives your timeline**: any personal Google Play account created after Nov 13, 2023 (which yours will be) must run a **closed test with at least 12 testers, each opted in continuously for 14 consecutive days**, before Google will grant production (public) access. A tester who opts in and later opts out doesn't count, and gaps reset that tester's clock — so this needs 12 real people who'll stay opted into the test build for two straight weeks. After that's satisfied, you apply for production access from the Play Console dashboard and Google reviews it within about 7 days.
   - **In practice: budget 3+ weeks minimum from "app is ready to test" to "live on Google Play"** — 14 days of continuous testing plus up to a week of review, and that's the optimistic case where you already have 12 people lined up on day one.
   - Start recruiting those 12 testers early — friends, family, other parents of kids with ADHD/autism you know, or communities where this app is genuinely relevant. They just need to install the closed-test build and stay opted in; they don't need to do anything else.

How to do it: this is mostly you clicking through Apple's and Google's own signup flows and paying the fees — I can walk you through any specific screen when you're ready, but I can't create these accounts for you (they require your personal or organization identity). Given the Google testing clock, **this is the item most worth starting before the Capacitor wrapper (item 4) is even finished** — you can register both accounts and start recruiting testers while that work is still in progress, then have a real build ready to hand your 12 testers the moment it's testable.

### 4. Capacitor native wrapper
**Needs developer accounts started (item 3) before you can test on a real device via each store's tools, though local development can start earlier.**

What it is: packaging the web app you already have into an actual iOS and Android app shell, with real native features (push notifications, haptics, sound) wired in through Capacitor plugins.

How to do it: this is genuine native-mobile-development territory. I can build the Capacitor wrapper itself and get it running, but testing on real iOS hardware and building for App Store submission needs either a Mac or a cloud Mac build service — Xcode Cloud's free tier (25 hours/month) is enough for occasional builds, so you don't need to buy a Mac. Android doesn't need a Mac at all. This is the more technically involved phase — if you want to move faster than we can manage async through chat sessions, it's reasonable to consider bringing in a contractor with Capacitor/iOS experience for just this phase, though it's learnable if you'd rather we work through it together.

### 5. Real payments (Apple StoreKit + Google Play Billing)
**Needs the Capacitor wrapper (item 4) and developer accounts (item 3) — payments don't work in a plain web page, only inside the native app shell.**

What it is: replacing the current dev-only fake premium toggle with real, working subscriptions through Apple and Google's own payment systems (required — you can't process payments any other way in an app on either store).

How to do it: set up the subscription product (price, trial length if any) in App Store Connect and Google Play Console, then wire up each platform's Capacitor billing plugin to unlock Premium on a real purchase. This is the other technically involved phase, same considerations as item 4 about pacing/possibly bringing in help.

### 6. Store assets & build pipeline
**Needs items 3-5 substantially done first.**

- App icon (multiple required sizes for each store).
- Screenshots for the various device sizes each store requires.
- Store description and keywords — the landing page copy from item 2 is a great starting point for this.
- Confirm the build pipeline (Xcode Cloud or Codemagic) is producing a submittable build.

### 7. Submit & launch
**The last step — needs everything above done.**

Final QA pass across both platforms, submit to App Review and Google Play review, respond to any feedback they come back with, and go live. Turn on the real App Store / Google Play links on the landing page (item 2) once this is done.

---

## Costs, all in

| Item | Cost |
|---|---|
| Apple Developer Program | $99/year |
| Google Play Console | $25 one-time |
| iOS build pipeline (Xcode Cloud free tier) | $0 |
| Custom domain for the landing page (optional) | ~$10-15/year |
| **Estimated total to get to launch** | **~$125-140** |

Firebase itself stays on the free Spark plan through all of this unless usage grows a lot faster than expected — not a near-term concern.

---

## What I'd genuinely suggest doing first

Items 1 and 2 (privacy policy, landing page) are drafted — your move now is review and a couple of small decisions (contact email, custom domain or not), not new work. That makes item 3 (developer accounts) the thing to prioritize next, and specifically to start it *now* rather than "in parallel, no rush": Google Play's 12-testers/14-consecutive-days requirement is a hard multi-week clock that only starts once you have a testable build, so registering both accounts and lining up your 12 testers early directly shortens the real-world path to launch. Items 4 and 5 (Capacitor, payments) are the real technical core of the remaining work — that's the conversation worth having properly once the accounts are moving, since it's where you'll decide how hands-on vs. how much outside help you want.

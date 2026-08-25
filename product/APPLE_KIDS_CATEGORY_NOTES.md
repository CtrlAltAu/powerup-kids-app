# Apple App Store Kids Category — leaderboard compliance notes

You asked me to make sure the worldwide leaderboard is "solid" for Apple. Here's
what I found in Apple's actual App Review Guidelines, what it meant for the
leaderboard design, and what's still outstanding before submission.

## What Apple's guidelines actually say

**Guideline 1.2 (User-Generated Content):** any app with content one user
creates that another user can see must include content filtering, a way to
report objectionable content, the ability to block abusive users, and
published contact info. It specifically calls out "random or anonymous chat"
and similar unmoderated interactions as things that "do not belong on the App
Store."

**Guideline 1.3 (Kids Category):** no third-party analytics or advertising
(with narrow exceptions neither of which apply here since we don't use either).
Links out of the app, purchases, or other distractions must sit behind a
parental gate.

**Guideline 5.1.4(b):** any app that collects or can share personal
information from a minor — name, address, email, location, chat, or
persistent identifiers combined with any of those — needs a privacy policy and
must comply with children's privacy law (COPPA, since this is a US-facing app).

## The gap this exposed, and the fix

A worldwide leaderboard is, structurally, exactly the kind of "content one
user creates that another user can see" that Guideline 1.2 is about — and the
free-text nickname field I originally built (a parent could type anything, up
to 24 characters) would have counted as unmoderated user-generated content
visible to strangers worldwide. That's a real rejection risk in the Kids
Category, and building full moderation infrastructure (profanity filtering,
a report button, a block button, published abuse contact info) for a "fun
bonus leaderboard" felt like the wrong amount of engineering for what this is.

So I removed free text entirely. **A parent can only pick a nickname from a
set of pre-generated combinations** (adjective + animal + number, e.g. "Swift
Dragon 482") — there's a "show more options" button to regenerate a fresh set,
but nothing is ever typed. Since there's no way for arbitrary text to reach
the leaderboard, there's nothing to moderate, and Guideline 1.2's
filtering/reporting/blocking requirements shouldn't apply in the first place.

Combined with what was already true — opt-in per kid (off by default), no
real name/email/DOB/location ever transmitted, no chat or messaging of any
kind, no way to identify or contact a specific other child, and no
third-party analytics or ads touching any of it — I think this puts the
leaderboard in a defensible spot for Kids Category review. It's the same
shape as leaderboards in plenty of approved kids' games: a score, a made-up
tag, nothing else.

## What's still worth doing before you submit (not done yet)

- **Privacy policy.** 5.1.4(b) requires one regardless of the leaderboard —
  this app already needs a privacy policy for the account/email data alone.
  Still open (this is your existing task #7).
- **Parental gate.** Not needed yet — there are currently no external links
  and no working purchase flow in the app (checked: no `href`/`target=_blank`
  anywhere, and the paywall is still a placeholder). Once real StoreKit
  billing goes in (task #6), that purchase flow will need to sit behind a
  parental gate (Apple has a documented pattern for this — a simple "what is
  9 x 3?" style gate is what most kids apps use).
- **COPPA specifically**, not just Apple's guidelines — Apple's rules are a
  reasonable proxy but not a legal substitute; worth a real look (or a
  lawyer's) once you're closer to submission, especially around the
  leaderboard and any account data tied to a child.
- I can't test actual App Review behavior from here — this is my best
  grounded reading of the published guidelines, not a guarantee of approval.

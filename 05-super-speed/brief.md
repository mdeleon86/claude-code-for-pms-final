# Back in Rotation: one-page brief

**Rook Dispatch** · Draft for Helen Achebe · 8 October 2026 · *Fictional course scenario* · Prototype: `prototype.html`

## The problem
Once a responder goes quiet, the only thing that brings them back is being asked, and being quiet is exactly what stops them being asked.

**Evidence:**
- **After 4.2, four responders went from about 12 offers a week to 0–1 and haven't come back.** Meteor Mite missed 12 of his 17 offers from the week of 10 Aug to 31 Aug. (`callout-history.csv`)
- **The code makes it stick.** Every miss costs points, including timeouts and offers the phone may never have shown. Only accepting earns points, and nothing heals with time. (`history.py`, `offer.py`)
- **Nobody can see why.**
  - Kip: "Mite thinks Mite's been forgotten."
  - About 10 tickets ask "is my account broken?"
  - The busiest four responders' share of offers rose from about 32% to 45%.

## Who it's for
- **Primary:** the quiet responder, like Meteor Mite.
- **Secondary:** their handler, like Kip.

## What we'd change

### Engineering changes (proposed, not built)
1. **Scores heal over time toward the middle (0.5),** even with no offers (Wen's 2019 TODO). A deliberate "no" still costs the full amount.
2. **A timeout costs less than a deliberate "no."** The code already tells them apart (`offer.py`).

Neither change puts anyone ahead of a closer or better-matched responder.

**A score moving toward 0.5 is not the same as getting offers back.** Responders who say yes often score near 1.0, so a healed responder can still rank behind them, by roughly 9 minutes of travel time. Whether healed scores bring offers back is what the shadow test must show.

### What Kip and Mite would see (prototype screens only)
**Kip:**
- **A "gone quiet" card:** *"3 offers in the last 2 weeks of data (to 31 Aug); before 4.2 about 11 a week; missed 12 of 17 since the week of 10 Aug."*
- **Mite's score moving back toward the middle.**
- **A live-callout alert** with a different sound for each responder. In the interviews, 3 of 4 handlers said they can't tell when a callout is live.

**Mite:**
- **"You're still active,"** why it's quiet, and his score healing.
- **"Ready now,"** which tells Kip he's by the phone but never moves him up the list.

**If the score heals but offers don't come back,** the card says so and points Kip to coverage area.

## What the prototype's simulations show

**Evidence** (each responder's real history, replayed under the code's real rules):
- **With both changes, the four's scores on 31 Aug rise from 0.00–0.12 to about 0.44–0.49.** Healing alone gives about 0.39–0.46.
- **In our runs, healing does most of the work,** because it keeps going after the offers stop.
- **Ashgrove and Halfmoon stay at 1.00 under every rule.** They still said yes to about 6–7 in 10 offers, so their roughly 25% drop in offers isn't coming from the score.

**Assumptions:**
- The data doesn't say which misses were timeouts, so a slider guesses.
- Everyone starts at 0.5 on 29 June.
- The replay keeps their real offers fixed, so it shows **scores, not offers**.
- The healing speed and the timeout discount are placeholders.

**Recommendations:**
- **Lead with healing.** Don't set the timeout discount until Ravi can separate timeouts from real "no"s. Its effect depends entirely on that split.
- **Add a safeguard against rewarding refusals.** In the Decliner Test, someone who refuses every offer gets an average score of 0.30 if scores heal halfway in 1 day, 0.07 at 3 days and 0.05 at 5 days (today's rules: 0.00). Keep healing at 3 days or slower, or pause it after a deliberate "no."
- **Look at Ashgrove and Halfmoon separately.** Their drop is likely the 4.2 distance weighting, but that's unconfirmed.

## What it deliberately doesn't do
- **No line-jumping, guaranteed offers or tie-breaker.**
- **It doesn't undo 4.2,** and it isn't a handler setting.
- **It doesn't wipe history.**
- **It doesn't touch the Responder Availability Record,** which Supply uses.
- **It doesn't store or infer anyone's identity.**
- **It doesn't fix late phone delivery.**

## How we'll test it
1. **A shadow run (about 2 weeks):** log how healed scores would change rankings, without acting on them.
2. **Then turn it on,** and compare before and after, excluding the release week.

**What we'll measure:**
- **Recovery:** the four's weekly offers against their own past levels, and days until their first accepted job.
- **Guardrails** (these must not get worse):
  - Time-to-accept.
  - People asked per incident.
  - Coverage gaps.
  - How often the first person asked wasn't the closest qualified responder.
  - *Only timeouts hold a job up (up to 60 seconds). A deliberate "no" passes it on right away.*
- **Spread:** the busiest four's share of offers moving back toward 32%.

## Open questions
- **Design (not decided):** should healing stop at 0.5, or aim for a typical responder's score? We need Wen's score data first.
- **Ravi:** what `pings_sent` counts (it conflicts with 18 of 25 tickets), a split of timeouts vs. real "no"s, and per-offer logs.
- **Wen:** travel times for the four; whether live matches the repo; whether scores reset at the deploy; the healing speed and timeout discount.
- **Sofia:** where the card, the alert and "Ready now" live in the console and the app.

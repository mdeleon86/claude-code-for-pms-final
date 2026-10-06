# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch** (started 31 Aug 2026), replacing Priya
Raghunathan, who left 21 Aug with no overlap. Sources: `00-rook/`.

### Company and products
- Rook sells software to **independently operating masked responders** and
  their **handlers** (and **quartermasters**, on Supply). It doesn't employ
  responders. Pricing is per active responder, with monthly 4.x releases.
- **Confidentiality is contractual:** no responder legal identities, ever.
  Never design for one or try to work out who anyone is (Security Policy 4.1).
- **Dispatch (mine):** incident → rank available responders → offer to the top
  one's phone → decline or timeout passes it down the list → acceptance assigns
  it. Handlers use the web console; responders use mobile. Routing config ships
  in the release and can't be tuned at runtime.
- **Metrics:** **acceptance rate** (headline, weekly aggregate),
  time-to-accept, **coverage gap** (nobody *could* go, as opposed to nobody *would*).
- **Supply (coupled):** requisitions, maintenance and failure reports. It reads
  Dispatch's **Responder Availability Record** to book maintenance into
  low-callout windows. **Flag any Dispatch change that touches availability**
  (`availability.py` says to warn the Supply team first).

### People
| Name | Role | Go to for |
|---|---|---|
| Helen Achebe | Director of Product (my manager) | Roadmap and commitments |
| Marcus Oyelaran | Eng Manager, Dispatch | Straight answers; start here |
| Wen Li | Staff Engineer | Built routing; only real source on ranking |
| Sofia Marino | Product Designer | Console and phone app; ran the interviews |
| Nadia Hoffmann | Support Lead | Ticket volume and themes |
| Ravi Menon | Data Analyst (#data) | Official weekly numbers (not Marcus, despite Priya's note) |

### Vocabulary
- **Callout offer / ping:** one callout pushed to one responder's phone.
- **Callout timeout** ("ping timeout"): how long an offer stays live (now 60s).
- **Routing priority:** the ranking score: proximity (0 beyond 45 min),
  capability and recent acceptance.
- **"The change to who gets pinged":** the 4.2 weight rebalance.
- **Capability tags:** flight, structural-entry, hazmat-tolerant, cold-weather,
  aquatic, crowd-management, de-escalation.

### Where things stand
**Release 4.2 (12 Aug)** made the routing changes below and added console filter
persistence (expect cosmetic tickets). Since then tickets are about 3× normal:
⅔ "phone never goes off," ⅓ "gone before I could answer."
- **Priya's "it's seasonal, don't revert" is a hypothesis.** No source supports it.
- **Availability Confidence** was committed for 4.2 but didn't ship. Confirm
  with Helen which squeezed items remain Q3 commitments.
- **Nobody has written down how routing works.** That's on me.
- The team is waiting for my read before drawing conclusions.

### Findings so far (sessions 1–4, not yet verified with the team)
**Data** (`data/callout-history.csv`, 16 responders × 10 weeks). Exclude the
week of 10 Aug by default; it changes the size of the drop, not the
conclusions.
- Acceptance: 75–78% for 6 weeks, 54% in release week, 68.5% after. A sudden
  drop, not seasonal; there's no prior-year data.
- Offers were moved, not lost (about 172 → 162 a week).
  - **4 collapsed and never recovered:** Farlight, Meteor Mite, The Undertow,
    Vesper (about 12 a week → 0–1).
  - 10 of the other 12 gained 19–49%. Halfmoon and Ashgrove fell about 25%,
    then held.
- For the four, acceptance fell first (42% in release week), and offers fell
  the week after. Bulwark missed 50% like Vesper and didn't collapse.
- **Nobody accepted below 60% before 4.2.** Acceptance before 4.2 barely
  predicts who lost pings (0.28); release-week acceptance strongly does (0.84).

**Code** (`code/dispatch-routing/`). There's one commit, so the 4.2 changes come
from the "was…" notes in `config.py`: weights 0.60 proximity / 0.25 acceptance /
0.15 capability (were 0.45 / 0.40 / 0.15), and the timer went from 90s to 60s.
- **The same rules apply to every responder**, including existing decliners.
  That answers half of Marcus's 14 Aug question; the intent is still open.
- **Points off:** −0.12 for any "not yes" (`history.py:39`). That includes
  timeouts, late or failed delivery, the wrong device and a late "yes"
  (`offer.py`: the clock starts at send, and delivery is never checked).
- **Points on:** only +0.08, for accepting (`history.py:27`). Break-even is
  60% acceptance.
- **"Recent acceptance" has no sense of time.** No fading, window or dates
  (Wen's 2019 TODO). It reflects roughly the last 9–13 offers, silence freezes
  it, and the glossary promises a recovery the code lacks.
- **No safety nets:** no rotation for low scorers and no alerts. Scores live
  only in memory (`_scores`), so the repo can't show whether the 4.2 deploy
  reset everyone to 0.5.
- **Simulation** (real rules, made-up responders): the 60s window is the
  trigger, and the weights *soften* the spiral. The shipped combination
  predicts slides, not collapses, so the four likely need an extra factor
  (location or delivery).

**Interviews and tickets** (count responders, not tickets):
- **Interviews:** vanishing 3 of 4, can't see live callouts 3 of 4, quiet 2 of
  4, and The Gale overloaded.
- **Tickets:** 12 responders; 7 report both problems, 4 quiet only, 1 vanish
  only.
- **18 of 25 tickets contradict the data.** Seven responders (Nightwell,
  Ironvale, Stormwrack and others) say they went quiet, but their offers *rose*.
  The code can't make offers vanish "in seconds."

**Five-analyst debate (30 Sept):** all agreed 4.2 caused it, not the season,
through the reweighting + the 60s timer + a penalty with no recovery. They split
on which factor dominates.

**Open asks:**
1. Ravi: a per-offer log (sent / shown on phone / outcome) and what
   `pings_sent` counts.
2. Wen: score history and travel times for the four vs. Bulwark; whether live
   matches the repo; whether scores survived the deploy.
3. Ravi or Nadia: Nightwell's actual assignments for 12–22 Aug, the quickest
   data-vs-tickets test.

**Other:** the roadmap says the routing change was "Internal," while the release
notes say responders asked for it. Interview priority page:
https://claude.ai/artifact/9JncqSoyU1P7Kfz8HGCe7Q (private).

### How to help me
- I'm new to PM terminology. Explain terms plainly and label your own
  shorthand as yours. Visual summaries at a simple reading level help me.
- Say what each claim rests on (which source, and how many people) and what
  it can't show. For calculations, show the rows and the arithmetic.
- Separate what the documents say from interpretation and cite files.
  Challenge inherited conclusions against the data, tickets and code.
- Git isn't on PATH. Use
  `%LOCALAPPDATA%\GitHubDesktop\app-3.6.6\resources\app\git\cmd\git.exe`.
  Repo: github.com/mdeleon86/claude-code-for-pms-final.

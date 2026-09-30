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
- **Callout offer:** one callout pushed to one responder's phone.
- **Callout timeout** ("ping timeout"): how long an offer stays live, the same
  for everyone. Now 60s.
- **Routing priority:** the ranking score. Inputs are proximity (travel time,
  0 beyond 45 min), capability match and recent acceptance.
- **"The change to who gets pinged":** the 4.2 routing weight rebalance.
- **Capability tags:** flight, structural-entry, hazmat-tolerant, cold-weather,
  aquatic, crowd-management, de-escalation.

### Where things stand
**Release 4.2 (12 Aug)** weighted proximity up and recent acceptance down, cut
the timeout from 90s to 60s, and added console filter persistence (expect
cosmetic tickets). Since then acceptance is down and tickets are about 3×
normal and flat. Support's split is **⅔ "phone never goes off"** and
**⅓ "gone before I could answer."**

- **Priya's view:** it's mostly seasonal, so don't revert 4.2. Treat this as a
  hypothesis; she says she "made calls faster than I checked them."
- **Marcus's 14 Aug question is unanswered:** was the rebalance meant to hit
  responders who've been declining?
- **Availability Confidence** was committed for 4.2 but didn't ship. I still
  need to confirm with Helen which squeezed items remain Q3 commitments.
- **Nobody has written down how routing works.** That's on me.
- The team is deliberately waiting for my read before drawing conclusions.

### Findings so far (sessions 1–3, not yet verified with the team)
**Data** (`data/callout-history.csv`, 16 responders × 10 weeks). By default,
exclude the week of 10 Aug, since it straddles the release. That choice moves
the size of the drop (3–12 points) but none of the conclusions.
- Acceptance held at 75–78% for 6 weeks, fell to 54% in the release week, and
  was 68.5% after. That's a sudden drop, not a seasonal shape, and there are
  no prior-year rows to test "seasonal."
- Offers were moved, not lost (about 172 → 162 a week).
  - **4 collapsed and never recovered:** Farlight, Meteor Mite, The Undertow,
    Vesper (about 12 a week → 0–1).
  - 10 of the other 12 gained 19–49%. Halfmoon and Ashgrove dropped about 25%,
    then held.
- For the four, acceptance fell first (42% in release week), and offers fell
  the week after. Sgt. Bulwark missed 50% in release week like Vesper, but
  didn't collapse.

**Code:** 4.2 changed the weights to 0.60 proximity, 0.25 acceptance, 0.15
capability (they were 0.45 / 0.40 / 0.15).
- A timeout costs −0.12, the same as a decline, even when the phone never
  showed the offer. Accepting earns +0.08.
- The score never recovers except by accepting (Wen's 2019 TODO). The glossary
  *also* says timeouts lower the score, but it promises a recovery the code
  doesn't have.
- The 60-second clock starts when the server sends the offer, not when the
  phone shows it.

**Interviews and tickets:** count responders, not tickets.
- Interviews (4 handlers): vanishing 3 of 4, can't see live callouts 3 of 4,
  quiet 2 of 4, and one overloaded responder (The Gale).
- Tickets (25 tickets, 12 responders): 7 report both problems, 4 quiet only,
  1 vanish only.
- The two sources cover different people; only Captain Vantage appears in both.

**Conflict:** 18 of 25 tickets contradict the data.
- 7 responders (Nightwell, Ironvale, Stormwrack, Falkirk, Cindermark, The
  Drift, The Longcast) say they went quiet, but their offers *rose*.
- The data matches the other sources only for the collapsed four and The Gale.
- Tickets say offers vanish "in seconds," which the code can't do.

**5-analyst debate (30 Sept):**
- All five agree 4.2 caused it, not the season. The mechanism: the reweighting
  removed the acceptance-score cushion, the 60s timer added misses, and the
  penalty has no recovery.
- They split on which factor dominates (proximity vs. penalty).
- Needed to settle it:
  1. A per-offer log (sent / shown on phone / outcome) and what `pings_sent`
     counts. Ravi.
  2. Score history and travel times for the four vs. Bulwark. Wen.
  3. Nightwell's actual assignments for 12–22 Aug. This is the quickest test
     of data vs. tickets.

**Other:** the roadmap says the routing change was "Internal," while the
release notes say responders asked for it. Interview priority page:
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

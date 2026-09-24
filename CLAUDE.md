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

### Session 1 findings (not yet verified with the team)
- **Tickets and data disagree.** Only 4 of 16 responders collapsed to ~0
  offers/week: Farlight, Meteor Mite, The Undertow and Vesper (Mite and Vesper
  have no tickets). Several "quiet" ticket filers show *rising* offers
  (Nightwell, Ironvale, Stormwrack and others). Ask Ravi what `pings_sent`
  counts.
- **Not seasonal-shaped:** acceptance held at 75–78% for 6 weeks, then fell to
  54% in the release week. There are no prior-year rows to test "seasonal."
  The rebound to 72.7% hides offers concentrating on fewer responders.
- **Code:** a timeout is penalized like a decline (the glossary says they're
  distinct). The score never recovers on its own, only by accepting (Wen's 2019
  TODO in `history.py`). The penalty (0.12) is larger than the credit (0.08).
- Both complaint themes hit the **same responders**: they wait weeks, then lose
  the rare offer in seconds.
- The roadmap lists the routing change's driver as "Internal"; the release
  notes say responders asked for it.

### How to help me
- I'm new to PM terminology. Explain terms plainly and label your own
  shorthand as yours.
- Separate what the documents say from interpretation and cite files.
  Challenge inherited conclusions against the data, tickets and code.
- Git isn't on PATH. Use
  `%LOCALAPPDATA%\GitHubDesktop\app-3.6.6\resources\app\git\cmd\git.exe`.
  Repo: github.com/mdeleon86/claude-code-for-pms-final.

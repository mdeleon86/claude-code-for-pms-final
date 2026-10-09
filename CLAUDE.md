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

### Findings so far (sessions 1–5, not yet verified with the team)
**Data** (`data/callout-history.csv`, 16 responders × 10 weeks). Exclude the
week of 10 Aug by default.
- Acceptance: 75–78% for 6 weeks, 54% in release week, 68.5% after. That's
  sudden, not seasonal (there's no prior-year data).
- **Offers moved, not lost.** Farlight, Meteor Mite, The Undertow and Vesper
  collapsed (about 12 a week → 0–1). 10 of the other 12 gained 19–49%.
  Halfmoon and Ashgrove fell about 25%, then held.
- The four's acceptance fell first, then their offers. Nobody accepted below
  60% before 4.2. Release-week acceptance predicts who lost offers (0.84);
  earlier acceptance barely does (0.28).

**Code** (`code/dispatch-routing/`). There's one commit, so the 4.2 changes come
from the "was…" notes in `config.py`: weights 0.60 proximity / 0.25 acceptance /
0.15 capability (were 0.45 / 0.40 / 0.15), and the timer went from 90s to 60s.
- **The same rules apply to every responder**, including existing decliners.
  That answers half of Marcus's 14 Aug question; the intent is still open.
- **Scoring:** −0.12 for any "not yes," including timeouts and late or failed
  delivery (`history.py:39`; `offer.py` starts the clock at send). Only +0.08
  for accepting (`history.py:27`). Break-even is 60% acceptance.
- **No sense of time:** no fading or window (Wen's 2019 TODO), so silence
  freezes the score. The glossary promises a recovery the code lacks.
- **No safety nets or alerts.** Scores live only in memory (`_scores`), so the
  repo can't show whether the 4.2 deploy reset them.
- **The 60s window is the trigger;** the weights alone soften the spiral.

**Interviews and tickets** (count responders, not tickets):
- **Interviews:** vanishing 3 of 4, can't see live callouts 3 of 4, quiet 2 of
  4, The Gale overloaded.
- **Tickets:** 12 responders (7 both, 4 quiet, 1 vanish). **18 of 25
  contradict the data.** Seven say they went quiet, but their offers *rose*.
- **Five analysts (30 Sept)** agreed 4.2 caused it, not the season.

**Module 5: "Back in Rotation"** (`05-super-speed/brief.md`, `prototype.html`)
- **Direction (Approach 2):** scores heal over time toward 0.5, and timeouts
  cost less than a "no." No line-jumping or tie-breaker. "Ready now" is a
  heads-up to the handler only.
- **Rule lab** (real history replayed; shows scores, not offers):
  - Healing does most of the work.
  - The timeout discount needs Ravi's split of timeouts vs. real "no"s.
  - Healing faster than about 3 days to halfway rewards decliners.
  - Ashgrove and Halfmoon stay at 1.00, so their drop has a different cause.
- **A healed 0.5 isn't offers back.** Peers sit near 1.0 (about a 9-minute
  gap). Open question: should healing stop at 0.5 or aim for a typical score?
- **The prototype has two separate simulations.** Sim A is Mite's offers and
  assumes 0.5 wins his old volume; Sim B is the Rule lab sliders. Last
  readiness review: about 87/100.

**Module 6: review-checklist skill** (`.claude/skills/review-checklist/SKILL.md`)
- **What it does:** a read-only review of a brief against six checks: owner,
  measurable success, consistent scope, problem before solution, evidence and
  assumptions, and risks and mitigation. Each check is rated Pass, Needs
  Improvement or Missing, with where, problem and fix. Given a folder, it
  reviews each brief and adds a summary table.
- **Test 1** (`06-sidekicks/briefs/`, 4 briefs):
  - Every brief had Risks Missing, and none had a measurable success target.
  - Top issues: Bulk Callout has no risks (extra declines hurt scores). Phone
    App misquotes Aunt Dot. Requisition Chains' scope creeps, and its 48h
    bypass undoes the second sign-off. Override Log has no success measure.
  - Halloran's interview complains approvals are *slow*, not that they need a
    second approver.
  - CHANGELOG says the override log shipped in 4.0 and bulk callout in 4.1.
- **Test 2:** checked that the skill was actually loaded. SKILL.md matched
  what was followed. One slip: a few facts (`history.py:39`, Policy 4.1)
  came from this file, not files opened in that session.
- **Schedule:** "Monday brief review" (task `weekly-brief-review`) runs Mondays
  at 9:00 local. First run is 12 Oct. Reports go to
  `06-sidekicks/review-runs/<date>.md`. The task file outside this folder holds
  no Rook details.
- **Ignore `06-sidekicks/scheduled-run-output.txt`.** It's dated 13 Oct (in
  the future), uses 4 checks, and isn't from this schedule.

**Slack:** the connector works. The class channel is
#claude-code-for-pms-sep21-26-weeknights. Always confirm the exact text
before posting there; for tests, use my own DMs.

**Open asks:**
- **Ravi:** a per-offer log (sent / shown / outcome), what `pings_sent` counts,
  a split of timeouts vs. real "no"s, and Nightwell's 12–22 Aug assignments.
- **Wen:** score history and travel times for the four vs. Bulwark; whether
  live matches the repo and scores survived the deploy; the healing speed.

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

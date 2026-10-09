# 06 · Sidekicks — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent five sessions on it: finding out what
went wrong, then building the fix nobody had — a working prototype,
by hand, in one sitting, for one room.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

Make a skill called `review-checklist`. When I point it at a product brief, check that:

1. It clearly names who owns the work.
2. It defines measurable success criteria.
3. Its scope stays consistent from beginning to end.
4. It explains the problem before proposing a solution.
5. Important claims are supported by evidence, with assumptions and unknowns clearly identified.
6. It considers risks, unintended consequences, and how those risks will be measured or mitigated.

Save the skill in `.claude/skills/review-checklist/SKILL.md` so I can reuse it in future sessions.
For every review, report each check as Pass, Needs Improvement, or Missing. Cite the relevant section of the brief, explain the problem, and recommend a specific correction.
Do not automatically rewrite the brief. Do not commit or push anything yet.
After creating the skill, show me its contents and explain how to invoke it.

### 2.

Run the review-checklist skill on 06-sidekicks/briefs/.

Review each brief separately using all six checks. Give me a summary table comparing the results and identify the most important issue in each brief.

Do not modify any files, commit, or push anything.

### 3.

Did you load and follow .claude/skills/review-checklist/SKILL.md when reviewing the four briefs, or did you perform the review independently? Please verify rather than assume.

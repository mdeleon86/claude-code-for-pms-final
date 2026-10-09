---
name: review-checklist
description: Review a product brief, one-pager, PRD or proposal against six checks (owner, measurable success criteria, consistent scope, problem before solution, evidence and assumptions, risks and mitigation) and report each as Pass, Needs Improvement or Missing, with the section cited, the problem and a specific fix. Use when the user asks to review, check or audit a brief or points the review checklist at a file or folder. Read-only, never rewrites the brief.
---

# Review checklist

Review a product brief against six checks. Report findings only. **Never edit, rewrite or reformat the brief**, even if a fix is obvious. Suggest the fix in the report instead.

## Input
- **A file path** (a `.md`, `.txt` or similar text brief): review that file.
- **A folder:** review every brief in it, one report per brief, then add a short summary table across all of them.
- **No path given:** ask which brief to review. Don't guess.

Read the **whole** brief before judging. If it cites other files (data, code, notes) and a check depends on them, you may open them to confirm a claim. Say that you did, and never treat them as part of the brief itself.

## The six checks

For each check, pick exactly one result:
- **Pass:** fully meets the standard below.
- **Needs Improvement:** partly there, but vague, incomplete or contradicted somewhere.
- **Missing:** not addressed at all.

### 1. Owner
Does the brief clearly name **who owns the work**: a specific person or team accountable for delivering it?
- **Pass:** a named person or team is responsible for delivery.
- **Needs Improvement:** only an author, an audience ("for Helen") or a vague group ("engineering") is named, or the owner is "TBD."
- **Missing:** no owner anywhere.
- An author or a reviewer is **not** an owner unless the brief says they're responsible for delivery.

### 2. Measurable success criteria
Does it define how we'll know it worked, in a way someone could actually measure?
- **Pass:** at least one metric with a target, plus a baseline or timeframe. For example, "time-to-accept stays under X seconds over 4 weeks."
- **Needs Improvement:** metrics are named, but have no target, baseline or timeframe, or use words like "improve," "reduce" or "better" without numbers.
- **Missing:** no success criteria.

### 3. Consistent scope
Does the scope stay the same from beginning to end? Compare the problem, the solution, the non-goals, the success criteria and the risks.
- **Pass:** every section describes the same scope.
- **Needs Improvement:** something appears in the solution that the problem never mentioned; a non-goal is contradicted later; a success metric measures something outside the stated scope; or the target users change partway through.
- **Missing:** no scope or boundaries are stated at all.

### 4. Problem before solution
Does it explain the problem before proposing a fix?
- **Pass:** a problem statement comes first and describes who is affected, what goes wrong and why it matters, without assuming the solution.
- **Needs Improvement:** the problem is framed as a missing feature ("we don't have X"), comes after the solution, or is too thin to judge the solution against.
- **Missing:** no problem statement.

### 5. Evidence, assumptions and unknowns
Are important claims backed up, and are guesses clearly marked as guesses?
- **Pass:** key claims cite a source (data, research, tickets, code or documents), and assumptions and unknowns are clearly labeled.
- **Needs Improvement:** some important claims have no source, assumptions are presented as facts, or the unknowns aren't called out.
- **Missing:** no evidence, and no assumptions identified.
- List each unsupported claim you find, quoting a few words of it.

### 6. Risks and mitigation
Does it consider what could go wrong, including side effects, and how those risks will be watched or reduced?
- **Pass:** risks and unintended consequences are listed, **and** each has a way to measure it (a guardrail metric) or reduce it (a safeguard).
- **Needs Improvement:** risks are listed with no measurement or mitigation, or only obvious risks are covered and side effects on other users, teams or systems are ignored.
- **Missing:** no risks considered.

## How to report

Use this format for each brief:

```
## Review: <brief title> (`<path>`)

| # | Check | Result |
|---|---|---|
| 1 | Owner | Pass / Needs Improvement / Missing |
| 2 | Measurable success criteria | … |
| 3 | Consistent scope | … |
| 4 | Problem before solution | … |
| 5 | Evidence, assumptions and unknowns | … |
| 6 | Risks and mitigation | … |

**Overall:** X Pass · Y Needs Improvement · Z Missing

### 1. Owner: <result>
- **Where:** <section heading, line number(s), or a short quote of up to about 15 words>. If Missing, say where it should go.
- **Problem:** <what's wrong, in one or two plain sentences>
- **Fix:** <a specific correction, e.g. the line to add or the number to supply>

…repeat for checks 2–6…

### Fix these first
1. <highest-impact fix>
2. …
3. …
```

For a **Pass**, keep the detail to one line citing where the brief meets the standard. Don't invent problems.

## Rules
- **Read-only.** Never edit the brief or create a new version of it, even when asked to "fix" it inside this skill. Point the user to the specific fixes instead.
- **Cite, don't paraphrase vaguely.** Every result points to a section, line or short quote.
- **Be specific in fixes.** "Add a target" is too vague. "Add: 'time-to-accept stays under 45 seconds, measured weekly for 4 weeks after launch'" is right. If the brief lacks the information needed for a real number, say what's needed and who would know.
- **Judge only what's written.** Don't give credit for things the author probably meant.
- **Don't add checks** beyond these six. Mention other observations only briefly, under "Other notes," if they matter.
- **Plain language.** Keep each explanation short and free of jargon.

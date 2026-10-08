# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

Review the "Back in Rotation" brief from the perspective of a senior product manager and dispatch engineer. Focus on the proposed "Ready now" feature. Could responders repeatedly use it to jump ahead of others, and what happens if multiple responders request priority at the same time? Identify the risks to fairness, emergency response times, and existing routing rules. Recommend practical safeguards that preserve the recovery opportunity without compromising dispatch quality. Keep the answer concise and distinguish confirmed code behavior from proposed design choices.

### 2.

Before updating the brief, compare two approaches: (1) giving a quiet responder priority for a suitable callout, with the safeguards you proposed, and (2) giving them a recovery opportunity without placing them ahead of a better-qualified or faster responder. Explain how each approach would work, its trade-offs, and whether the second approach could actually help someone who has stopped receiving offers. Recommend the approach that best balances responder recovery, fairness, and emergency response speed. Use the existing code and our findings as evidence, and clearly identify any assumptions. Keep it concise.

### 3.

Use Approach 2 as the proposed direction for "Back in Rotation." Before revising the brief, evaluate which parts we can realistically build using the existing dispatch-routing code and which require new data, infrastructure, or engineering decisions. Pay particular attention to time-based score recovery, confirmed phone delivery, the "Ready now" indicator, and the proposed tie-breaker. Identify the minimum viable version we should prototype, what we should defer, and how we'd measure whether quiet responders recover without worsening emergency response times. Distinguish evidence from assumptions. Keep it concise and do not edit any files yet.

### 4.

give me a confidence score (0-100) on how you feel about this one pager and the direction of what we need to do next? is this the right thing to prototype, or do we also need to include additional input based on the deep analysis we've been doing?

### 5.

Revise 05-super-speed/brief.md based on our three Lab A follow-up prompts and your latest confidence assessment.

Keep "Back in Rotation" as the working title and Approach 2 as the solution. The core routing changes are time-based score recovery toward 0.5 and lighter penalties for timeouts than deliberate declines.

Incorporate the important findings from our earlier analysis: live-callout alerts, visible recovery progress, what happens when recovery fails, the impact on overloaded responders, and the unresolved data questions.

Describe what Kip and a quiet responder like Meteor Mite would actually see and experience. Clearly distinguish proposed prototype screens from engineering changes that are not yet implemented.

Remove priority-jumping, guaranteed return offers, and the tie-breaker. Include shadow testing and the key measures for determining whether recovery works without slowing emergency response.

Keep it to approximately one page. Update only 05-super-speed/brief.md. Do not build the prototype, commit, or push yet.

Show me the complete revised brief when finished.

### 6.

Review our existing Back in Rotation prototype and improve the interactive simulation. We already have accept, decline, and timeout outcomes, so don't duplicate those features.

Make it possible to compare how the current Release 4.2 rules and our proposed recovery rules affect Meteor Mite over time. Clearly distinguish actual historical data from illustrative simulation results.

Include a scenario where timeouts continue to happen, and show whether recovery still occurs. Make the comparison interactive and understandable to a nontechnical product director.

Keep everything in the existing single-file 05-super-speed/prototype.html. Do not commit, push, or publish it yet. Explain what you changed and what assumptions the simulation uses.

### 7.

Improve the existing `05-super-speed/prototype.html` using the techniques demonstrated in Module 5. Keep the current Back in Rotation prototype, including Kip's console, Meteor Mite's phone, and all existing interactions.
1. Add a six-responder comparison
Use the actual `callout-history.csv` data to compare the four responders who experienced the largest declines (Farlight, Meteor Mite, The Undertow, and Vesper), plus Corporal Ashgrove and Halfmoon.
Compare four scoring rules:

* Current Release 4.2 rules.
* Score recovery over time only.
* Reduced timeout penalty only.
* Both proposed changes together.

Show the calculated scores side by side so we can understand which change makes the biggest difference.
2. Add three interactive sliders
Allow users to adjust:

* How many missed offers were timeouts rather than deliberate declines.
* How much a timeout lowers someone's score compared with a deliberate decline.
* How quickly someone's score recovers over time.

Write the slider labels and explanations at approximately a 5th-grade reading level. Use everyday language and explain what happens when each slider moves.
3. Add a Decliner Test
Simulate someone receiving 10 offers per week and deliberately declining every offer for 10 weeks.
Show their ending score under all four rules. Determine whether our proposed changes accidentally reward people who repeatedly refuse work. Do not force the results to match the instructor's example.
4. Protect the credibility of the analysis
Use actual historical data where available and the confirmed routing rules from the repository.
Clearly label assumptions, especially because the historical data doesn't distinguish timeouts from deliberate declines. Do not invent missing historical records or claim the simulation predicts actual dispatch outcomes.
5. Test and explain
Keep everything in the existing single-file HTML prototype. Make sure the new controls work and don't break existing features.
Explain what you changed, what the simulation demonstrates, and whether the results suggest we should reconsider any part of our proposed solution.
Do not modify `brief.md`, commit, push, or publish anything yet.

### 8.

Act as a senior product manager and dispatch engineer. Review the completed `05-super-speed/brief.md` and `05-super-speed/prototype.html` against Helen's original request in `05-super-speed/director-request.txt`.
Test the prototype's interactions, Rule Lab sliders, six-responder comparison, and Decliner Test. Check whether the calculations, conclusions, and labels are consistent with the actual code and historical data.
Identify any misleading assumptions, simulation errors, fairness risks, or gaps between the brief and prototype.
Give me:

1. An overall readiness score out of 100.
2. Any critical issues that must be fixed before submission.
3. Any optional improvements.
4. Whether the prototype clearly demonstrates the proposed solution to Helen.

Keep the feedback concise. Do not modify files, commit, or push anything yet.

### 9.

Use your latest readiness review of `05-super-speed/brief.md` and `05-super-speed/prototype.html` to fix the four critical issues you identified.
1. Update the brief
Incorporate the Rule Lab findings:

* Score healing appears to be the main recovery mechanism in our simulation.
* The timeout discount requires more evidence from Ravi.
* Fast healing may reward deliberate declines, so we need safeguards.
* Ashgrove and Halfmoon may require a different solution.

Clearly distinguish evidence, assumptions, and recommendations. Keep the brief approximately one page.
2. Correct misleading recovery claims
Remove unsupported promises about returning to normal offer volume or recovering within a specific number of days.
Ensure the recovery language and timelines agree with the underlying calculations. Clearly distinguish a score moving toward 0.5 from actually receiving more offers.
Flag the unresolved design question of whether recovery should stop at 0.5 or target another score. Do not make that decision without evidence.
3. Clarify the two simulations
The existing "Compare the rules" section and Rule Lab use different controls and assumptions.
Make this distinction obvious to users, or connect the controls if that is straightforward and does not introduce unnecessary complexity.
4. Correct unsupported wording
Fix claims about the sequence of missed offers, the dates of historical data, and the response-time cost of deliberate declines versus timeouts.
5. Final quality check
Make the prototype easy for Helen to understand. Add a brief guide explaining what to click first, but preserve the existing interactive features.
Test all controls and calculations after making changes. Report exactly what you changed, any remaining limitations, and an updated readiness score.
Do not commit, push, or publish anything yet.

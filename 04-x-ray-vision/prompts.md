# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

Interesting. How do you think this could info what we saw from some of the data analysis?

### 2.

Show me exactly what changed in the dispatch-routing code for Release 4.2. Compare the old behavior with the current behavior, explain each change in plain English, and tell me how each change could affect which responders receive pings and how often.

### 3.

Take the three changes from Release 4.2 and test them one at a time. If only the answer window changed, what would happen? If only the routing weights changed, what would happen? Then explain what happens when both changes operate together and which combination best fits the pattern we saw in the responder data.

### 4.

Based only on the actual code and files in this repository, what can we prove caused responders to lose pings, and what are we still assuming? Keep the answer concise and cite the specific file for each finding. Do not run simulations or create hypothetical data.

### 5.

Compare what we found in the dispatch-routing code with the responder data from last session. Does the data behave the way we'd expect if Release 4.2's routing changes affected existing responders based on their previous track records? Use the data to test that idea, and tell me whether it answers Marcus's question about whether the change applied to existing responders who had already been turning jobs down or only to new responders.

### 6.

Check whether there is any evidence in the repository that responder track-record scores were preserved or reset when Release 4.2 was deployed. Look at the code, release files, and company documents. Don't speculate. If the repository can't answer it, tell me exactly what information is missing.

### 7.

Using only evidence we have confirmed so far, give me the strongest answer we can give Marcus about who the 4.2 routing changes applied to. Separate what the code proves from what we cannot know about the live deployment. Keep it brief and don't do any new simulations.

### 8.

What situations can cause a responder to lose points even when they may not have intentionally rejected a job? Check the code for timeouts, delivery failures, cancellations, or anything else that could lower their score without a deliberate decline. Keep it concise.

### 9.

Trace what happens after a responder's score starts falling. What mechanisms would you normally expect to prevent a temporary run of missed offers from becoming a long-term problem, and which of those mechanisms are actually missing from this code? Stick to what the code supports and keep it concise.

### 10.

The code calls this a "recent acceptance" score, but does it actually measure recent behavior? Trace exactly how that score is calculated over time and explain whether old misses ever stop affecting someone. If the name and behavior don't match, explain why that matters for someone who's gone quiet. Keep it concise.

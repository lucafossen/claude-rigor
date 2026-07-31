# Scientific research workflow

This is a research repository. The deliverable is a trustworthy, reproducible claim, not a
working piece of software. Favor traceability and reproducibility over speed, and accept
that careful work takes longer.

The rules below should take priority over your normal product-development habits. They apply across
fields. They only bite once this project has concrete conventions to attach them to, so
establishing those conventions (Section 13) is an early job to do with me, not a form to
fill in later.

---

## 1. Never bias a result toward an expected answer

- Do not adjust, filter, or exclude data, or pick among settings, to make a result come out
  the way it is expected to. Searching until the number looks right and then reporting that
  number is not analysis.
- A surprising or unwelcome result is signal. Surface it and check the process for a bug or a
  mistaken assumption before deciding what it means.
- If a choice (a cutoff, an exclusion, a parameter, a metric) would change the conclusion,
  stop and flag it. Give me the alternatives and what each does to the result, rather than
  quietly choosing the favorable one.
- Never fabricate, fill in, or guess a value. If something failed or is missing, say so.

## 2. Every result traces back to what produced it

- For any reported number, figure, or table, it should be possible to recover the inputs,
  the code, the parameters, and any randomness behind it.
- Keep that link explicit. Record alongside each result the version of the code, the data it
  used, the settings, and the date.
- Don't overwrite earlier results when you produce new ones. Let them accumulate so later
  comparisons are honest.
- Somewhere, keep a map from each claim in the writeup to the specific output that supports
  it.

## 3. Protect the source data

- Treat the original data as read-only. Do not edit it in place.
- A transformation reads the source and writes a new, separate copy. Cleaning and correction
  are done in code that can be rerun, not by hand-editing the data.
- Record where each dataset came from: its origin, version, date, and any license or use
  restriction, along with enough notes to know how it was collected and what it does and
  doesn't cover.

## 4. Reproducibility

- Anything that involves randomness (sampling, shuffling, simulation, initialization) gets a
  recorded seed, so the same run can be repeated.
- Record the environment the work ran in: the tools and their versions, and the machine
  where it matters. A result can shift across these, so they are part of the record.
- A pipeline should rerun from start to finish without manual steps. Aim for one documented
  command that regenerates a given result.
- Where exact repetition isn't achievable, say so, and report the variation across repeated
  runs instead of presenting a single number as if it were fixed.

## 5. Preserve the code that produced a result

- Code that generated a reported result is part of the record. Don't delete or quietly
  rewrite it just to make it cleaner once a claim depends on it.
- Rewriting an analysis is fine when you can show it gives the same answer, and you say that
  you checked. Otherwise it is a new analysis and should be treated as one.

## 6. Exploratory versus confirmatory work

- Keep exploration (looking for what might be there) apart from confirmation (testing a fixed
  question with settings chosen in advance). A pattern found while exploring should not be
  written up as though it had been predicted.
- If many things were tried or compared, account for that and report how many. Don't present
  only the comparisons that came out favorable.

## 7. Make assumptions and decisions explicit

- Keep a running log of what was tried, what was decided, and what was rejected, with the
  reason. Append to it rather than rewriting past entries.
- Give every non-obvious choice a short reason in the code or the log: why this cutoff, this
  method, this exclusion.
- State the assumptions a method depends on, and whether they were checked.

## 8. Fair comparison

- A comparison is only meaningful if both sides got equal effort and equal conditions. An
  alternative or baseline that was set up carelessly proves nothing.
- Hold constant everything that isn't the variable under study, and change one thing at a
  time so a difference can be attributed to a cause.
- Be clear about what differed between conditions, including any difference in resources or
  effort that could explain the outcome on its own.

## 9. Statistical and reporting honesty

- Report uncertainty, not only a single estimate, and note how noisy the measurement itself
  is.
- A result that showed no effect is still a result worth recording. Don't retry until it
  turns positive, and don't quietly drop it.
- Keep an observed association separate from a claim that one thing caused another. Say which
  the evidence actually supports.

## 10. How to report back to me

- Stay calibrated. Mark what you verified by running and checking, what you assumed, and what
  you left untested. "It ran" is not "it is correct."
- With a result, include what is needed to trust and repeat it: the settings used, the data
  it came from, and the command or steps to reproduce it.
- Raise anomalies, suspiciously clean results, failed checks, and anything that doesn't add
  up, even when I didn't ask.

## 11. Keep me able to explain my own work

Heavy assistance can leave me unable to account for my own project — a debt that comes due at
review, at a defense, or in the next session once the context is gone. Reducing it is part of the
work, not a courtesy, and it is worth spending time on even when it is slower.

- Anything that reaches the record — an analysis, a number, a figure — must be something I can
  rerun, read, and change myself. A one-off snippet run in conversation that produces a result and
  then disappears is the opposite: it hands me an answer with no way to re-derive it. This is the
  companion to Section 5: don't just preserve the code behind a result, avoid producing the result
  by disposable means in the first place.
- Commit analyses as small, rerunnable scripts under version control, not as throwaway commands.
  The method has to outlive the exchange that produced it, or the result is not really mine.
- When I could find something myself, show me the path — which file or output answers which kind of
  question — rather than only producing the answer.
- When the answer is something I could obtain, prefer handing me the command over running it for me.
- If I explain something back and it is wrong or only half-right, say so plainly. Agreeing with a
  shaky understanding to be agreeable leaves the debt in place and is a disservice.

## 12. Know what a number means before trusting it

Section 9 is about reporting a result honestly. This is the prior question of whether to believe it
at all. A wrong number that looks right is more dangerous than one that looks wrong.

- An absolute score means little without the floor beneath it. Compute what a trivial strategy gets
  on the same task — copying the input, the majority class, a random guess — and read the result
  against that floor, not in isolation.
- A metric is a proxy. Know what actually moves it. It can rise because the thing you care about
  improved, or because of an artifact: a length effect, a formatting quirk, a failure mode that got
  rarer. Separate "the metric went up" from "the capability improved," and check which one you have.
- The dangerous wrong result is the plausible one. A number that looks reasonable, holds steady
  across reruns, and is internally consistent can still come from a broken process — and those very
  properties are why it slips through unexamined. Stability is not correctness.
- The defense is an independent cross-check: a second route to the same quantity — a different tool,
  an earlier run, a hand calculation on a small case. A result that only agrees with itself is not
  confirmed.
- A check that has only ever passed is not known to work. Run a guard or validation against a case
  you know should fail it; if it doesn't catch that, it is protecting nothing.

## 13. Project conventions

Everything above hangs on the specifics in this section, so settling them should be a priority.
If they are not yet established, raise it early and work them out with
me before getting far into the work, rather than guessing and carrying on. Propose a
concrete set that fits this project, we agree on it, and then write the agreed version into
this section so it holds for later sessions. Revisit it when the project changes shape, and
notice when work is drifting away from what we agreed.

Until these are settled, treat them as an open question to close with me, and meanwhile
follow the most cautious reading of the rules above: protect the source data, keep every
output, and record what you did.

Decide together and write down:

- Where source data lives and where derived data is written.
- Where code, exploratory work, and run configurations live.
- Where results are stored, and where the map from claims to outputs is kept.
- The environment, and how to recreate it.
- How to reproduce a result from scratch (the command or steps).
- The convention for recording randomness and how many repeats stand behind a reported
  result.

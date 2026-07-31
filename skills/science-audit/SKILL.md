---
name: science-audit
description: Audit a research project against the principles in CLAUDE.md (scientific research workflow) and align it to them. Use when the user wants to check a project for reproducibility, provenance, data hygiene, fair comparison, and honest reporting, or asks to "run a science audit" or "align this project to the research ideals".
argument-hint: "[path to project, defaults to current directory]"
---

# science-audit

Audit a research project against the ideals in `CLAUDE.md` and bring it closer to them.
The audit is itself bound by those ideals: it must not do anything that would change a
reported result, touch raw data, or rewrite history. It surfaces and proposes; it fixes
only what is safe and additive, and only with the user's go-ahead.

## Scope

The target project is the path in the argument, or the current directory if none is given.

## Procedure

### 0. Draw the line between past and future work

The standard is forward-looking. The skill will not, on its own, redo old work to make it
conform, because re-running results, retrofitting seeds, or rewriting the analysis that
produced them can destroy the provenance the standard exists to protect. So treat adoption
as a line drawn in time:

- Results and code that already exist are grandfathered by default. The audit's own goal
  for them is an honest record, not a redo. Where provenance is missing, write down what can
  be reconstructed and mark plainly what cannot (for example, "produced before provenance
  tracking; seed and environment unknown"). Never invent or back-fill a record that wasn't
  kept.
- New work from adoption onward follows the full standard.

Redoing past work is allowed when the user asks for it. Reproducing or re-running an old
experiment is legitimate; the rule is only that the user decides it, not the skill, and the
existing results are kept alongside the new ones rather than overwritten. When a redo would
help (say, to recover lost provenance or check an old result), offer it as an option and let
the user choose.

When a principle is unmet only because of past work and the user hasn't asked to redo it,
file it as grandfathered with an honest note, not as a gap to fix. Don't try to repair the
whole history in one pass. Say this framing to the user explicitly so they know the past is
being documented, not rewritten.

### 1. Load the standard

- Read the project's standard: `.claude/rules/research-workflow.md`, the project's `CLAUDE.md`,
  and anything that `CLAUDE.md` imports (for example a line like `@.claude/research-workflow.md`).
  Together those are the ideals to audit against.
- If the project has no research standard, read the generic one that ships alongside this skill,
  at `template/.claude/rules/research-workflow.md` relative to the root of this skill's own
  directory tree, and tell the user the project is being judged against it rather than against
  conventions of its own. Offer to install it with `/research:onboard`.
- Check whether the project conventions (the equivalent of Section 13) are actually
  established or just left blank. If they are unset, treat that as a priority.
  Make closing it a lead recommendation, and offer to work the conventions out with the user
  then and there rather than only noting their absence.

### 2. Survey the project

Build a picture before judging. Look at the directory layout, the README and any notebooks
or logs, how data flows from source to results, where outputs and figures come from, how
the environment is specified, and whether anything records randomness, versions, or
decisions. Note what kind of work this is (data analysis, simulation, experiments, etc.)
so the findings fit the project rather than a template.

Do not run the project's pipelines or analyses as part of surveying. Reading is safe;
executing could change outputs or cost real compute.

### 3. Audit against each principle

Go through the sections of `CLAUDE.md` and, for each, record a finding. Anchor every
finding in something you actually saw in the project (a file, a path, an absence), not a
generic worry. Suggested checks:

- **No biased results**: any sign of results overwritten in place, hand-edited outputs, or
  thresholds that look chosen after the fact. Flag, never auto-"correct".
- **Traceability**: can each figure or reported number be traced to the code, data, and
  settings that made it? Is there a map from claims to outputs?
- **Source data protected**: is raw data separate and treated as read-only, or is it edited
  in place? Are origin, version, date, and license recorded?
- **Reproducibility**: is randomness seeded and recorded? Is the environment pinned? Can the
  pipeline rerun from a single documented command?
- **Preserved code**: is the code behind past results kept, or has it been overwritten?
- **Exploratory vs confirmatory**: are the two mixed? Are all comparisons that were run
  accounted for?
- **Explicit assumptions**: is there a decision log? Are non-obvious choices justified?
- **Fair comparison**: are baselines and conditions set up with equal care, and are the
  differences between conditions stated?
- **Statistical honesty**: is uncertainty reported? Are null results kept? Is association
  separated from causation?
- **Comprehension**: do reported numbers trace to committed, rerunnable scripts, or to ad-hoc
  snippets that no longer exist? Could the user reproduce a given result themselves from what is
  in the repo?
- **Interpretable numbers**: are trivial baselines or floors reported alongside absolute scores?
  Is it stated what each key metric measures and what artifacts could move it? Is any headline
  result cross-checked against an independent route rather than only its own reruns?

Grade each finding as one of: **meets** the ideal, **gap** (missing but safe to add),
**risk** (something that may already have compromised a result and needs a human decision).

### 4. Sort proposed changes by safety

Split everything you would change into two lists, and keep them separate.

**Safe and additive**: may be applied on approval, because they only add records or
structure and cannot change any existing result:
- installing the research standard (a new `CLAUDE.md`, or a separate imported file when one
  already exists), a README, a decision log (`NOTES.md`), or a dataset card
- adding a `results/INDEX.md` mapping claims to the outputs that support them
- writing down the environment (a lockfile or an exported environment file)
- adding a run manifest format, or a place for it, for future runs
- recording seeds and versions going forward
- promoting an ad-hoc analysis into a committed, rerunnable script, leaving any result it already
  produced untouched (this adds a reproducible method; it does not change the number)
- adding a baselines/floors record for the reported metrics, computed from the existing data
- reorganizing so raw data sits in a separate read-only location (by copying, leaving the
  original in place, never by moving or editing it)

**Needs a human decision**: propose, explain, and wait. Never do these automatically:
- anything that would re-run, re-fit, or regenerate a result
- adding a seed to code that already produced reported, unseeded results (this changes the
  numbers; the user decides whether and when)
- deleting, moving, or rewriting data or result-producing code
- changing an analysis, threshold, or exclusion
- correcting a result that looks wrong

### 5. Report, then act

- Present the audit as a short report: for each principle, the finding and the evidence,
  then the two change lists.
- Apply the safe and additive changes only after the user approves them, and only those.
- For the risk items, lay out the options and their consequences and let the user choose.
  Do not pick the convenient one.
- When done, summarize what was changed and what was left for the user to decide, in
  calibrated terms: what you verified, what you assumed, and what remains open.

## Guardrails

- Read-only on data and results unless the user explicitly approves a specific change.
- Never alter, regenerate, or "fix" a result to make it look better; surface it instead.
- Never move or edit raw data; if isolating it, copy and leave the original untouched.
- Prefer adding records over changing artifacts. When in doubt, propose rather than apply.
- Don't redo or regenerate past results on your own initiative. Grandfather them with an
  honest record, and redo only when the user asks, keeping the old results alongside the new.
- Keep the project's own files free of agent-specific names. Any human-facing artifact you
  create in the project (decision log, dataset card, results index, README text) should read
  as an ordinary research document and must not mention this skill, the assistant, or the
  files that wire it in. Confine that machinery to the files that have to contain it (the
  imported standard and the agent config directory).

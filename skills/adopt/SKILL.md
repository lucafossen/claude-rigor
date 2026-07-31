---
name: adopt
description: Install the scientific research standard into the current project, then settle its project conventions. Use when the user wants to adopt the research workflow here, asks to "adopt the research standard", "install the research workflow", "set up research conventions", or wants a project to start following the reproducibility and provenance rules.
argument-hint: "[path to project, defaults to current directory]"
---

# adopt

Turn the research standard on for one project, and leave it usable rather than merely present.

Installing the file is the small half. The standard is written to hang on Section 13 (project
conventions), and until those are settled it says to treat them as an open question and follow the
most cautious reading of everything else. A copy with Section 13 left blank is a half-installed
standard. Finishing that conversation is the point of this skill.

**Order of operations.** Write the file first, then talk. Do not preview the plan, summarize what
you found, or ask whether to proceed before the standard is on disk — installing it is what the
user asked for by running this skill, it is purely additive, and deleting the file undoes it. Steps
1 to 3 run straight through without stopping for input. Only step 4 is a conversation. A session
that is interrupted, or that gets no answer, must still leave the project with the standard
installed and Section 13 honestly marked open.

## 1. Find the standard

Read the shipped copy at `template/.claude/rules/research-workflow.md`, relative to the root of
this skill's own directory tree — the same tree this `SKILL.md` sits in, two levels up from
`skills/adopt/`. When the skill is installed as a plugin that root is inside the plugin cache;
when it was copied into a project it is that project's copy. If it is not at that path, search for
`research-workflow.md` nearby before giving up, and say plainly if you cannot find it rather than
writing a standard from memory.

## 2. Check what is already there

Look for a standard the project already has, in `.claude/rules/`, in `CLAUDE.md`, and in anything
`CLAUDE.md` imports.

- **Nothing found**: continue to step 3.
- **Already installed**: do not overwrite. Report what is there, diff it against the shipped copy,
  and let the user decide whether to update it, keeping any conventions they have filled in.
- **The project has its own unrelated `CLAUDE.md`**: that is fine and stays untouched. The standard
  goes in `.claude/rules/`, which loads alongside it.

## 3. Install it

Do this now, before any conversation about conventions. Read the shipped standard and write it
verbatim to `.claude/rules/research-workflow.md` in the project. Do not edit the text on the way
in, and do not reformat, summarize, or trim it — a standard the user cannot compare against the
original is worth less than one they can.

Use the file tools to read and write it, not shell commands. `cp` and `mkdir` do not exist on a
Windows command prompt, and a shell copy asks the user for a permission they should not have to
grant to install a text file.

Nothing gates this step except step 2 finding a standard already installed. In particular it does
not wait on the user agreeing to Section 13, and it does not depend on the user being available:
the file is additive, it changes nothing that is already in the project, and deleting it undoes
the whole thing. An existing `CLAUDE.md` is not a reason to hold off, because this does not touch
it — files in `.claude/rules/` load at the start of every session in that project and nowhere
else, so no import line is needed and no other project is affected.

Never write the standard, or a draft of Section 13, to some other file as a substitute for
installing it. A draft the user has to paste in by hand is the failure this skill exists to
prevent.

## 4. Settle Section 13

This is the part that matters, and it is a conversation, not a form. Read Section 13, then work
through it with the user. Ask about the ones that are genuinely open; propose a concrete default
where the repository already answers the question, and say what you are inferring it from.

- Where source data lives, and where derived data is written.
- Where code, exploratory work, and run configurations live.
- Where results are stored, and where the map from claims to outputs is kept.
- The environment, and how to recreate it.
- How to reproduce a result from scratch — the command or steps.
- How randomness is recorded, and how many repeats stand behind a reported number.

Look before you ask. If the project already has a `data/` directory, a lockfile, a `Makefile`, or a
seed set somewhere, propose what is there instead of asking from a blank page. If the project is
empty, say so and propose a starting layout.

Write the agreed answers into Section 13 of the installed copy, in the user's words where they gave
them. Leave anything genuinely undecided marked as open, with a note of what has to happen to close
it — an honest gap is better than a plausible-looking guess, and Section 1 forbids the guess.

## 5. Hand back

Tell the user, briefly:

- Where the standard now lives, and that deleting the file turns it off.
- Which conventions were settled and which are still open.
- To commit the file, so the repository records which version of the standard governed its results.
- That `/research:science-audit` checks the project against it, once there is work to check.

## Boundaries

- Do not touch data, results, or analysis code. This skill writes
  `.claude/rules/research-workflow.md` and edits that file's Section 13. Nothing else.
- Do not modify the project's `CLAUDE.md`. The standard does not need it.
- Do not overwrite a standard the project already has without the user agreeing to it first.
- Do not fill in conventions the user has not agreed to just to leave the section looking
  complete. Mark them open, in the installed file — not in a draft somewhere else.

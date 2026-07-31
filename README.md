# claude-rigor

A research standard that points Claude Code at a scientific workflow — provenance, reproducibility,
protected source data, honest reporting, interpreting a number before trusting it, and keeping the
researcher able to explain their own project — instead of its default ship-the-software mindset.
Ships with three skills: `adopt` installs the standard into a project and settles its conventions,
`science-audit` checks a project against those principles, and `tutor` helps you re-understand a
project you have been building.

## Install

Install the skills once, then turn the standard on in the projects that want it.

### 1. The skills

Inside Claude Code, once per machine:

```
/plugin marketplace add lucafossen/claude-rigor
/plugin install research@claude-rigor
```

They invoke as `/research:adopt`, `/research:science-audit` and `/research:tutor`. Plugin skills are
namespaced, which is why the `research:` prefix appears.

Installing them once for the machine does not affect your non-research projects. A skill is inert
until it is invoked, so the skills simply sit unused in a project that has not adopted the standard.
The standard is the half that changes how Claude behaves, and that is the half you turn on per
project.

### 2. The standard, per project

In the project you want it in:

```
/research:adopt
```

That writes the standard to `.claude/rules/research-workflow.md` and then works through Section 13
(project conventions) with you, which is the half that a copied file leaves undone. It will not
overwrite a standard the project already has.

Files in `.claude/rules/` load at the start of every session in that project and nowhere else, so
there is nothing to import, no `CLAUDE.md` to edit, and no effect on any other project. Commit the
file, so the repository records which version of the standard governed its results. To turn it off,
delete it.

No skills installed, or you would rather do it by hand? Copy the [`.claude`](template/.claude)
folder from [`template/`](template) into the project — it is already in the right shape, and merges
into an existing `.claude` folder without disturbing it. Then fill in Section 13 yourself.

### Turning it on for every project instead

If most of what you do is research, put a copy in your user rules directory, which loads in every
project on the machine:

| | |
| :-- | :-- |
| macOS, Linux | `~/.claude/rules/research-workflow.md` |
| Windows | `%USERPROFILE%\.claude\rules\research-workflow.md` |

Nothing else to configure — files in that directory load on their own, with no import line. Update
it by copying a newer version over the top.

On macOS and Linux you can symlink instead of copying, so `git pull` in a clone updates the standard
everywhere at once. On Windows a symlink needs Administrator or Developer Mode.

A global install also governs your software projects, where a standard whose opening
line is "the deliverable is a trustworthy claim, not a working piece of software" is the wrong
instruction. And Section 13 is per-project by design — a single global copy has nowhere to record
one project's conventions. The arrangement that avoids both problems is to install globally for
Sections 1–12 and keep a short per-project file for Section 13, imported from that project's
`CLAUDE.md`.

The standard is also not committed next to the results it governed, so a repository no longer
records which version of the rules applied. If that matters for a given project, use
`/research:adopt` there instead.

## adopt

`/research:adopt [path]` turns the standard on for one project: it writes
`.claude/rules/research-workflow.md`, then works through Section 13 with you, proposing what the
repository already answers and asking about the rest. Settling those conventions is the part a
copied file leaves undone — the standard is written to hang on them, and until they are set it
tells Claude to treat them as an open question. It will not overwrite an existing standard or
touch data, results, or analysis code.

The first run asks permission to read the standard from where the plugin is installed, which is
outside your project. Approve it. If it cannot read the shipped copy it stops and says so rather
than writing one from memory, since a standard you cannot diff against the original is worth less
than no standard at all.

## science-audit

`/research:science-audit [path]` reviews a project against its `CLAUDE.md` and proposes how to close
the gaps. It reads rather than runs, never edits raw data or alters a result, and applies
only the safe additive fixes you approve, leaving anything that would change a reported
number for you to decide.

## tutor

`/research:tutor [topic]` pays down **comprehension debt** — the state of not being able to explain your own
project after building it with heavy assistance. It teaches one slice at a time, grounded in the
actual files (with `file:line` you can open), then makes you explain it back and corrects you
honestly, because being told is not the same as understanding. It also runs the other way: explain a
part to it and it checks you against the code. Read-only — it surfaces problems for you to decide on
rather than fixing them mid-lesson. Good before a supervision meeting, a defense, or picking up a
project after a break.

## Developing this repository

The repository root is itself the plugin root, so the skills load without installing anything:

```bash
claude --plugin-dir .
claude plugin validate .
```

`skills/` is the single source of truth for both install paths — there is no second copy to keep in
sync.

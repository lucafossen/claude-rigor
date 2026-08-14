# claude-rigor

A research standard that steers Claude Code towards a more scientific workflow (provenance, reproducibility,
protected source data, neutral reporting, interpreting a number before trusting it, keeping the
researcher able to explain their own project) instead of its default "shipping mindset".
Installing this plugin gives you three skills: `onboard` installs the standard into a project and clarifies its conventions,
`science-audit` checks a project against those principles, and `tutor` helps you engage with and understand the details of a
project you have been building with Claude.

## Install

Install the skills once, then turn the standard on in the projects that want it.

### 1. The skills

Inside Claude Code, once per machine:

```
/plugin marketplace add lucafossen/claude-rigor
/plugin install research@claude-rigor
```

They invoke as `/research:onboard`, `/research:science-audit` and `/research:tutor`.

### 2. The standard, per project

In the project you want to activate, write in Claude Code:

```
/research:onboard
```

That writes the standard to `.claude/rules/research-workflow.md` and then works through project conventions with you. It will not overwrite a
standard that the project already has.

## science-audit

`/research:science-audit [path]` reviews a project against its `CLAUDE.md` and proposes how to close
the gaps. It reads rather than runs, never edits raw data or alters a result, and applies
only changes you approve.

## tutor

`/research:tutor [topic]` pays down **comprehension debt**, the state of not being able to explain
your own project after building it with heavy assistance. It teaches one slice at a time, grounded in
the actual files (with `file:line` you can open).

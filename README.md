# Kickoff

[![License: MIT](https://img.shields.io/badge/license-MIT-3b3431?style=flat-square)](LICENSE)

A pipeline for starting a new project. Three stages run in order: an interview to pin down the idea, deep research, then file generation.

```text
Stage 1  Clarify   interview until the brief is solid
Stage 2  Research  parallel web tracks, sources tagged by confidence
Stage 3  Generate  devkit/ files you can build from
```

You get asked the Stage 1 questions, and one more if research concludes the project shouldn't be built. The rest runs unattended.

Each stage writes to disk before the next one starts, so a `/compact` in the middle doesn't lose the work.

## What lands in your project

A `devkit/` folder:

- `devkit/RESEARCH.md` holds the brief, the research report (competitors, stack with confidence tags, architecture patterns, things not to hand-roll, risks). It grows stage by stage.
- `devkit/PROJECT.md` is the control plane. Vision, scope, architecture, invariants, build plan, plus a protocol telling any agent how to keep the kit current.
- `devkit/STATE.md` is the cursor. Current task, next steps, blockers.

Templates for `DECISIONS.md`, `JOURNAL.md` and per-feature specs come with `creator`. They get created when something triggers them, not upfront as empty files.

## What's in this repo

Three skills, all original work here:

| Folder | Job |
|---|---|
| `kickoff/` | The orchestrator. Sequencing, handoffs, the no-go check. Thin by design. |
| `research/` | Stage 2. Parallel tracks, source policy, confidence tags, a self-critique loop. |
| `creator/` | Stage 1 checklist, Stage 3 generation. Ships the `devkit/` templates. |

One more skill does the interview at Stage 1. It lives in its own repo and this one doesn't copy it:

| Skill | Used at | Source |
|---|---|---|
| `grill-me` | Stage 1 interview | [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock |

Installing from source means you get its updates and its author keeps the credit.

## Install

### With an AI agent

Paste this into Claude Code, Codex, Hermes or any agent that can run commands on your computer:

```text
Install the kickoff pipeline. It's four skills.
1. Run: npx skills add bgtvdm/kickoff -g --skill '*'
   (installs kickoff, research and creator from https://github.com/bgtvdm/kickoff)
2. Run: npx skills add mattpocock/skills -g --skill grill-me
3. Skip any skill I already have and tell me which ones you skipped.
4. Check that each of the four folders has a SKILL.md directly inside,
   then tell me to start a new session so they load.
To update later: npx skills update.
```

### By hand

```bash
npx skills add bgtvdm/kickoff -g --skill '*'
npx skills add mattpocock/skills -g --skill grill-me
```

The [cross-agent installer](https://github.com/vercel-labs/skills) puts each skill where your agent looks for it. Drop `-g` to install for the current project only.

Then open a project and say `kickoff: <your idea>`.

## How each stage behaves

Stage 1 asks one question at a time, each with a recommended answer, working down the dependency order of the decisions. It stops once the four things are settled: what and for whom, what problem, MVP scope, constraints. There's a question ceiling so it can't stall the pipeline. In an existing repo it reads the code instead of asking about it.

Stage 2 runs at least 3 web tracks in parallel: competitors and market, stack and architecture, pitfalls and what not to hand-roll. Genuinely separate concerns get their own track, up to 7. Stack claims get checked against current docs. Every claim carries a HIGH, MEDIUM or LOW tag.

Stage 3 hands everything to `creator`, which synthesizes it into `devkit/` and points at the first concrete build step. `creator` skips its own research fan-out, since Stage 2 already covered it.

## Using less of it

For a lighter run, use `creator` on its own. It has a research step built in.

The knobs live in the skill files. Stage 1's question ceiling is one line in `kickoff/SKILL.md`. The 3-track floor and 7-track ceiling are in `research/SKILL.md`.

## Credits

Stage 1's interview method is [Matt Pocock's](https://github.com/mattpocock/skills) `grill-me`. File issues about those skills on their repos, not here.

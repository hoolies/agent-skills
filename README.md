# Agent skills

Private working copy of the Cursor skills I use.

Cursor reads the live copies from `~/.agents/skills`. `writing-shell-scripts` also lives in `~/.cursor/skills`. This repo is the snapshot. Editing a file here does not change what Cursor runs until that file is copied back.

`sources.json` records where each skill came from. Several are other people's work, including Anthropic skills under their terms. Keep this repo private.

## Where they came from

132 skills. Two repos account for most of them. The rest are skills those lists link to, plus two that were already on this machine.

| Source | Skills | What it is |
| --- | ---: | --- |
| [spencerpauly/awesome-cursor-skills](https://github.com/spencerpauly/awesome-cursor-skills) | 64 | The catalog's own Cursor workflows: browser QA, parallel review, CI, auth, testing, UI |
| [mattpocock/skills](https://github.com/mattpocock/skills) | 37 | Specs, grilling, TDD, triage, code review, implementation |
| [anthropics/skills](https://github.com/anthropics/skills) | 8 | Docs and office files, linked from the catalog |
| [getsentry/skills](https://github.com/getsentry/skills) | 6 | Review, bugs, and the skill scanner, linked from the catalog |
| [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) | 4 | React and design guidelines, linked from the catalog |
| [ZengLiangYi/ChatCrystal](https://github.com/ZengLiangYi/ChatCrystal) | 3 | Task recall and writeback |
| One-off upstreams | 8 | PostHog, terraform, concise, dependabot, authsome, penthera, toprank, shadcn |
| Already on this machine | 2 | `writing-shell-scripts`, `reverse-engineer-anything` |

The one-off names and URLs are in `sources.json`. Matt Pocock skills that the catalog also links (`tdd`, `grill-me`, `improve-codebase-architecture`) stayed as the Matt Pocock copies.

## How a new skill is merged

Install into `~/.agents/skills` with the skills CLI, copy mode, Cursor agent, global:

```bash
DISABLE_TELEMETRY=1 npx --yes skills@latest add OWNER/REPO --skill '*' -g -a cursor -y --copy
```

Order is the merge. Matt Pocock went in first, then the catalog's own skills, then each linked repo. An existing directory stays. A later skill with the same name gets a new directory, and `sources.json` records both.

Same job becomes one skill. Different jobs stay separate, and the extra one is slash-only (`disable-model-invocation: true`) so it does not auto-run on the same request.

Merged into one body:

- **Specs and docs.** `doc-coauthoring` is the procedure. It asks whether to grill or write from the conversation, and whether to publish to the issue tracker, a local file, or both. `/to-spec` and `/grill-with-docs` only start that skill. `domain-modeling` stays its own skill and is called when a term or an ADR is being decided.
- **Search.** The local SEO checklist and the Search Console workflow are both `seo-analysis`. The technical pass always runs. Rankings and PageSpeed run when there is a live URL. `seo-auditing` is gone.
- **Skill scanning.** `skill-scanner` always runs the static scan, then HOL Guard when that CLI is installed. `hol-guard` is gone.
- **Handoff.** `/handoff` remains. `claude-handoff` is gone.

Kept side by side, with the second name changed when the directory would collide:

- Sentry's code review is `sentry-code-review`, beside Matt Pocock's `code-review`.
- `tdd` stays automatic. `/writing-tests` and `/python-tdd-with-uv` are slash-only.
- `code-review`, `sentry-code-review`, `reviewing-code`, `parallel-code-review`, `find-bugs`, `security-review`, and `code-simplifier` each cover a different review. `code-simplifier` and `find-bugs` do not start just because the request was "review".

After a merge, copy the live tree back into this repo. Leave `~/.agents/skills` as the copy Cursor runs.

```bash
rsync -a --exclude '.env' --exclude '*.pem' ~/.agents/skills/ skills/
rsync -a ~/.cursor/skills/writing-shell-scripts skills/
```

Update `sources.json` for anything added, renamed, or removed. `local` is `writing-shell-scripts`. `removed` is `claude-handoff`, `hol-guard`, `seo-auditing`.

---
name: doc-coauthoring
description: >-
  Write a spec, proposal, RFC, decision doc, or other technical document.
  Use when the user wants a spec, a design doc, documentation, or to grill a
  plan into docs. Ask whether to grill first or write from what was already
  said, and whether to publish to the issue tracker, a local file, or both.
  Call domain-modeling when a term or ADR is being decided. /to-spec and
  /grill-with-docs start this same skill.
---

# Specs and docs

This is the one path for a spec, proposal, RFC, decision doc, or a plan that should become docs. `/to-spec` and `/grill-with-docs` start here too.

## Ask first

Ask both questions in one round, with a recommendation, and wait.

1. **How to start.** Grill until the open decisions are settled, or write from what has already been said.
2. **Where it goes.** The issue tracker, a local markdown file, or both.

Recommend grilling when the plan still has branches nobody has chosen. Recommend writing from the conversation when those choices are already made. Recommend the issue tracker for a spec an agent will implement. Recommend a local file for a doc people will read in the repo. Recommend both when it is a spec that should also live in the repo.

## Grill

Call the `grilling` skill. Work its rounds until the frontier is empty and the user confirms the shared understanding.

When a term is still fuzzy, or a decision should become an ADR, call `domain-modeling` and update `GLOSSARY.md` and `docs/adr/` as that skill describes. Do not invent a second glossary format.

Then write the document.

## Write from the conversation

Do not interview. Synthesize what was already said and what the repo shows.

Explore the repo if you have not. Use the glossary vocabulary. Respect ADRs in the area you are touching.

For a spec, sketch the test seams before publishing. Prefer an existing seam, at the highest point that still catches the behavior. The fewer seams, the better. Check with the user that the seams match what they expect. That check is the seam list, not a new design interview.

## What to write

A spec uses this template. Another doc (proposal, RFC, decision doc) uses the sections that fit, and skips user stories when there is no actor.

```markdown
## Problem Statement

The problem, from the user's perspective.

## Solution

The solution, from the user's perspective.

## User Stories

A long numbered list:

1. As an <actor>, I want <feature>, so that <benefit>

## Implementation Decisions

Modules, interfaces, schema, and API contracts that were decided.
No file paths or code snippets, unless a prototype snippet states a decision more precisely than prose. Keep only the decision-rich part.

## Testing Decisions

What a good test is here (external behavior, not internals), which modules are tested, and prior art in the repo.

## Out of Scope

## Further Notes
```

## Publish

**Issue tracker.** The tracker and triage labels should already be configured. If they are not, tell the user to run `/setup-matt-pocock-skills` and stop. Publish the spec and apply the `ready-for-agent` label.

**Local file.** Put a spec or decision doc under `docs/` unless the repo already has a place for it. Use the glossary and ADR paths from `domain-modeling` when the file is an ADR. Do not create a second docs convention.

**Both.** Publish to the tracker and write the same document locally. Say both paths when you are done.

After it is published, offer one reader check: a fresh pass that looks only at the document and lists what a new reader cannot understand. Do that pass only if they say yes.

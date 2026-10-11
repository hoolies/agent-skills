---
name: grill-with-docs
description: >-
  Start the doc-coauthoring skill. Suggest /grill-with-docs when a plan
  should be interviewed and written down. The doc-coauthoring skill also
  runs on its own when the user asks for a spec or a doc.
disable-model-invocation: true
---

Follow the `doc-coauthoring` skill in full. Ask whether to grill or write from the conversation, and whether to publish to the issue tracker, a local file, or both. When a term or ADR is being decided, that skill calls `domain-modeling`.

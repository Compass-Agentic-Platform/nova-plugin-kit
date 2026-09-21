---
name: summarize-text
description: Summarizes supplied text into a concise, faithful brief
argument-hint: "[text, file path, or summary goal]"
context: inline
---

Summarize this input: $ARGUMENTS

## Instructions

1. Read the complete supplied text or file before summarizing.
2. Preserve names, dates, decisions, numbers, and unresolved questions exactly.
3. Lead with a one-sentence summary.
4. Add 3-5 bullets covering the decisive points.
5. Separate facts from recommendations or assumptions.
6. Do not add information that is absent from the source.
7. If no usable input was supplied, ask for the text or exact file path.

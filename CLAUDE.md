# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A **data-only** repository: it publishes the 401 poems of *Kurunthogai* (குறுந்தொகை), a
classical Tamil Sangam anthology, as a single JSON file. There is no application, build
system, test suite, or dependencies — the "API" is the static JSON document itself.

The entire payload lives in `json/KurunthogaiPoems.json`.

## Data model

The file is one object with a single key `KurunthogaiPoems`, holding an array of 401 poem
objects. Every object has exactly these four keys:

| key                | meaning |
|--------------------|---------|
| `index`            | poem number, 1–401, contiguous and in order |
| `poem_verses`      | the full Tamil verse text (line breaks are not preserved — lines run together) |
| `poet_name`        | attributed poet in Tamil; empty string `""` when the poet is unknown/unattributed |
| `poem_thinai_type` | the *thinai* (landscape/mood) the poem belongs to |

`poem_thinai_type` is one of five values: `குறிஞ்சி`, `முல்லை`, `மருதம்`, `நெய்தல்`, `பாலை`.

### Known data issues (verify before relying on counts)

- One record uses `நெய்தல` (missing the final pulli ்) instead of `நெய்தல்`, so a naive
  group-by yields six thinai buckets instead of five.
- 11 poems have an empty `poet_name`.

When editing the JSON, preserve UTF-8 encoding, keep `index` contiguous, and keep the four
keys on every object.

## Working in this repo

- Validate after any edit: `python3 -m json.tool json/KurunthogaiPoems.json > /dev/null`
- The `.gitignore` is a stock multi-language template (Java/Python/Node/etc.); it is not a
  signal that any of those toolchains are used here.

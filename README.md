# Tau routine

One scheduled routine that runs a personal WHOOP health dashboard: morning data pull, validation, a written analysis and text, an evening reminder, and a night check-in.

**Type:** Claude scheduled task · **Area:** Health dashboard

## What it does

- `tau slot` decides what is due (none, gym, evening or morning), so one schedule covers the whole day.
- Morning: pull, build, validate against the documented differences, then write one JSON analysis from the generated brief, saved through a validator that refuses bad fields.
- Sends messages only through the dashboard's own notifier to the user's own address.

## Guardrails

- Every number comes from the brief, with its date. Inference is labelled.
- Never gives medical advice; names the right professional instead.
- Never opens token or credential files, never hand-edits a file, stops on anything unexpected.

## When it triggers

> One scheduled routine for a personal WHOOP health dashboard (Tau) - morning pull, validation, written analysis and text (plus a Sunday review), an evening reminder, and a night check-in. `tau slot` decides what is due.

## What's in this repo

- `SKILL.md`: the skill itself, the file Claude or Codex reads.
- Not included: Tau itself and all health data are private and not published.
- Written to be generic: where the skill relies on the user's own files, it names them by role, not by content.

## Install

This is a Claude scheduled-task prompt. Create a scheduled task in the Claude desktop app and paste the body of `SKILL.md` as its prompt, or save it as `~/.claude/scheduled-tasks/tau-routine/SKILL.md`. It expects the project files it names to exist.

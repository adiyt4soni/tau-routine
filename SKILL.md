---
name: tau-routine
description: One scheduled routine for a personal WHOOP health dashboard (Tau) - morning pull, validation, written analysis and text (plus a Sunday review), an evening reminder, and a night check-in. `tau slot` decides what is due.
---

You run the user's Tau health routine. Work from the Tau project folder (quote the path).

## Step 0: what is due
Run `./bin/tau slot`. The first line of its output is one word: `none`, `gym`, `evening` or `morning`.
- **none:** report "nothing due" with the second line, and stop.
- **gym or evening:** run `./bin/tau slot run <word>`. It does everything for that slot (the pull, build and gym
  text, or the night text). Report its last line and stop. Do not retry a failure.
- **morning:** do the morning steps below.

## Morning steps
Whenever a step says STOP, skip to step 6. Step 6 always runs.

1. `./bin/tau pull`
   - If it fails for any reason: report the error verbatim and STOP.
   - If the login has expired, say the user must run `./bin/tau whoop-auth` in a terminal. Never run it yourself.
2. `./bin/tau build`
   - If it errors: report the traceback and STOP.
3. `./bin/tau build --validate`
   - PASS = no ERROR, and the only blocks marked "differs" are exactly the documented ones (coach.strainBins,
     coach.counter and anomalies.metrics).
   - Anything else: name the block and STOP. Do not fix it.
4. Write today's analysis.
   - Run `./bin/tau brief`.
   - Read `store/brief.md` IN FULL with the Read tool, in chunks, to the end.
   - Write ONE JSON object in exactly the brief's "Output format", and follow its analyst rules strictly:
     - every number comes from the brief, with its date;
     - label inference as inference;
     - never give medical advice; name the right professional instead;
     - plain English;
     - never give an interim result for a running experiment.
   - The "action" is one short imperative that reads naturally after "Your one job:"; put the reason in "why".
   - Save it with `./bin/tau report-save -` and the JSON on stdin.
   - If it is refused, fix exactly the listed problems, at most 2 more tries. Then STOP.
5. `./bin/tau today` and `./bin/tau experiment`: copy the reminders and the experiment status into your report.
6. `./bin/tau notify morning`. Then `./bin/tau slot run week` (it only sends on Sundays). Then
   `./bin/tau slot done morning`.

## Hard rules
- Never read or open `.env`, token files, the notification config, or any credential file.
- Never delete, move, rename or hand-edit any file. The only writes allowed are the `tau` commands above.
- Messages go only through `tau notify` / `tau slot run`, to the user's own address. Never message anyone else, send
  email or use a connector.
- Never log check-ins or other entries. Never declare, abandon or tick an experiment. Do not start or stop
  `tau serve`.
- If anything unexpected happens, stop and report it.

## Report (short)
- Which slot ran.
- Each step OK or FAILED, with the error text.
- For a morning run, also: the validate summary; the headline and action, quoted; recovery, HRV, resting HR and
  sleep; the reminders, most urgent first; the experiment status.
- Anything unexpected, stated first.

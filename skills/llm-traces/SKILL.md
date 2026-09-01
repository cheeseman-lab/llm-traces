---
name: llm-traces
description: Use when the user wants to save, offboard, or archive a Claude session for the lab ("offboard this chat", "save this to LLM_traces"), when anyone asks whether the lab has done a task before ("has anyone done bulk RNA-seq?"), or before starting a substantial analysis or pipeline task that another Cheeseman lab member may already have worked through.
---

# llm-traces

Shared archive of distilled Claude sessions at `/lab/cheeseman_lab/LLM_traces`
(the ROOT below). Its purpose is that every approach the lab has already tried
— **especially the failed ones** — is tried exactly once. A trace without its
dead ends is considered incomplete.

Two operations: **find** (look up prior work) and **offboard** (save the
current session).

## Find — before starting new work, or when asked "has anyone done X?"

1. Read `ROOT/INDEX.md`. If thin matches, also `grep -ril <keywords>` across
   `ROOT/*/` GUIDE and LOG files.
2. On a match: read that trace's `GUIDE.md` **and** `LOG.md`. Report to the
   user: who did it, when, what worked, and the dead ends already ruled out.
3. Treat `do-not-retry` dead ends as settled; treat `retry-if:` dead ends as
   retryable only if the stated condition has changed. Never re-attempt a
   listed dead end without telling the user it previously failed and why.
4. No match: say so, proceed normally — and suggest offboarding at the end if
   the session turns out to be reusable.

## Offboard — when the user asks to save the session

Create exactly this structure (get username from `$USER`):

```
ROOT/<username>/YYYY-MM-DD_topic-slug/
├── GUIDE.md        # from ROOT/templates/GUIDE.md
├── LOG.md          # from ROOT/templates/LOG.md
├── scripts/        # working code from the session, runnable by others
└── transcript.md   # only if the user asks for the raw transcript
```

Checklist — every item, in order:

1. Propose a `topic-slug` (lowercase, hyphens) and 3–6 search tags; confirm
   with the user in one short question.
2. Copy both templates and fill every section. In `GUIDE.md`, generalize:
   modules/partitions/resources verbatim, project-specific paths replaced
   with marked `EDIT` placeholders.
3. `LOG.md` **Dead ends**: one block per abandoned approach — commands tried,
   verbatim error/symptom, cause, and a verdict (`do-not-retry` or
   `retry-if: <condition>`). Include approaches YOU tried and silently
   recovered from during the session (failed tool calls, wrong flags, OOM
   retries), not just user-visible failures. Write "None — first approach
   worked" only if literally true.
4. Copy scripts into `scripts/` (final working versions, not drafts).
5. **Append an entry at the top of `ROOT/INDEX.md`** in the format shown at
   the top of that file, including the `Dead ends:` line. A trace absent
   from INDEX.md does not exist — this step is not optional.
6. Show the user the GUIDE.md draft and the INDEX entry for a quick check.

## Common mistakes

| Mistake | Correction |
|---|---|
| Trace folder at ROOT top level | Always under `ROOT/<username>/` |
| One combined README | Separate GUIDE.md (recipe) and LOG.md (history + dead ends) |
| Skipping INDEX.md ("folder is discoverable anyway") | Grep-based discovery is the fallback, INDEX is the contract — append the entry |
| Dead ends summarized as "some issues with salmon" | Each dead end needs the actual error and a verdict, or it will be re-tried |
| Editing/reordering others' INDEX entries | INDEX is append-only, newest at top |

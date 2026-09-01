# llm-traces

Claude Code plugin for the Cheeseman lab's shared archive of distilled Claude
sessions at `/lab/cheeseman_lab/LLM_traces`.

The archive's purpose: every approach the lab has tried — **especially the
failed ones** — gets tried exactly once. A session about, say, bulk RNA-seq
gets offboarded into a trace (reusable `GUIDE.md`, `LOG.md` with structured
dead ends, working `scripts/`), registered in the archive's `INDEX.md`. Six
months later, "has anyone done bulk RNA-seq?" is answered by the archive —
including which approaches are known dead ends and why.

This repo holds the skill and templates. **Trace data never lives here** — it
stays on the lab share.

## Install (Claude Code, recommended)

In Claude Code:

```
/plugin marketplace add cheeseman-lab/llm-traces
/plugin install llm-traces@llm-traces
```

Then restart Claude Code (or `/reload-plugins`). Update later with
`/plugin update llm-traces@llm-traces`.

## Install (claude.ai)

On the Plugins page: **Add marketplace** → `cheeseman-lab/llm-traces`, then
install the `llm-traces` plugin from it. Alternatively, upload the skill zip
from `/lab/cheeseman_lab/LLM_traces/llm-traces-skill.zip` via Settings →
Skills.

Caveat: claude.ai cannot reach the lab filesystem, so there the skill only
helps *format* a trace (GUIDE/LOG/scripts to download and copy to the share
yourself) — lookup and INDEX updates need Claude Code on the cluster.

## Install (symlink, no plugin system)

```bash
mkdir -p ~/.claude/skills
ln -s /lab/cheeseman_lab/LLM_traces/skills/llm-traces ~/.claude/skills/llm-traces
```

Use one method or the other, not both (both = the skill loads twice).

## Usage

- End of a useful session: *"offboard this chat into LLM_traces"*
- Before starting an analysis: *"has anyone in the lab done X?"* — or Claude
  checks the archive on its own when the task looks like something the lab
  may have solved before.

Conventions, layout, and templates are documented in
`/lab/cheeseman_lab/LLM_traces/README.md` (the archive itself).

## Repo layout

```
.claude-plugin/   plugin.json + marketplace.json (repo is its own marketplace)
skills/llm-traces/SKILL.md   the skill (canonical copy)
templates/        GUIDE.md + LOG.md seeds for the archive's templates/
```

The lab share's `skills/llm-traces` tracks this repo via a clone at
`/lab/cheeseman_lab/LLM_traces/.repo` — after merging skill changes here, run
`git -C /lab/cheeseman_lab/LLM_traces/.repo pull` so symlink installs pick
them up.

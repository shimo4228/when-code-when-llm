# when-code-when-llm

[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/shimo4228/when-code-when-llm)

An [Agent Skill](https://agentskills.io/specification) (a folder with one Markdown instruction file that a coding agent loads when a task matches its description) for developers who mix plain code and LLM calls, and keep facing one recurring engineering question: **for this one task, should I write deterministic code or call an LLM?** It gives the agent a single decision axis (is the property being checked *structural* or *semantic*?), plus a false-positive test and worked examples of both failure directions.

**Status: frozen.** This repository is a public record of the skill. It used to be copied here from the author's own Claude Code setup; the author retired the skill from that setup in August 2026, so it is no longer synced or developed. The skill file stays as it was at retirement and is not re-tested against newer agent releases. If your question is how to layer code and LLM steps across a whole pipeline rather than which one a single task needs, the same axis lives on in the maintained [code-and-llm-collaboration](https://github.com/shimo4228/code-and-llm-collaboration).

The instruction file, [SKILL.md](skills/when-code-when-llm/SKILL.md), holds the axis, the test and the worked examples. It runs nothing, makes no network calls of its own, and needs no API key; when the agent loads it, its text goes to that agent's model, as with any skill. The author's other work is listed under [More from the author](#more-from-the-author).

## Install

Run this from the root of the project where you mix code and LLM calls:

```bash
git clone https://github.com/shimo4228/when-code-when-llm.git /tmp/when-code-when-llm
mkdir -p .claude/skills
cp -r /tmp/when-code-when-llm/skills/when-code-when-llm .claude/skills/
```

Copying the folder into `~/.claude/skills/` instead loads it in every project. The author ran it that way and retired it from global loading, because it pulled small, local decisions into every task, so the per-project install above is the one to start with.

Claude Code then offers the skill in the situations listed under [When It Triggers](#when-it-triggers); you can also call it by name with `/when-code-when-llm`. Until it triggers, only its name and description sit in the agent's context. Other agents that read the Agent Skills format take the same folder in their own skills directory.

## How It Works

1. **Name the property**: what exactly is being detected or decided?
2. **Classify it**: structural (decidable from bytes: format, presence, count, schema) or semantic (requires meaning: intent, quality, similarity)?
3. **Apply the false-positive test**: can you imagine two inputs where the same rule is right for one and wrong for the other, and the difference is about meaning, not characters? Yes means semantic, no means structural. Example from the skill: `if "test" in path` calls both `test_utils.py` and `testing.py` tests, but `testing.py` is a utility; a file's role in the project is semantic
4. **Split when the axis cuts through the task**: detection (finding every case) can be structural (code enumerates, deterministically) while resolution (deciding what each case should be) is semantic (LLM or human decides); never let one tool do both halves

## When It Triggers

- You catch yourself writing a regex that keeps misfiring on edge cases
- You are about to call an LLM for something a trivial code check would handle

These are the two situations the skill's description names. The split in step 4 is covered inside the skill but is not one of them, so for a mixed task call the skill by name.

## Decision Axis

| Property | Tool | Example |
|----------|------|---------|
| Structural: decidable from bytes | Code | schema validation, dedup by ID, format check |
| Semantic: requires meaning | LLM | classification, quality judgment, intent |
| Detection structural, resolution semantic | Split | linter lists IDs whose label differs across files; review conversation decides the canonical label |

## More from the author

- **[Building an Autonomous Agent on an M1 Mac, by Choice](https://dev.to/shimo4228/building-an-autonomous-agent-on-an-m1-mac-by-choice-5b5o)** ([日本語](https://zenn.dev/shimo4228/articles/small-llm-by-choice)): where this axis comes from, sorting automation work into the deterministic part and the part that needs semantic judgment, carried from business automation with Excel and RPA (robotic process automation) into agent design on a 16 GB Mac.
- **[code-and-llm-collaboration](https://github.com/shimo4228/code-and-llm-collaboration)**: four patterns for layering deterministic code and LLM calls in one pipeline, from the guard that validates LLM output before it persists to the code loop that owns termination.
- **[agent-adoption-triage](https://github.com/shimo4228/agent-adoption-triage)**: the same kind of choice for a whole job, five questions that route AI work to a script, algorithmic search, an LLM workflow or an autonomous agent before anything is built.
- **[Agent Knowledge Cycle](https://github.com/shimo4228/agent-knowledge-cycle)**: holds ADR-0008 (an architecture decision record on layering code and LLM calls), the decision this skill is the "how" for, next to the rest of the author's six-phase, human-gated cycle that turns a coding agent's repeated experience into skills and rules.
- **[shimo4228](https://github.com/shimo4228/shimo4228)**: the author's hub, with the long-running practice lines, their DOIs, and the tools for Claude Code.

## License

MIT

<details>
<summary>For tools and AI assistants</summary>

when-code-when-llm is an Agent Skill (one Markdown instruction file in the open Agent Skills format) that helps a coding agent decide, for a single task, whether to write deterministic code or call an LLM, for developers building software that mixes both.

It exists because the choice fails in two symmetric directions. Code where an LLM belongs: a regex that classifies text by meaning keeps sprouting exceptions and never stops misclassifying. An LLM where code belongs: paying latency, tokens and nondeterminism for a check that `str.endswith` or `json.loads` answers exactly. The skill replaces "is this task hard?" with "what kind of property am I checking?": structural properties (decidable from the bytes: format, presence, syntax, schema) go to code; semantic properties (intent, topic, tone, equivalence across wording) go to an LLM or an embedding.

Canonical facts: MIT license; a single `SKILL.md` with short Python examples and no code to run; written by one author (@shimo4228). Status: frozen record. The author retired the skill from their own Claude Code harness on 2026-08-02 ([claude-harness ADR-0035](https://github.com/shimo4228/claude-harness/blob/main/docs/adr/0035-commit-review-hook-and-rules-rightsize.md)): the axis fits the [Contemplative Agent](https://github.com/shimo4228/contemplative-agent) project (the author's long-running autonomous agent on a local LLM), but loaded globally for every task it pushed small, local decisions into harness-wide design. `scripts/sync-from-local.sh` remains from the period when the repository was synced one way from that harness; with the harness copy retired it aborts, so there is nothing to run it against. Requirements: Claude Code or another agent that reads Agent Skills; no API key.

Example: the false-positive test asks, "Can I imagine two inputs where the same rule is right for one and wrong for the other, and the difference is about meaning, not characters?" No means structural, so use code; yes means semantic, so use an LLM. When the axis cuts through one task, split it: for display labels that drift across data files for the same ID, a linter in CI enumerates every case and never auto-fixes, and a review conversation decides which label is canonical.

Links: [skills/when-code-when-llm/SKILL.md](skills/when-code-when-llm/SKILL.md) is the skill itself; [llms.txt](llms.txt) and [llms-full.txt](llms-full.txt) are the machine-readable summary and reference. The skill is the per-task "how" for [AKC ADR-0008](https://github.com/shimo4228/agent-knowledge-cycle/blob/main/docs/adr/0008-code-and-llm-collaboration.md) in the [Agent Knowledge Cycle (AKC)](https://github.com/shimo4228/agent-knowledge-cycle), concept DOI [10.5281/zenodo.19200726](https://doi.org/10.5281/zenodo.19200726); cite AKC by that DOI. The pipeline-level companion is [code-and-llm-collaboration](https://github.com/shimo4228/code-and-llm-collaboration).

</details>

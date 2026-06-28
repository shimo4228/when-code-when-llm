# when-code-when-llm

[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/shimo4228/when-code-when-llm) [![GitMCP](https://img.shields.io/endpoint?url=https://gitmcp.io/badge/shimo4228/when-code-when-llm)](https://gitmcp.io/shimo4228/when-code-when-llm) [![View Code Wiki](https://assets.codewiki.google/readme-badge/static.svg)](https://codewiki.google/github.com/shimo4228/when-code-when-llm)

An [Agent Skill](https://agentskills.io/specification) that answers one recurring engineering question: **for this one task, should I write deterministic code or call an LLM?** It gives the agent a single decision axis — is the property being checked *structural* or *semantic* — plus a false-positive test and worked examples of both failure directions.

## Install

### Claude Code

```bash
# Copy into your global skills directory
cp -r skills/when-code-when-llm ~/.claude/skills/when-code-when-llm
```

### SkillsMP

```bash
/skills add shimo4228/when-code-when-llm
```

## How It Works

1. **Name the property** — what exactly is being detected or decided?
2. **Classify it** — structural (decidable from bytes: format, presence, count, schema) or semantic (requires meaning: intent, quality, similarity)?
3. **Apply the false-positive test** — if a regex keeps producing false positives/negatives, the property is semantic; if an LLM is being asked something a three-line check answers, it is structural
4. **Split when the axis cuts through the task** — detection can be structural (code enumerates, deterministically) while resolution is semantic (LLM or human decides); never let one tool do both halves

## When It Triggers

- You catch yourself writing a regex that keeps misfiring on edge cases
- You are about to call an LLM for something a trivial code check would handle
- A single task mixes exact detection with judgment-based resolution

## Decision Axis

| Property | Tool | Example |
|----------|------|---------|
| Structural — decidable from bytes | Code | schema validation, dedup by ID, format check |
| Semantic — requires meaning | LLM | classification, quality judgment, intent |
| Detection structural, resolution semantic | Split | linter enumerates label drift; review conversation decides the canonical label |

## Syncing from the harness

The canonical copy of this skill lives in the author's live Claude Code harness. This repository is a one-way publication mirror:

```bash
scripts/sync-from-local.sh --dry-run   # report differences only
scripts/sync-from-local.sh             # apply to working tree (never commits)
```

## Related skill

[code-and-llm-collaboration](https://github.com/shimo4228/code-and-llm-collaboration) applies the same structural-vs-semantic axis at the next level up: not "which tool for this one task" but "how do code layers and LLM layers compose in one pipeline."

## About this skill

This skill is a design-pattern skill from the [Agent Knowledge Cycle (AKC)](https://github.com/shimo4228/agent-knowledge-cycle) research line — a Zenodo-citable six-phase bidirectional growth loop ([DOI 10.5281/zenodo.19200726](https://doi.org/10.5281/zenodo.19200726)) for sustaining intent alignment between an AI agent and its operator over time. It is the "how" counterpart to [AKC ADR-0008](https://github.com/shimo4228/agent-knowledge-cycle/blob/main/docs/adr/0008-code-and-llm-collaboration.md). AKC is one of three research lines by [@shimo4228](https://github.com/shimo4228), alongside [Contemplative Agent](https://github.com/shimo4228/contemplative-agent) ([DOI 10.5281/zenodo.19212118](https://doi.org/10.5281/zenodo.19212118)) — autonomous agents grounded in four contemplative axioms — and [Agent Attribution Practice (AAP)](https://github.com/shimo4228/agent-attribution-practice) ([DOI 10.5281/zenodo.19652013](https://doi.org/10.5281/zenodo.19652013)) — harness-neutral ADRs on accountability distribution.

## License

MIT

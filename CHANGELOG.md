# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] — 2026-06-12

Initial release as a standalone skill repository.

### Added

- `skills/when-code-when-llm/SKILL.md` — decision framework for choosing between deterministic code and LLM-based processing for a single task: the structural-vs-semantic axis, the false-positive test, worked examples of both failure directions, and the enumerate/decide split for tasks where detection is structural but resolution needs judgment.
- `scripts/sync-from-local.sh` — one-way export from the live Claude Code harness; the harness copy is canonical, this repository is the publication mirror.

### Notes

- Previously published inside the [Agent Knowledge Cycle](https://github.com/shimo4228/agent-knowledge-cycle) repository as a design-pattern skill under `docs/skills/`. Extracted here so the skill is installable on its own; the paired decision record [AKC ADR-0008](https://github.com/shimo4228/agent-knowledge-cycle/blob/main/docs/adr/0008-code-and-llm-collaboration.md) stays in the research repository.

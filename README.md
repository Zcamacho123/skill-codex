# Codex Integration for Claude Code

Delegate reviews, implementation, debugging, and refactoring from Claude Code to the OpenAI Codex CLI. This fork adds task-based model defaults and a concise workflow with supporting references.

## Requirements

- Claude Code with plugin support.
- Codex CLI on `PATH`, authenticated and configured for the intended workspace.
- Access to the selected model. Verify with the installed client; availability varies.

## Install this fork

In Claude Code:

```text
/plugin marketplace add Zcamacho123/skill-codex
/plugin install skill-codex@skill-codex
```

This fork keeps the upstream marketplace/plugin names. If the upstream marketplace is already registered as `skill-codex`, remove that marketplace entry before adding this fork so the name resolves to the intended source. Inspect installed plugins in `/plugin`.

For local development, clone this repository, enter it, then run:

```bash
claude --plugin-dir ./plugins/skill-codex
```

For a standalone installation, copy `plugins/skill-codex/skills/codex/` from this checkout, including `references/`, into `~/.claude/skills/codex/`. Review or back up an existing installation first. Standalone copies need manual updates.

## Use

```text
/skill-codex:codex Review this entire codebase for architecture and correctness risks.
/skill-codex:codex Review my settings feature against main, including uncommitted changes.
/skill-codex:codex Implement the agreed validation rules and run the relevant tests.
/skill-codex:codex Use Astra xhigh to investigate this cross-service race condition.
```

With the standalone installation, use `/codex`. Natural-language requests such as “Use Codex to review this feature” also match the skill.

## Default models

These are recommended presets for this plugin, not benchmark rankings. An explicit model or effort overrides its corresponding preset; existing preferences carry forward.

| Work | Default | Effort |
| --- | --- | --- |
| Whole-codebase / architecture review | GPT-6 Astra | Medium |
| Feature / PR review | GPT-6.1 Sol | Medium |
| Implementation, routine debugging, refactoring, tests | GPT-6.1 Sol | Medium |
| Difficult cross-system, security, or concurrency investigation | GPT-6 Astra | High |
| Deep unresolved design analysis | GPT-6 Astra | Extra high |
| Mechanical edits, diff summaries, extraction | GPT-6 Luna | Low |

The general fallback is Sol 6.1 medium. Claude states the selection and proceeds without asking you to pick a model on every run. Read the [routing guide](plugins/skill-codex/skills/codex/references/model-routing.md) for exact IDs, escalation, availability, and dated official sources.

## Execution behavior

Reviews use read-only access; requested implementation uses workspace-write. The skill preserves runtime approvals, captures diagnostics, uses literal prompt input, and prefers a recorded session ID for follow-up. It checks findings and diffs before reporting success. It does not automatically broaden permissions, fix review findings, or change your global model configuration.

## Maintenance and validation

- [Core skill](plugins/skill-codex/skills/codex/SKILL.md)
- [CLI examples and failure handling](plugins/skill-codex/skills/codex/references/cli-workflows.md)
- [Plugin practices and validation cases](plugins/skill-codex/skills/codex/references/plugin-practices.md)

```bash
claude plugin validate ./plugins/skill-codex
claude plugin validate .
```

When releasing changes, keep all package versions aligned and test installation plus representative task invocations in a disposable repository.

Based on [skills-directory/skill-codex](https://github.com/skills-directory/skill-codex). Original attribution and MIT license are retained.

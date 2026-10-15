# Claude Code plugin and skill practices

Apply when maintaining this integration. Checked against official documentation on **2026-10-15**.

## Packaging

- Keep the manifest at `plugins/skill-codex/.claude-plugin/plugin.json`, with `skills/` beside `.claude-plugin/`. The repository marketplace points to that plugin directory.
- Bundle references inside the plugin; cached installations must not depend on files outside their plugin root.
- Use skill-relative reference links. For future scripts/hooks, use `${CLAUDE_PLUGIN_ROOT}` in supported plugin configuration instead of assuming the working directory.
- Align plugin, marketplace, and `openpackage.yml` versions and bump them for released changes.
- Invoke the plugin as `/skill-codex:codex`, or the standalone skill as `/codex`. Point installation URLs at this fork and preserve attribution.

## Skill design

- Keep the core workflow concise; load references on demand. Describe concrete triggers and exclusions.
- Allow loading when the user explicitly requests Codex delegation. For a separate deployment or other side-effect command, consider `disable-model-invocation: true` for user-controlled invocation.
- Avoid broad `allowed-tools` grants just to suppress prompts; runtime permissions remain authoritative.
- Claude Code frontmatter `model` selects the Claude executor, not the OpenAI model. Select Codex models in CLI arguments.
- Add `context: fork` only for a deliberately isolated Claude workflow with a complete task; it does not select Codex or automatically preserve conversation context.
- Avoid dynamic shell injection just to collect context readable after invocation. Keep task text out of executable shell code.

## Workflow quality

Give each task a concrete goal, bounded scope, and verification criteria. Plan larger changes before implementation. Verify with relevant tests, source evidence, or screenshots. Keep persistent project conventions in `CLAUDE.md` and optional delegation instructions in this skill. Retain only useful logs and context.

## Release validation

From the repository root, when Claude Code is installed:

```bash
claude plugin validate ./plugins/skill-codex
claude plugin validate .
claude --plugin-dir ./plugins/skill-codex
```

Verify `/skill-codex:codex` appears. Exercise these cases in a disposable repository with Codex available; inspect actual commands and results.

| Request | Expected behavior |
| --- | --- |
| “Use Codex to review this whole codebase” | Astra medium; read-only; no model questionnaire |
| “Use Codex to review this feature against main” | Sol 6.1 medium; read-only; explicit comparison scope |
| “Use Codex to implement this acceptance checklist” | Sol 6.1 medium; workspace-write; verify changes |
| “Use Codex to summarize this diff cheaply” | Luna low; read-only |
| “Use Codex with Astra xhigh on this feature” | Preserve both explicit overrides |
| “Resume the earlier architecture review” | Recorded ID and workspace; no unrelated latest session |
| “Explain what Codex is” | Answer without launching the CLI |
| Requested model unavailable | Report the error; do not silently substitute |
| Prompt contains backticks or `$(...)` | Deliver literal text; do not execute it in the shell |
| Review finds a bug | Report evidence; do not edit without authorization |

Validate JSON/YAML, links, version consistency, and whitespace. If CLIs are unavailable, distinguish structural checks and simulated cases from live integration tests.

## Sources

- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Plugin manifest reference](https://code.claude.com/docs/en/plugins-reference)
- [Create a marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices)

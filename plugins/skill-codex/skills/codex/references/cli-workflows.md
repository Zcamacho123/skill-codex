# CLI workflows

Check installed help first. Examples use Bash and existing authentication. Adapt paths and task text; run an example task only for a corresponding user request.

## New review

Run from the intended repository root. A quoted heredoc preserves backticks, dollar signs, and quotes literally. Use private, unique run files.

```bash
codex_run_dir=$(mktemp -d "${TMPDIR:-/tmp}/codex-run.XXXXXX")
cat > "$codex_run_dir/prompt.txt" <<'CODEX_PROMPT'
Review this codebase's architecture and correctness risks. Read applicable repository
instructions. Do not edit files. Prioritize actionable issues; give file/line evidence,
impact, and a suggested verification for each. State what you could not verify.
CODEX_PROMPT
codex_status=0
codex exec -m gpt-6-astra \
  -c 'model_reasoning_effort="medium"' \
  --sandbox read-only \
  -o "$codex_run_dir/final.txt" \
  - < "$codex_run_dir/prompt.txt" \
  > "$codex_run_dir/stdout.log" 2> "$codex_run_dir/stderr.log" \
  || codex_status=$?
printf 'Exit: %s\nRun files: %s\n' "$codex_status" "$codex_run_dir"
```

Inspect the final answer, status, and relevant diagnostics. Do not publish private raw logs. Keep run files until follow-up is complete, then clean up only files created for that run.

For a feature review, use Sol 6.1 medium and specify the actual comparison base, requirements, and uncommitted changes. For authorized edits, use workspace-write and explicit acceptance checks. Avoid convenience flags that can alter sandbox settings.

With a positional prompt, close unused stdin in noninteractive harnesses:

```bash
codex exec -m gpt-6-luna -c 'model_reasoning_effort="low"' \
  --sandbox read-only 'Summarize the current diff without editing files.' </dev/null
```

Use `--json` for structured progress or session-ID extraction: stdout becomes JSONL events. Keep `-o` for the final message. Normal progress streams to stderr; an empty final file during execution does not establish a hang.

## Resume

Prefer the recorded session ID; `--last` can select another task. Check local `resume --help`, use the original workspace, and confirm effective model, effort, and permissions. Do not promise every CLI version preserves all settings.

```bash
# Replace the example ID with the recorded session ID.
codex_run_dir=$(mktemp -d "${TMPDIR:-/tmp}/codex-resume.XXXXXX")
codex_status=0
codex exec --sandbox read-only resume SESSION_ID \
  'Check whether your first finding is reachable from the public API. Do not edit.' \
  </dev/null > "$codex_run_dir/final.txt" 2> "$codex_run_dir/stderr.log" \
  || codex_status=$?
printf 'Exit: %s\nRun files: %s\n' "$codex_status" "$codex_run_dir"
```

This plain-output resume captures the final message from stdout. If using JSON events instead, use the installed CLI's supported final-answer output option. Pass recorded model/effort overrides when needed to retain the original selection, using positions confirmed by local help.

Use `codex exec --sandbox read-only resume --last 'Follow-up question' </dev/null` only when the intended latest session is unambiguous. For authorized fixes after review, explicitly use the supported workspace-write setting. Preserve resume diagnostics as for new runs.

## Failure handling

- Missing executable: report setup is needed; do not fabricate a run.
- Invalid flag: consult local help and correct the invocation before one retry.
- Authentication, quota, or model access: report the concrete error without credentials.
- Timeout: inspect process state and logs before restarting; distinguish active, failed, and interrupted work.
- Partial edits: inspect the diff, preserve user work, and report incomplete checks. Do not reset the workspace wholesale.

Sources, checked 2026-10-15: [CLI reference](https://learn.chatgpt.com/docs/developer-commands?surface=cli), [non-interactive mode](https://learn.chatgpt.com/docs/non-interactive-mode), and [security](https://learn.chatgpt.com/docs/security).

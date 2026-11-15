# Model routing

Last checked: **2026-11-15**. These are this plugin's recommended starting presets, not OpenAI product defaults or measured rankings. Validate quality on representative tasks and adjust for cost and latency preferences.

## Task defaults

| Task | Model ID | Effort | Intended use |
| --- | --- | --- | --- |
| General delegation / unclear category | `gpt-6.1-sol` | `medium` | Balanced starting point after the task itself is clear |
| Whole-codebase review | `gpt-6-astra` | `medium` | Trace architecture, module boundaries, and cross-cutting risks |
| Architecture or migration assessment | `gpt-6-astra` | `medium` | Compare alternatives and downstream consequences |
| Feature, PR, or diff review | `gpt-6.1-sol` | `medium` | Check changed behavior, regressions, edge cases, and tests |
| Feature implementation | `gpt-6.1-sol` | `medium` | Build against concrete requirements and acceptance checks |
| Routine debugging / refactoring | `gpt-6.1-sol` | `medium` | Reproduce a bounded problem or improve a defined component |
| Test design and coverage review | `gpt-6.1-sol` | `medium` | Identify behavioral gaps and regression checks |
| Difficult cross-system bug | `gpt-6-astra` | `high` | Investigate competing hypotheses across services or layers |
| Security-sensitive or concurrency review | `gpt-6-astra` | `high` | Examine trust boundaries, authorization, races, and failure paths |
| Deep design or unresolved complex analysis | `gpt-6-astra` | `xhigh` | Escalate when medium/high leaves material uncertainty |
| Small mechanical edits | `gpt-6-luna` | `low` | Apply precise renames, formatting, or simple transformations |
| Diff summary / documentation extraction | `gpt-6-luna` | `low` | Produce concise descriptions grounded in supplied changes |

Route mixed work by its hardest material risk. A small authorization diff can warrant Astra high. A broad mechanical replacement can still fit Luna. Use one run by default; split only when independent subtasks justify coordination and usage.

## Optional specialist choice

Consider **`gpt-6.1-terra` at `medium`** for screenshot interpretation, visual UI feedback, concise technical writing, or a scoped code fix with clear verification. This is an optional recommendation; keep the established coding/review defaults above. Compare Terra with Sol on representative tasks before changing a project preference.

Terra was released on **2026-10-26** and is documented for Codex CLI **0.159.0 or later**. Check both the installed version and account catalog before selecting it. A sufficient CLI version does not establish account access. Do not install upgrades or silently substitute Terra for an explicit model choice.

## Selection and escalation

Apply precedence separately to model and effort: current explicit request → established user/project preference → task preset. Keep the same choices on related resumes unless the user requests a change or an explained escalation is needed. These presets do not modify global Codex configuration.

Improve missing context and acceptance criteria before increasing effort. Move from medium to high for deeper investigation, then consider Astra for broader judgment. Reserve `xhigh` for demanding unresolved work. Select `max` or `ultra` only when requested or clearly justified within the user's agreed usage budget; announce the choice.

## Current model families and compatibility

The core coding presets use **GPT-6 Astra**, **GPT-6.1 Sol**, and **GPT-6 Luna**, with **GPT-6.1 Terra** as an optional specialist choice. Official guidance positions Astra for demanding work, Sol 6.1 for complex work at lower cost, and Luna for focused, repeatable workloads. Our routing table applies that guidance to development tasks.

`gpt-6-sol` and GPT-5.6 models (`gpt-5.6-sol`, `gpt-5.6-terra`, `gpt-5.6-luna`) are compatibility choices only when exposed by the installed client/account. Do not assume they alias Sol 6.1 or automatically replace an explicit choice.

Do not recommend retired defaults. Official Codex documentation lists GPT-5.3-Codex-Spark as retired, GPT-5.4/mini and GPT-5.5 as retired for ChatGPT sign-in, and GPT-5.2/GPT-5.3-Codex as deprecated for ChatGPT sign-in. API-key availability can differ; check explicit legacy requests independently. The API deprecations page now schedules `gpt-5.3-codex` shutdown for **2027-04-01**, with `gpt-6-sol` as its listed replacement; distinguish that future API shutdown from its existing ChatGPT-sign-in deprecation.

GPT-6 Cyber (`gpt-6-cyber`), released **2026-11-05**, requires Daybreak Red access and separate model approval. Do not treat its API listing as general Codex CLI availability or automatically replace the Astra high security-review preset. Verify access and the supported execution path for an explicit request.

## Reasoning and availability

Use `low`, `medium`, `high`, or `xhigh` for ordinary CLI presets, subject to the installed catalog. UI “Light” maps to `low`; “Extra High” maps to `xhigh`.

Codex exposes `max` and `ultra` only for supported model/client/account combinations. Ultra involves subagents; Luna does not support Ultra. The Sol 6.1 API reasoning list ends at `max`; do not transfer Codex-specific Ultra settings to API requests.

Terra's API supports `none`, `low`, `medium` (default), `high`, `xhigh`, and `max`. This does not establish which efforts the installed Codex client exposes: validate its catalog, including any request for `none` or Ultra. Keep the optional Terra preset at `medium` when supported.

Sol 6.1 currently defaults to a **500,000-token context window in Codex/Work**; its API specification is separate. Do not size a CLI task from the API context limit alone.

If a preset is unavailable, inspect the actual catalog and report the constraint. For an implicit default, choose an available alternative suited to the task and state the substitution; do not silently raise cost. For an explicitly selected unavailable model/effort, explain the blocker and ask for a supported alternative. Never invent an identifier or repeatedly retry a rejected combination.

## Sources and refresh

Official sources, checked 2026-11-15:

- [Codex models, availability, and retirements](https://learn.chatgpt.com/docs/models)
- [OpenAI model selection](https://developers.openai.com/api/docs/guides/model-selection)
- [API release notes](https://developers.openai.com/api/docs/changelog)
- [API model catalog](https://developers.openai.com/api/docs/models)
- [API deprecations](https://developers.openai.com/api/docs/deprecations)
- [GPT-6.1 Terra API specification](https://developers.openai.com/api/docs/models/gpt-6.1-terra)
- [GPT-6 Cyber access requirements](https://developers.openai.com/api/docs/models/gpt-6-cyber)
- [GPT-6.1 Sol API specification](https://developers.openai.com/api/docs/models/gpt-6.1-sol)

On release or model-access error, recheck these pages and the installed catalog. Update the date, IDs, and affected presets together. Separate capabilities from account access; avoid fixed price or speed promises.

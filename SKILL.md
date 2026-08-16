---
name: codex-council
description: Consult multiple independent AI providers or a clearly labeled same-model Codex panel, then synthesize consensus, meaningful disagreement, assumptions, risks, and one recommended direction. Use whenever the user asks to "ask the council", "run this past the council", get multiple AI opinions, compare model perspectives, convene a review panel, or requests a council status/configuration check.
---

# Codex Council

Gather independent perspectives without presenting agreement as proof. Prefer external providers for true cross-vendor review; use Codex subagents only as a disclosed fallback.

## Choose the mode

1. Run `scripts/council.py status` with an available Python 3 executable.
   - On Codex Desktop, call `codex_app__load_workspace_dependencies` when `python` is not on `PATH`, then use its Python path.
2. Use **cross-vendor mode** when one or more API providers or Ollama are ready.
3. Use **local Codex mode** only when the user explicitly asks for it or no independent provider is ready and the user accepts the fallback.
4. Never describe same-model subagent agreement as independent confirmation.

Read [references/providers.md](references/providers.md) when configuring providers, diagnosing status, or selecting models.

## Cross-vendor workflow

Before transmitting data, state which providers will receive the prompt and whether any file content is included. Do not send secrets, credentials, medical records, personnel data, or unrelated proprietary files. Treat user instructions to consult the council as authorization to send the stated question, not as blanket authorization to upload workspace files.

Run:

```text
<python> <skill-dir>/scripts/council.py ask --providers=gemini,openai,grok -- "question"
```

Useful options:

- `--roles=balanced|architecture|review|security-focused` or a comma-separated role list
- `--verbosity=brief|standard|detailed`
- `--debate` for a second critique round; warn that it roughly doubles requests and cost
- `--file=<path>` only after confirming the file is in scope for external transmission
- `--output=<path>` to persist the raw transcript; otherwise it is printed only

If the sandbox blocks network access, retry the same command with the normal approval mechanism. Never expose API-key values in command text or output.

Display each provider response substantially as returned. Name provider failures and exclude them from agreement claims.

## Local Codex workflow

This skill explicitly permits using collaboration subagents when the user requested a council and independent providers are unavailable or local mode was requested.

1. Lead with: **Local Codex council — these perspectives use the same model family with different roles, not independent vendors. Agreement is a shared starting point, not corroboration.**
2. Select three roles suited to the question, normally from security, performance, maintainability, devil's advocate, simplicity, scalability, developer experience, and compliance.
3. Spawn the three role prompts in parallel when collaboration tools and slots are available. Keep members blind to one another in the first round.
4. If collaboration tools are unavailable, produce three explicitly role-framed analyses locally and disclose that they came from one model invocation.
5. Wait for all available perspectives, note failures, then synthesize.

## Synthesis contract

End with:

- **Shared conclusions** — only points supported by successful members
- **Meaningful disagreements** — differences that could change the decision
- **Unchecked assumptions** — premises no member could verify
- **Risks and blind spots** — including anything the panel omitted
- **Recommendation** — one concrete direction and the condition that would reverse it

When all members agree, explicitly identify the load-bearing assumption to verify first. Do not use vote counts as a substitute for evidence.

## Status requests

Run `scripts/council.py status`. Report configuration presence and local Ollama reachability without revealing keys. Status does not make paid model requests.

## Provenance

This is a Codex-native adaptation inspired by `hex/claude-council`. It does not install Claude commands, hooks, or `${CLAUDE_PLUGIN_ROOT}` behavior. Read [references/upstream-license.md](references/upstream-license.md) for attribution and license terms.

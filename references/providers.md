# Provider configuration

The runner uses environment variables and sends HTTPS JSON requests directly. It never writes or prints API keys.

| Provider | Required variable | Model override | Default model |
|---|---|---|---|
| Gemini | `GEMINI_API_KEY` | `GEMINI_MODEL` | `gemini-3.1-pro-preview` |
| OpenAI | `OPENAI_API_KEY` | `OPENAI_MODEL` | `gpt-5.6-sol` |
| Grok | `XAI_API_KEY` or `GROK_API_KEY` | `GROK_MODEL` | `grok-4.5` |
| Perplexity | `PERPLEXITY_API_KEY` | `PERPLEXITY_MODEL` | `sonar-reasoning-pro` |
| Kimi | `KIMI_API_KEY` or `MOONSHOT_API_KEY` | `KIMI_MODEL` | `kimi-k3` |
| Ollama | Running local service | `OLLAMA_MODEL`, `OLLAMA_HOST` | First installed model |

Optional shared settings:

- `COUNCIL_MAX_TOKENS`: visible/reasoning output budget. Default is 2048, with a larger floor for reasoning models.
- `COUNCIL_TIMEOUT`: request timeout in seconds. Default is 180.
- `OPENAI_REASONING_EFFORT`: `low`, `medium`, or `high`; default `medium`.
- `PERPLEXITY_RECENCY`: `day`, `week`, `month`, or `year`.
- `KIMI_ENDPOINT`: override the Moonshot-compatible endpoint.

## Windows

Codex Desktop may not expose `python` on `PATH`. Resolve the bundled Python executable with `codex_app__load_workspace_dependencies` and invoke `scripts/council.py` with that absolute path.

Set persistent keys through the operating system's secure environment/credential workflow. Do not place keys in project files, shell history, Git configuration, prompts, or council transcripts.

## Selection

- Prefer at least two genuinely independent providers for consequential decisions.
- Do not count OpenAI API plus Codex subagents as independent vendors.
- Use Perplexity when current web-grounded context matters, but verify citations separately.
- Use Ollama when source content must remain local.
- Use `--providers=all` to query every configured provider.

## Privacy and cost

External mode transmits the complete assembled prompt and any `--file` contents to each selected provider. Debate mode transmits other providers' responses back to every selected provider and roughly doubles requests. Persisted `--output` transcripts can contain sensitive prompt and file content.

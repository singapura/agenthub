# AgentHub

A single-file, zero-install AI agent management system that runs entirely in your browser.

## Run it

Open `agent-hub.html` in Chrome or Edge (double-click, or drag into a tab). No server, no install. All data (agents, skills, memories, tasks, API keys) is stored in the browser's IndexedDB for that file's origin.

## What it does

- **Multiple LLMs via APIs** — any OpenAI-compatible chat-completions endpoint. Presets: DeepSeek (direct), OpenRouter (DeepSeek models, browser-friendly), and an offline Mock provider so the whole app works without a key.
- **Assessment before every API call** — each task goes through: local secret/PII scan with one-click redaction → triage & routing (cheap model or local heuristic picks agent, skills, memories) → cost estimate in EUR with per-task and daily budget gates → your explicit approval. Nothing reaches the main model unapproved.
- **Agents** — named configurations (system prompt, model, temperature, default skills, memory access level).
- **Skills** — reusable instruction blocks injected into the system prompt, attached by agents, triage, or manually.
- **Persistent memory** — typed entries (user / project / fact / feedback). Agents with write access save memories via the `save_memory` tool (or `<memory>` tags for models without tool support) and can search with `search_memory`.
- **Reasoning models** — DeepSeek R1's chain of thought streams into a collapsible block; set a per-model max-output override (~16.000) so reasoning is not truncated.
- **Image generation** — models flagged `image` (e.g. `google/gemini-2.5-flash-image` via OpenRouter) return images inline with download / remove-from-task controls. Model line example: `google/gemini-2.5-flash-image | Gemini 2.5 Flash Image | 0.3 | 2.5 | notools,image`. Note: images are stored as base64 in IndexedDB (~1–2 MB each) — remove them from tasks after downloading to keep storage and backups lean.
- **Cost tracking** — real token usage per task (exact provider-reported cost when available), daily and all-time totals (European formatting, e.g. `1.234,56 EUR`), live OpenRouter credit balance in Settings.
- **Backup** — one-click JSON export/import of everything. Backups never contain API keys; keys already in the browser are kept on import.

## Important: DeepSeek and CORS

DeepSeek's API blocks direct browser calls (no CORS headers). To use DeepSeek models:

1. **Recommended:** put an [OpenRouter](https://openrouter.ai) key in the OpenRouter provider — it serves DeepSeek V3/R1 at comparable prices and allows browser requests, or
2. run your own proxy and add it as a custom provider.

The DeepSeek-direct preset is included in case they enable CORS later; if blocked, you get a guided error.

## First steps

1. Open the file — it seeds example agents, skills, and the Mock provider (try a task containing the word "remember" to see memory writing).
2. Settings → OpenRouter → add your API key.
3. Edit the agents to point at a real model, and set the triage model to something cheap (e.g. DeepSeek V3).
4. Set your per-task and daily budgets.

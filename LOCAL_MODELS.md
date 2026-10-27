# Optional local model

Text-only work — summarizing the digest and drafting event descriptions — can go to a local model server the user already runs (Ollama or any OpenAI-compatible endpoint).

Data lookups, decisions, and calendar writes stay in the bot. If `LOCAL_LLM_URL` or `LOCAL_LLM_MODEL` is unset, or a call fails, the bot does that work itself.

## Setup
Store the endpoint and model as bot secrets or environment variables. Never commit them to this repo.

- `LOCAL_LLM_URL` — server base URL
- `LOCAL_LLM_MODEL` — model name

The local model never receives credentials (connector tokens, secrets, or account logins). Send only the text to summarize or draft.

## Ollama (`/api/chat`)

```bash
curl -s http://localhost:11434/api/chat \
  -H "Content-Type: application/json" \
  -d '{"model":"MODEL","messages":[{"role":"user","content":"Summarize this dividend digest."}],"stream":false}'
```

## OpenAI-compatible (`/v1/chat/completions`)

```bash
curl -s http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"MODEL","messages":[{"role":"user","content":"Summarize this dividend digest."}]}'
```

Replace the host and `MODEL` with `LOCAL_LLM_URL` and `LOCAL_LLM_MODEL`. Do not put an API key in the repo or in the prompt. If the server requires one, keep it in bot secrets and send it only as a request header.

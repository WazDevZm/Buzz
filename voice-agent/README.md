<p align="center"><img src="branding/buzz-logo-black.png" alt="Buzz" width="220"></p>

# Buzz: BongoHive AI voice agents

A self-hosted AI voice-agent platform that answers and places phone calls. It uses local open-source models only: call audio, transcripts and prompts stay on BongoHive infrastructure.

**Status:** Phase 0 (discovery). See [docs/PLAN.md](docs/PLAN.md).

## Architecture

```
Caller ─ SIP trunk ─ Asterisk 22 ──ARI + chan_websocket──▶ Dograh (orchestration, UI, API)
                                                             │
                     Web widget / Test Audio ──WebRTC──────▶ │
                                                             ├─▶ vLLM     (Qwen2.5-7B-Instruct-AWQ)
                                                             ├─▶ Speaches (faster-whisper large-v3-turbo, Kokoro)
                                                             └─▶ webhook ─▶ n8n ─▶ HubSpot / Google Sheets
```

## Layout

| Path | Contents |
|---|---|
| `infra/inference` | vLLM + Speaches compose |
| `infra/app` | Dograh compose, nginx/TLS |
| `infra/asterisk` | Asterisk config templates |
| `infra/monitoring` | Prometheus, Grafana, alerts |
| `infra/backup` | Postgres and MinIO backup/restore |
| `agents/` | Workflow exports, prompts, test scenarios |
| `integrations/` | n8n flows, webhook schemas, suppression-list service |
| `knowledge_base/` | Cleaned source documents |
| `tests/` | WER, latency, load and scenario tests |
| `docs/` | Plan, runbooks, decisions, changelog |
| `branding/` | Buzz logos. Use the black logo on light backgrounds; this is the default in the UI. |

Secrets live only in git-ignored `.env` files; each has a committed `.env.example`.

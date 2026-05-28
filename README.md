# Mike (Hermes Agent) Backup

> Backup of core configuration and documentation for the Hermes Agent instance running on the VPS.

## What's Backed Up

| File | Purpose |
|------|---------|
| `config.yaml` | Main Hermes configuration (model, providers, tool backends, cron settings) |
| `mike-runbook.md` | Troubleshooting guide for when the agent goes down |
| `.env.example` | Template showing which environment variables are required |

## What's NOT Backed Up (And Why)

- **`.env`** — Contains API keys and secrets. Kept locally on the VPS only.
- **Session transcripts** — Too large, not meaningful to version.
- **Cache files** — Ephemeral, regenerates automatically.

## Recovery Steps

If the VPS needs to be rebuilt:

1. Clone this repo
2. Copy `config.yaml` to `~/.hermes/` (or `/opt/data/`)
3. Fill in `.env` with actual secrets (from password manager)
4. Install Hermes Agent
5. Restart gateway

## Current Setup

- **Model:** moonshotai/kimi-k2.6 (OpenRouter)
- **Web Search:** Tavily
- **STT:** Local faster-whisper
- **TTS:** Edge TTS
- **Cron Job:** Daily 6 AM ET briefing (ID: `718bda6a5ea8`)

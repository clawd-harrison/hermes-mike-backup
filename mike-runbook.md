# Mike Runbook — Hermes Agent Troubleshooting Guide

> For: Soham G  
> Agent: Mike (Hermes Agent)  
> Platform: Telegram DM  
> Home Channel: `telegram:1592824531`

---

## Deployment Info

| Detail | Value |
|--------|-------|
| Host | Linux VPS (6.8.0-117-generic) |
| Working Directory | `/opt/hermes` |
| User Home | `/opt/data` |
| Hermes Home | `/opt/data` (`HERMES_HOME=/opt/data`) |
| Python | `/opt/hermes/.venv/bin/python3` (Python 3.13.5) |
| Package Manager | `uv` at `/usr/local/bin/uv` |

---

## Model & Provider

| Detail | Value |
|--------|-------|
| Current Model | `moonshotai/kimi-k2.6` |
| Provider | `openrouter` |
| API Key | `/opt/data/.env` → `OPENROUTER_API_KEY` |

---

## Key Paths

| What | Path |
|------|------|
| Main Config | `/opt/data/config.yaml` |
| Secrets / API Keys | `/opt/data/.env` |
| Session Store | `/opt/data/state.db` |
| Gateway Logs | `/opt/data/logs/gateway.log` |
| Session Transcripts | `/opt/data/sessions/` |
| Skills | `/opt/data/skills/` |
| Cron Jobs | `/opt/data/cron/` |
| Audio Cache | `/opt/data/audio_cache/` |
| Image Cache | `/opt/data/image_cache/` |

---

## Health Checks

```bash
# Check if gateway process is running
ps aux | grep "hermes gateway" | grep -v grep

# Check recent logs
tail -50 /opt/data/logs/gateway.log

# Full diagnostics
/opt/hermes/.venv/bin/hermes doctor
```

---

## Restart Commands

**Foreground (for testing / debugging):**
```bash
cd /opt/hermes
/opt/hermes/.venv/bin/hermes gateway run
```

**Background service (recommended for persistence):**
```bash
/opt/hermes/.venv/bin/hermes gateway install
/opt/hermes/.venv/bin/hermes gateway start
/opt/hermes/.venv/bin/hermes gateway status
```

**Quick restart:**
```bash
pkill -f "hermes gateway"
sleep 2
cd /opt/hermes && /opt/hermes/.venv/bin/hermes gateway run
```

---

## Cron Jobs

| Job | ID | Schedule |
|-----|-----|----------|
| daily-ai-news-and-joke | `718bda6a5ea8` | `0 10 * * *` (6:00 AM ET) |

**Check status:**
```bash
/opt/hermes/.venv/bin/hermes cron list
```

**Manual trigger (if a run was missed):**
```bash
/opt/hermes/.venv/bin/hermes cron run 718bda6a5ea8
```

---

## Voice / STT / TTS

| Feature | Status | Notes |
|---------|--------|-------|
| TTS | Working | Edge TTS (free, no key) |
| STT | Working | `faster-whisper` installed in venv |

**Fix STT if broken:**
```bash
cd /opt/hermes && uv pip install faster-whisper
```

**Fix TTS if broken:**
```bash
cd /opt/hermes && uv pip install edge-tts
```

---

## Common Issues

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| No Telegram reply | Gateway crashed | Restart gateway |
| Cron missed run | Gateway was down | Manual trigger + ensure service stays up |
| Tool errors | Python env issue | Use `/opt/hermes/.venv/bin/python3` |
| Model errors | API key issue | Check `/opt/data/.env` |

---

## Useful Commands

```bash
# View config
cat /opt/data/config.yaml

# Edit config
/opt/hermes/.venv/bin/hermes config edit

# Recent sessions
/opt/hermes/.venv/bin/hermes sessions list

# Gateway status
/opt/hermes/.venv/bin/hermes gateway status

# Diagnostics
/opt/hermes/.venv/bin/hermes doctor
```

---

## Recovery Steps

1. SSH into the VPS
2. Check process: `ps aux | grep hermes`
3. Check logs: `tail -100 /opt/data/logs/gateway.log`
4. Try restarting the gateway
5. Run `hermes doctor` for diagnostics
6. Re-run setup if needed: `hermes setup`

---

## In-Chat Commands

You can also send these directly to me in Telegram:
- `/restart` — Restart the gateway
- `/status` — Show session/platform status
- `/commands` — Browse all available commands
- `/help` — Show help

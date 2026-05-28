# Claude Code Authentication

## Status
Authenticated with Pro subscription via CLAUDE_CODE_OAUTH_TOKEN.

## Token Storage
- Location: /opt/data/.config/claude-code-env
- Permissions: 600 (restricted)
- Do NOT commit this file to GitHub.

## How to Use
Source the env file before running Claude Code:
```bash
source /opt/data/.config/claude-code-env
claude -p "your task here"
```

## Rebuild / New Container
If the container is ever rebuilt or recreated, the env var must be set again:
- **Option 1:** Mount `/opt/data/.config/claude-code-env` into the new container and source it on startup.
- **Option 2:** Pass `-e CLAUDE_CODE_OAUTH_TOKEN=<token>` in the `docker run` command.
- **Option 3:** Add the env var to the container's startup script or Docker Compose `environment:` section.

The token is valid for ~1 year. If auth suddenly stops working after a rebuild, check that the env var is present: `echo $CLAUDE_CODE_OAUTH_TOKEN`.

## Important: June 2026 Metering Change
Starting June 15, 2026, `claude -p` (print mode) calls draw from a **separate monthly credit bucket** on Pro plans, not the main chat usage pool. If Mike reports throttling or "rate limit" errors on coding tasks after that date, the cause is likely the separate bucket running dry — not a broken token.


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

## Subscription
- Uses Anthropic Pro subscription (included with $20/month plan)
- No API key needed, no additional cost


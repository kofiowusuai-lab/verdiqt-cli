# AGENTS.md

## Purpose

This repo packages the Verdiqt CLI for agent workflows. Agents use it to scan ad creative before launch and return policy verdicts with links to the Verdiqt workbench.

## Start

```bash
npm link
verdiqt --help
```

Or:

```bash
node ./bin/verdiqt --help
```

## Credentials

Users must provide their own token:

```bash
verdiqt login --token vrdq_REPLACE_WITH_USER_TOKEN
```

Do not use or commit credentials from `~/.config/verdiqt/config.json`.

## Agent Rules

- Always run `verdiqt --help` once after install to confirm the CLI is callable.
- Use `--json` for scans so the agent can parse the result.
- Use `--open` when the user should inspect the full workbench report.
- Never commit API tokens, `.verdiqt.json`, verdict JSON, creative uploads, media, CSV exports, or local config.
- If scanning video, ensure `ffmpeg` and `ffprobe` are installed.
- If the CLI reports missing login, ask the user for their own `vrdq_...` token or instruct them to mint one in Verdiqt settings.

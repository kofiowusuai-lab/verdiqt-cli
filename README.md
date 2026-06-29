# verdiqt-cli

Command-line client for the Verdiqt policy engine. It pre-flights ad creative against platform, industry, jurisdiction, and legal-layer rules before media buyers or agents push campaigns live.

The CLI can scan copy, image files, video files, landing page URLs, and CSV batches. It talks to a Verdiqt backend using a user-owned `vrdq_...` API token.

## Quick Start

```bash
git clone https://github.com/kofiowusuai-lab/verdiqt-cli.git
cd verdiqt-cli
npm link
verdiqt --help
```

Or run without linking:

```bash
node ./bin/verdiqt --help
```

## Login

Mint an API token in the Verdiqt web app settings, then save it locally:

```bash
verdiqt login --token vrdq_REPLACE_WITH_USER_TOKEN
```

Optional custom backend:

```bash
verdiqt login \
  --token vrdq_REPLACE_WITH_USER_TOKEN \
  --base-url https://verdiqt-app.vercel.app
```

The CLI stores credentials locally in:

```text
~/.config/verdiqt/config.json
```

Do not commit that file.

You can also avoid local config and use environment variables:

```bash
export VERDIQT_TOKEN=vrdq_REPLACE_WITH_USER_TOKEN
export VERDIQT_BASE_URL=https://verdiqt-app.vercel.app
```

## Commands

```bash
verdiqt --help
verdiqt login --help
verdiqt logout
verdiqt me
verdiqt scan --help
verdiqt scans --help
verdiqt batch --help
```

## One-Shot Scans

Scan ad copy:

```bash
verdiqt scan \
  --platform meta \
  --industry finance \
  --copy "Invest smarter with clear risk controls." \
  --json \
  --open
```

Scan copy from a file:

```bash
verdiqt scan \
  --platform google \
  --industry ecommerce \
  --copy-file ./ad-copy.txt \
  --json
```

Scan an image:

```bash
verdiqt scan \
  --platform meta \
  --industry health_supplements \
  --image ./creative.png \
  --json \
  --open
```

Scan a video:

```bash
verdiqt scan \
  --platform tiktok \
  --industry crypto \
  --sub-mode paid \
  --video ./promo.mp4 \
  --json \
  --open
```

Scan a landing page URL:

```bash
verdiqt scan \
  --platform meta \
  --industry finance \
  --url https://example.com/landing-page \
  --json
```

## Supported Scan Options

Platforms:

- `meta`
- `google`
- `x_twitter`
- `linkedin`
- `tiktok`
- `youtube`

Industries:

- `health_supplements`
- `finance`
- `crypto`
- `gambling`
- `alcohol`
- `dating`
- `ecommerce`

Jurisdictions:

- `US`
- `UK`
- `EU`
- `IN`
- `AU`

Legal layers:

- `ftc_substantiation`
- `finra_finance`
- `asa_uk`
- `asci_india`
- `eu_dsa`

Video/image scans require local media paths. Video support uses `ffmpeg` and `ffprobe`.

## Batch Scans

Upload a CSV:

```bash
verdiqt batch upload ./jobs.csv
```

Wait for completion:

```bash
verdiqt batch wait BATCH_ID
```

Check status:

```bash
verdiqt batch status BATCH_ID
```

Export results:

```bash
verdiqt batch export BATCH_ID --out results.csv
```

## Scan History

```bash
verdiqt scans list
verdiqt scans get SCAN_ID
verdiqt scans feedback SCAN_ID --rating up
verdiqt scans feedback SCAN_ID --rating down --note "Missed policy risk"
```

## Agent Quick Start

When another user gives this repo link to an agent, the expected flow is:

```bash
git clone https://github.com/kofiowusuai-lab/verdiqt-cli.git
cd verdiqt-cli
npm link
verdiqt --help
verdiqt login --token vrdq_REPLACE_WITH_USER_TOKEN
verdiqt scan --platform meta --industry ecommerce --copy "Sample ad copy" --json --open
```

Agent operating rules are in `AGENTS.md`. The key rule is that users must bring their own Verdiqt API token; agents must not reuse tokens from another machine.

## Safety

- Do not commit `~/.config/verdiqt/config.json`, `.verdiqt.json`, verdict JSON, creative uploads, CSV exports, `.env*`, or API tokens.
- Do not publish real customer creative unless the user explicitly asks for a sanitized fixture.
- Use `--json` for agent workflows so the result can be parsed deterministically.
- Use `--open` when the user should inspect the full web workbench result.

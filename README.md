<h1 align="center">gws</h1>

**One CLI for all of Google Workspace -- built for humans and AI agents.**

Drive, Gmail, Calendar, and every Workspace API. Zero boilerplate. Structured JSON output. 107 agent skills included.

> [!NOTE]
> This is **not** an officially supported Google product.

<p>
  <a href="https://www.npmjs.com/package/@googleworkspace/cli"><img src="https://img.shields.io/npm/v/@googleworkspace/cli" alt="npm version"></a>
  <a href="https://github.com/googleworkspace/cli/blob/main/LICENSE"><img src="https://img.shields.io/github/license/googleworkspace/cli" alt="license"></a>
  <a href="https://github.com/googleworkspace/cli/actions/workflows/ci.yml"><img src="https://img.shields.io/github/actions/workflow/status/googleworkspace/cli/ci.yml?branch=main&label=CI" alt="CI status"></a>
  <a href="https://www.npmjs.com/package/@googleworkspace/cli"><img src="https://img.shields.io/npm/unpacked-size/@googleworkspace/cli" alt="install size"></a>
  <a href="https://img.shields.io/badge/status-active-green"><img src="https://img.shields.io/badge/status-active-green" alt="status"></a>
</p>

## What It Does

A dynamic CLI that reads Google's [Discovery Service](https://developers.google.com/discovery) at runtime and builds its entire command surface automatically. When Google adds an API endpoint, `gws` picks it up without a code change. Ships with 107 slash command skills for AI-assisted development workflows.

## Features

- **Dynamic command surface** -- every Google Workspace API, generated from Discovery at runtime
- **107 agent skills** -- SKILL.md files for Gmail, Drive, Docs, Calendar, Sheets, and more
- **Structured JSON output** -- every response is machine-parseable for AI agent pipelines
- **Helper commands** -- `+send`, `+reply`, `+agenda`, `+upload`, `+triage` and 30+ more
- **Multiple auth flows** -- OAuth desktop, service account, headless/CI, pre-obtained tokens
- **Auto-pagination** -- stream paginated results as NDJSON
- **Model Armor integration** -- sanitize responses for prompt injection protection
- **Gemini CLI extension** -- install as a Gemini agent extension

## Tech Stack

- **Rust** (core CLI binary)
- **Node.js 18+** (npm distribution, skill installation)
- **Google Discovery API** (dynamic command generation)
- **OAuth 2.0** (AES-256-GCM encrypted credential storage)

## Quick Start

```bash
npm install -g @googleworkspace/cli

gws auth setup     # walks through Google Cloud project config
gws auth login     # OAuth login
gws drive files list --params '{"pageSize": 5}'
```

### Install Agent Skills

```bash
# All skills at once
npx skills add https://github.com/googleworkspace/cli

# Or pick specific ones
npx skills add https://github.com/googleworkspace/cli/tree/main/skills/gws-drive
npx skills add https://github.com/googleworkspace/cli/tree/main/skills/gws-gmail
```

## Architecture

Two-phase parsing strategy:

1. Read `argv[1]` to identify the service (e.g. `drive`)
2. Fetch the service's Discovery Document (cached 24h)
3. Build a `clap::Command` tree from the document's resources and methods
4. Re-parse remaining arguments, authenticate, build HTTP request, execute

All output -- success, errors, download metadata -- is structured JSON.

## License

Apache-2.0

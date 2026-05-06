[🔌 Register / Sign In](https://app.anycrawler.com/login) | [🛋 Dashboard](https://app.anycrawler.com/overview) | [🌓 Get API Keys](https://app.anycrawler.com/api-keys) | [🕪 Public API](https://api.anycrawler.com)

# AnyCrawler Read Skill for AI Agents ✅

This repository provides an AnyCrawler read skill that can be installed into compatible AI agent runtimes. The primary skill name is `$anycrawler-read`. The bundled runtime remains crawl-first, leaving search workflows to the separate `$anycrawler-search` skill.

## What this repo contains

- `skill/anycrawler-read/SKILL.md`: the slim agent-facing runtime guide
- `skill/anycrawler-read/references/public-api.md`: the minimal API contract for agent use
- `skill/anycrawler-read/references/maintainer.md`: release, billing, and full gateway notes
- `skill/anycrawler-read/scripts/anycrawler_crawl_api.py`: the bundled CLI
- `skill/anycrawler-read/agents/openai.yaml`: agent display metadata
- `tests/test_anycrawler_crawl_api.py`: regression tests for the CLI

## Install

A common installation path is `~/.codex/skills`. The source directory in this repository is `skill/anycrawler-read`, and the installed skill directory should also be named `anycrawler-read` so it matches the explicit invocation name `$anycrawler-read`.

### macOS / Linux

```bash
git clone https://github.com/AnyCrawler-com/AnyCrawler-Skill.git
mkdir -p ~/.codex/skills
cp -R AnyCrawler-Skill/skill/anycrawler-read ~/.codex/skills/anycrawler-read
```

### Windows PowerShell

```powershell
git clone https://github.com/AnyCrawler-com/AnyCrawler-Skill.git
New-Item -ItemType Directory -Force "$HOME\.codex\skills" | Out-Null
Copy-Item -Recurse -Force ".\AnyCrawler-Skill\skill\anycrawler-read" "$HOME\.codex\skills\anycrawler-read"
```

After installation, start a new AI agent session so the new skill can be discovered again.

On the first managed-install invocation in each agent session, the bundled CLI checks the latest tagged release from `AnyCrawler-com/AnyCrawler-Skill`. If the installed skill is behind, it upgrades itself before running the crawl command. Direct repo checkouts skip self-mutation.

## Quick setup

Before using AnyCrawler, complete the following setup:

1. [Register or sign in to AnyCrawler](https://app.anycrawler.com/login)
2. [Create or copy an API key from the dashboard](https://app.anycrawler.com/api-keys)
3. Set `ANYCRAWLER_API_KEY` in your environment variables

### Bash

```bash
export ANYCRAWLER_API_KEY="sk-your-key"
```

### PowerShell

```powershell
$env:ANYCRAWLER_API_KEY = "sk-your-key"
```

Optional environment variables:

- `ANYCRAWLER_BASE_URL`: defaults to `https://api.anycrawler.com`

## Use with an AI agent

You can explicitly mention the skill in your prompt:

```text
Use $anycrawler-read to crawl https://example.com with render and save markdown.
```

```text
Use $anycrawler-read to take a screenshot of https://example.com and download the PNG.
```

From the repository root, you can also run the bundled CLI directly:

```bash
python skill/anycrawler-read/scripts/anycrawler_crawl_api.py page \
  --url https://example.com \
  --include-metadata

python skill/anycrawler-read/scripts/anycrawler_crawl_api.py screenshot \
  --url https://example.com \
  --download-snapshot snapshot.png
```

For the stable public crawl API contract, read `skill/anycrawler-read/references/public-api.md`.

## Documentation map

- Agent runtime guide: `skill/anycrawler-read/SKILL.md`
- Minimal API contract: `skill/anycrawler-read/references/public-api.md`
- Maintainer-only notes: `skill/anycrawler-read/references/maintainer.md`
- CLI implementation: `skill/anycrawler-read/scripts/anycrawler_crawl_api.py`

## Stable public endpoints

- `POST /v1/crawl/page`
- `POST /v1/crawl/screenshot`
## Notes

- This repository targets the stable public contract only and does not depend on undocumented worker passthrough fields.
- The bundled CLI remains crawl-first. Search workflows should live in the separate `$anycrawler-search` skill.
- Keep SSR follow-up parsing generic here. Site-specific extraction logic should live in a separate extraction-focused skill or workflow.

## Releases

Versioning, compatibility, billing notes, and the release checklist live in `skill/anycrawler-read/references/maintainer.md`.

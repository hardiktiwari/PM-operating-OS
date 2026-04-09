# Changelog

All notable changes to PM Operating System are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/).

## [2.0.0] — 2026-04-09

Self-updating knowledge base. [Blog post →](https://medium.com/@hardiktiwari)

### Added

- **Continual learning** — `AGENTS.md` auto-updates with user corrections and workspace facts via the Continual Learning plugin. Memory compounds across sessions.
- **Product dossiers** — `knowledge/products/` with structured templates: `brief.md`, `roadmap.md`, `metrics.md`, `updates.md`, `experiments/`, `releases/`.
- **Workspace drafts** — `workspace/drafts/` as agent scratch space, separated from curated product knowledge.
- **Context-graph skills** — `decision-logger`, `what-if`, `knowledge-updater` for capturing reasoning and simulating decisions.
- **Onboarding agent** — interactive setup that writes rules, creates product folders, and configures MCPs in one conversation.
- **MCP template** — pre-configured `.cursor/mcp.json` for Figma, Google Drive, and Jira.

### Changed

- **Simplified repo structure** — removed setup scripts, Jinja templates, and headless scheduler in favor of agent-driven onboarding. Skills and knowledge layer are the core primitives.
- **AGENTS.md** — streamlined to generic PM Chief of Staff template with auto-populated learned sections.
- **README** — rewritten for faster onboarding (clone → open → "onboard").
- **Knowledge structure** — `knowledge/projects/` renamed to `knowledge/products/` with richer templates (added `roadmap.md`, `releases/`).

### Removed

- `scripts/setup.py` and `scripts/setup.sh` — replaced by the onboarding agent.
- `templates/` — Jinja template layer removed; agents write files directly.
- `scheduler/` — headless scheduler removed (can be re-added as an extension).
- `knowledge/examples/` — Spotify, Netflix, Shopify, Uber examples removed to reduce repo size.
- `memory/` — context-graph trajectory store removed from default repo (skills remain for opt-in use).
- `.cursor/commands/` and `.cursor/agent-schedules.json` — slash commands and IDE cron removed from default (described in blog for advanced users).
- `ONBOARDING.md`, `MCP_SETUP.md`, `docs/` — consolidated into README and onboarding agent.

## [1.0.0] — 2025-12-01

Initial release. [Blog post →](https://medium.com/@hardiktiwari)

### Added

- Static knowledge layer — strategy, customer segments, metrics, competitive landscape templates.
- 12 core PM skills — PRD writer, working backwards, experiment designer, launch post, exec communicator, and more.
- Rules and agents templates with Jinja2 rendering via `scripts/setup.py`.
- Interactive onboarding flow.
- Example knowledge bases using public investor data from Spotify, Netflix, Shopify, and Uber.
- Headless scheduler (`scheduler/`) for running agent commands via Claude API.
- Slash commands (`/planday`, `/slack-sync`, `/program-update`) with IDE cron scheduling.

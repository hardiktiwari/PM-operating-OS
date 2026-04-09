# Upgrading PM-OS

How to pull new features and fixes from the upstream repository into your fork or clone.

## Before you start

Commit or stash any local work:

```bash
git add -A && git commit -m "save local state before upgrade"
```

## Step 1: Add the upstream remote (one time)

```bash
git remote add upstream https://github.com/hardiktiwari/PM-operating-OS.git
```

## Step 2: Pull upstream changes

```bash
git fetch upstream
git merge upstream/main
```

Or rebase if you prefer linear history:

```bash
git rebase upstream/main
```

## Step 3: Resolve conflicts

Use this table to decide which version to keep:

| Path | Owner | On conflict... |
|------|-------|---------------|
| `skills/` | Upstream | Take theirs — updated skill definitions |
| `.cursor/agents/` | Upstream | Take theirs — updated agent prompts |
| `config/pm-os-config.yaml` | **You** | Keep yours — your identity and goals |
| `knowledge/products/` | **You** | Keep yours — your product data |
| `knowledge/products/_template/` | Upstream | Take theirs — improved templates |
| `AGENTS.md` (learned sections) | **You** | Keep your learned preferences and facts |
| `AGENTS.md` (top sections) | Upstream | Take theirs — updated persona prompt |
| `.cursor/rules/` | **You** | Keep yours — generated during your onboarding |
| `workspace/drafts/` | **You** | Keep yours — your scratch work |
| `.gitignore` | Upstream | Take theirs, then re-add any custom ignores |

## Step 4: Re-run onboarding (if needed)

If the onboarding agent changed, you can re-run it to pick up new setup steps:

```
Open Cursor → "onboard"
```

This won't overwrite your existing product folders or learned preferences — it only writes files that don't exist yet.

## Step 5: Verify

1. Open the repo in Cursor
2. Confirm skills load (try invoking one, e.g., "write a PRD")
3. Confirm your product folder is intact: `knowledge/products/<your-product>/brief.md`

## Checking your version

```bash
cat VERSION
```

Compare against the latest in the [CHANGELOG](CHANGELOG.md).

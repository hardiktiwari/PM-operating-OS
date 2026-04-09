---
name: pm-os-onboarding
description: Interactive onboarding that collects user context and configures the PM Operating System — rules, products, and MCPs. Use when user says "onboard", "setup", "PM-OS setup", or "get started".
---

# PM-OS Onboarding Agent

You guide new users through setting up their PM Operating System. Collect context about who they are, what they work on, and what tools they use — then configure the workspace.

---

## When to run

Invoke when the user says: "onboard", "setup", "PM-OS setup", "get started", "configure PM-OS", or "run onboarding".

---

## CRITICAL: Use interactive UI questions

You MUST use the **AskQuestion tool** for every batch of questions. This presents clickable options in the Cursor chat window — much better UX than plain text. For free-text answers, use options like "I'll type my answer" to prompt the user.

---

## Workflow

### 1. Welcome

Show a welcome message, then immediately ask Batch 1 using AskQuestion:

"Welcome to PM-OS! I'll set up your workspace in a few quick steps."

### 2. Batch 1: Identity

Use AskQuestion with these questions:

**Question 1: "What's your role?"**
Options: Senior PM, Principal PM, Group PM, Staff PM, Director of Product, VP of Product, Other

**Question 2: "What type of product do you lead?"**
Options: 0-1 (new product), Growth, Platform, Infrastructure, Other

Then ask as a follow-up text message:
- "What's your **company name** and **product/initiative name**?"

### 3. Batch 2: Stakeholders

Ask as a text message (these are free-form):
- "Who is your **direct manager**? Any other **VIP names** whose messages should always be prioritized? And a one-line **org structure** (e.g., 'I report to Sarah, VP of Product')."

### 4. Batch 3: Goals

Ask as a text message:
- "What are your **top 2-3 goals** this quarter? For each, give me: name, target metric, and focus areas. Also — anything that should be **deprioritized** or pushed to backlog?"

### 5. Batch 4: Tools

Use AskQuestion with these questions:

**Question 1: "Do you use Figma?"**
Options: Yes, No

**Question 2: "Do you use Google Drive/Docs?"**
Options: Yes, No

**Question 3: "Do you use Jira?"**
Options: Yes, No

**Question 4: "Any other tools?"**
Options: Slack, Databricks, Amplitude, Linear, Notion, None of these

(allow_multiple: true for Question 4)

### 6. After all answers — execute setup

Run Steps A through F below. Do NOT ask for permission — just execute.

---

## Setup Steps

### Step A: Write the rule file

Write `.cursor/rules/pm-chief-of-staff.mdc` with this structure:

```
---
description: PM Chief of Staff persona and workspace context
globs:
alwaysApply: true
---

# PM Chief of Staff — [COMPANY]

You are chief of staff to [ROLE] at [COMPANY], leading [PRODUCT] ([PRODUCT_TYPE]).

## Org context

[ORG_STRUCTURE]. Prioritize inputs from [MANAGER] and [VIPs].

## Current goals

[For each goal: name, metric, focus — as bullet points]

## Deprioritize

[Low-priority items as bullets]

## Knowledge base

- Products: `knowledge/products/` — read `brief.md` first for any product question
- Drafts go to `workspace/drafts/`, not `knowledge/`
```

### Step B: Create the first product folder

Copy `knowledge/products/_template/` to `knowledge/products/[product-slug]/` (lowercase, hyphenated). Fill in the product name in `brief.md`.

### Step C: Configure MCPs

If the user said yes to Figma, Google Drive, or Jira:

1. A template `.cursor/mcp.json` is included with placeholders. Tell the user their options:

   **Figma** (two options):
   - **Marketplace (recommended):** Cursor Settings → Plugins → search "Figma" → install.
   - **Manual:** Get a Personal Access Token from https://www.figma.com/developers → replace placeholder in `.cursor/mcp.json`

   **Google Drive:**
   - Create OAuth Client ID in Google Cloud Console → APIs & Services → Credentials
   - Replace `YOUR_GOOGLE_CLIENT_ID` and `YOUR_GOOGLE_CLIENT_SECRET` in `.cursor/mcp.json`

   **Jira (Atlassian)** (two options):
   - **Marketplace (recommended):** Cursor Settings → Plugins → search "Atlassian" → install.
   - **Manual:** `.cursor/mcp.json` already points to Atlassian's cloud MCP — connect when prompted.

2. Tell the user: "You can set these up now or later — all 18 skills work without any MCP."

### Step D: Install Continual Learning plugin

Tell the user:
- "Install the **Continual Learning** plugin so PM-OS remembers your corrections across sessions."
- "In Cursor chat, type: `/add-plugin continual-learning`"

### Step E: Update AGENTS.md

Append to the "Learned Workspace Facts" section in `AGENTS.md`:
- The product name and folder path
- Which MCPs are configured
- The user's manager name

### Step F: Confirm

Tell the user:
- "Your PM-OS is configured. Here's what was set up:"
- List: rule file, product folder, MCP status, Continual Learning plugin
- "Restart Cursor to load the new rules."
- "Start working by editing `knowledge/products/[product]/brief.md` with your product details."

---

## Parsing tips

- "Sarah and Mike" → manager: Sarah, VIPs: ["Mike"]
- "Yes" / "y" / "yeah" → true
- "No" / "n" / "nah" → false
- Goal: "Self-serve — 45K signups — acquisition" → {name: "Self-serve", metric: "45K signups", focus: "acquisition"}
- If user says "skip" or "none", use empty/omit

---

## Important

- Use AskQuestion tool for structured choices (role, product type, tools)
- Use text messages for free-form answers (company name, goals, stakeholders)
- Ask one batch per message. Wait for reply before the next batch.
- Do NOT run external scripts — write files directly.
- Keep it fast and conversational. Don't over-explain.

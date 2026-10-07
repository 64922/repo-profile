---
name: repo-profile
description: Writes or improves the GitHub About section (description + topics) for the current project. Use when the user wants to write a repo description or set or suggest GitHub topics — 触发词：仓库描述、仓库简介、仓库标签、设置 topics、完善 About 区。
---

# Repo Profile

Polish the current project's GitHub **About section** — one English description plus 8–15 topics — from the repo's own content: draft, confirm with the user, write with `gh`, verify.

Only the description and topics are in scope; leave every other repository setting untouched. Work the steps in order and finish each one before moving on; if a completion criterion cannot be met, stop and report why.

## Step 1 — Preconditions

1. Resolve the repo: `git rev-parse --show-toplevel` for the root and `git remote get-url origin` for the URL. If `origin` is not on github.com, look for another `github.com` remote. Parse `owner/repo`; with no GitHub remote, stop and report that this skill needs one.
2. Ensure `gh` is installed: `gh --version`. Missing on Windows → run `winget install --id GitHub.cli --accept-source-agreements --accept-package-agreements`. Missing elsewhere → give the user `brew install gh` or the instructions at https://github.com/cli/cli#installation, and wait for them to install it. If `gh` is still not found after installing, add its install directory to PATH for this session.
3. Ensure `gh` is authenticated: `gh auth status`. Not logged in → run `gh auth login --web` and let the user complete the browser flow.

**Done when** `git rev-parse` resolves the root and `gh auth status` exits 0.

## Step 2 — Gather context

Read, in order of value:

1. The main README — the primary account of what the project does.
2. Manifests — `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, and similar (`name` / `description` / `keywords` fields).
3. The top-level directory listing.
4. The current About section: `gh repo view <owner>/<repo> --json description,repositoryTopics`.

**Done when** all four sources are read. If the README is missing or yields no usable facts, ask the user one targeted question about what the project does before drafting.

## Step 3 — Draft

Compose one recommended draft, entirely in English:

- **Description** — follow the Description rules below: what it does and who it is for, 100–200 characters. Append 1–2 lines on why this wording fits.
- **Topics** — 8–15 topics following the Topic rules below.

When the current About section has content, improve it in place and tag every topic **keep**, **add**, or **remove**.

**Done when** the description is within 350 characters and all 8–15 topics pass the format checks below.

## Step 4 — Confirm

Show the user:

- the description with its rationale,
- the topics as a diff table (keep / add / remove) when the About section is non-empty, otherwise as a grouped list (language, framework, domain, purpose),
- the exact write plan: description, topics, or both.

Wait for explicit confirmation, and regenerate on each round of feedback. If the user confirms only one of the two items, plan to write only that item.

**Done when** the user has confirmed exactly what will be written.

## Step 5 — Write

```bash
gh repo edit <owner>/<repo> --description "<confirmed description>"
gh api -X PUT "repos/<owner>/<repo>/topics" -f "names[]=<topic1>" -f "names[]=<topic2>"
```

The topics call atomically replaces the whole set with the confirmed list. Skip whichever of the two commands the user did not confirm.

**Done when** every confirmed command exits 0.

## Step 6 — Verify

```bash
gh repo view <owner>/<repo> --json description,repositoryTopics
```

Compare the fetched values with the confirmed draft item by item, and fix and re-confirm anything that differs (a truncated description, a dropped topic).

**Done when** the fetched About section matches the confirmed draft exactly, or the user accepts the reported difference.

## Description rules

- Plain functional style: state what the project does and who it is for — a verb or noun phrase, no filler adjectives ("powerful", "seamless").
- English, 100–200 characters; 350 is GitHub's hard limit.
- Weave in the defining technology and domain words naturally; they double as search terms.
- One sentence, no emoji, no trailing period.

## Topic rules

- GitHub format: lowercase letters, digits, and hyphens; 50 characters maximum each; 20 topics maximum in total.
- 8–15 topics mixing four kinds — language (`typescript`), framework (`react`), domain (`cli`), purpose (`developer-tools`).
- Use widely established spellings (`typescript`, not `ts`) so an existing topic page backs the tag.
- Each topic carries one concept; drop near-duplicates.

# repo-profile

An agent skill that drafts and applies your GitHub repo's About section — one description and 8–15 topics — from the repository's own content.

[![License: MIT](https://img.shields.io/github/license/64922/repo-profile)](LICENSE)

## What it does

Point it at a project and it reads the README, manifests, directory layout, and the repo's current About section, then:

- **Drafts a description** — English, plain functional style, 100–200 characters (GitHub's hard limit is 350).
- **Picks 8–15 topics** — mixing language, framework, domain, and purpose, each checked against GitHub's topic format rules.
- **Improves instead of overwriting** — when an About section already exists, the draft is refined in place and every topic is tagged keep, add, or remove.
- **Writes and verifies** — after you confirm, it applies the description and topics with the `gh` CLI and re-fetches them to check the result matches.
- **Stays in its lane** — only the description and topics change; every other repository setting is left untouched.

Requirements: a local clone of a GitHub repository that you have write access to. GitHub only.

## Install

```bash
npx skills add 64922/repo-profile -g
```

Installs globally for agents that support skills, such as OpenCode, Claude Code, Codex, and Cursor. Drop `-g` to install it into the current project instead.

If `gh` (GitHub CLI) is not installed, the skill installs it on Windows via winget and walks you through setup elsewhere.

## Usage

In a project you want to polish, just ask your agent:

```text
Write a repo description and topics for this project.
```

The skill takes it from there:

1. Checks the git repo and `gh` setup, then reads the project.
2. Shows one recommended description and the topics as a keep / add / remove diff.
3. Waits for your edits or confirmation.
4. Writes the confirmed items and re-fetches them from GitHub to verify.

Nothing is written to the remote until you confirm.

## Learn more

The skill's full instructions — six steps with completion criteria, plus the description and topic rules — live in [SKILL.md](SKILL.md).

## Contributing

Contributions are welcome — open an issue or a pull request.

## License

Released under the MIT License. See [LICENSE](LICENSE).

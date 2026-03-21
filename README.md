# lite-git

[简体中文](./README.zh-CN.md)

This repository publishes the `lite-git` agent skill in a layout that can be discovered by `vercel-labs/skills`.

## Install with npx skills

```bash
npx skills add <owner>/lite-git --skill lite-git
```

Or install directly from a GitHub URL:

```bash
npx skills add https://github.com/<owner>/lite-git --skill lite-git
```

## Repository Layout

- `.agents/skills/lite-git/` is the canonical distributable skill folder for cross-agent installers.
- `.github/prompts/` contains workspace-only prompt files for GitHub Copilot / VS Code.

## Included Files

- `.agents/skills/lite-git/SKILL.md`
- `.agents/skills/lite-git/templates/office.gitignore`
- `.agents/skills/lite-git/templates/office-default-lfs.gitattributes`
- `.agents/skills/lite-git/templates/mixed-binary-lfs.gitattributes`

## Local Check

List the skill from the repo root:

```bash
npx skills add . --list
```

Install the local repo version:

```bash
npx skills add . --skill lite-git -y
```

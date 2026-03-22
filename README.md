# lite-git

[简体中文](./README.zh-CN.md)

This repository publishes the `lite-git` agent skill in a layout that can be discovered by `vercel-labs/skills`.

Repository: https://github.com/nickhoo555/lite-git

`lite-git` also handles missing Git LFS during initialization by guiding the user through installation and then continuing setup.

## Install with npx skills

```bash
npx skills add nickhoo555/lite-git --skill lite-git
```

Or install directly from a GitHub URL:

```bash
npx skills add https://github.com/nickhoo555/lite-git --skill lite-git
```

Install globally without prompts:

```bash
npx skills add nickhoo555/lite-git --skill lite-git -g -y
```

Install to GitHub Copilot only:

```bash
npx skills add nickhoo555/lite-git --skill lite-git -a github-copilot -y
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

## Example Human-Agent Conversations (Full Workflow)

The examples below show a complete end-to-end flow: initialize a repository, save a new version, and switch to an older version safely.

### Scenario 1: Initialize an office-document repository

User:
Initialize this folder. It mostly contains Word files, slides, and images.

Assistant (lite-git):
I checked the folder state. This directory is not a Git repository yet.

I found:
- Documents: QuarterlyReport.pptx, BudgetNotes.docx
- Images: cover.png, flowchart.jpg
- Temp file: ~$QuarterlyReport.pptx

Suggested plan:
1. Ignore office temp/lock files and OS clutter
2. Track office and media files with Git LFS
3. Create the initial snapshot commit

I will run:
- git init -b main
- create .gitignore
- create .gitattributes for LFS
- git add + git commit

Done:
- Repository initialized
- Temp file ignored: ~$QuarterlyReport.pptx
- LFS rules added for docx/pptx/png/jpg
- Initial commit created: Initial snapshot

### Scenario 2: Save current version with ignore/LFS checks

User:
Save the current version.

Assistant (lite-git):
I checked your changes.

Detected:
- Modified: QuarterlyReport.pptx
- New: CustomerCase.png
- New: ~$QuarterlyReport.pptx (temp file)

Action:
1. Temp file is excluded (ignored)
2. pptx/png are committed with LFS tracking
3. Safe files are committed now

Done:
- commit: Save current draft of slides
- Included: QuarterlyReport.pptx, CustomerCase.png
- Excluded: ~$QuarterlyReport.pptx

### Scenario 3: Switch to last week's version and keep editing

User:
Switch to last week's version. I need to continue editing from there.

Assistant (lite-git):
I checked the working tree first. No uncommitted changes found, so switch is safe.

Since you want to keep editing, I will avoid detached HEAD and create a branch from the target commit.

Action:
- locate target commit from git log
- git switch -c revise-from-last-week <commit>

Done:
- You are now on branch: revise-from-last-week
- You can continue editing and committing safely
- main stays unchanged

## Quick Prompt Examples

- /version assistant initialize this folder for office files
- /version assistant save current version and check ignore/lfs
- /version assistant switch to last week version and continue editing

or:

- /lite-git initialize this folder for office files
- /lite-git save current version with ignore/lfs checks
- /lite-git switch to last week version and keep editing

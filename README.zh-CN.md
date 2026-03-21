# lite-git

[English](./README.md)

这个仓库发布了一个名为 `lite-git` 的 agent skill，目录结构已经按 `vercel-labs/skills` 的发现规则组织，用户可以直接通过 `npx skills add` 安装。

## 用 npx skills 安装

```bash
npx skills add <owner>/lite-git --skill lite-git
```

也可以直接从 GitHub URL 安装：

```bash
npx skills add https://github.com/<owner>/lite-git --skill lite-git
```

## 这个 skill 做什么

`lite-git` 面向不懂 Git 的用户，主要处理这些场景：

- 初始化一个适合办公文件的 Git 仓库
- 提交当前版本前检查哪些文件应该忽略
- 判断哪些文件更适合交给 Git LFS 管理
- 安全切换到旧版本、其他分支或历史快照

## 仓库结构

- `.agents/skills/lite-git/`：可分发、可安装的标准 skill 目录
- `.github/prompts/`：给 GitHub Copilot / VS Code 使用的工作区 prompt，不属于通用 skills 安装包主体

## 包含的文件

- `.agents/skills/lite-git/SKILL.md`
- `.agents/skills/lite-git/templates/office.gitignore`
- `.agents/skills/lite-git/templates/office-default-lfs.gitattributes`
- `.agents/skills/lite-git/templates/mixed-binary-lfs.gitattributes`

## 本地检查

在仓库根目录列出可发现的 skill：

```bash
npx skills add . --list
```

安装当前本地仓库里的 skill：

```bash
npx skills add . --skill lite-git -y
```

## 适用说明

这个 skill 更适合下面这类仓库：

- 以 Word、Excel、PowerPoint、PDF、图片等办公文件为主
- 使用者不熟悉 Git 命令
- 希望把“检查 ignore / LFS / 提交 / 切换版本”整理成更安全的流程

如果仓库同时包含大量代码和文档，skill 会倾向于只把明确的二进制文件交给 Git LFS，而不是把所有内容都默认放进 LFS。
# lite-git

[English](./README.md)

这个仓库发布了一个名为 `lite-git` 的 agent skill，目录结构已经按 `vercel-labs/skills` 的发现规则组织，用户可以直接通过 `npx skills add` 安装。

仓库地址：https://github.com/nickhoo555/lite-git

## 用 npx skills 安装

```bash
npx skills add nickhoo555/lite-git --skill lite-git
```

也可以直接从 GitHub URL 安装：

```bash
npx skills add https://github.com/nickhoo555/lite-git --skill lite-git
```

全局安装并跳过交互确认：

```bash
npx skills add nickhoo555/lite-git --skill lite-git -g -y
```

仅安装到 GitHub Copilot：

```bash
npx skills add nickhoo555/lite-git --skill lite-git -a github-copilot -y
```

## 这个 skill 做什么

`lite-git` 面向不懂 Git 的用户，主要处理这些场景：

- 初始化一个适合办公文件的 Git 仓库
- 提交当前版本前检查哪些文件应该忽略
- 判断哪些文件更适合交给 Git LFS 管理
- 初始化时如果缺少 Git LFS，会提示并帮用户安装后继续流程
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

## 示例人机对话（完整流程）

下面是一个从零开始的真实使用流程示例，包含初始化仓库、保存版本、切换到旧版本。

### 场景 1：初始化一个办公文档仓库

用户：
初始化这个文件夹，主要是 Word、PPT、图片。

助手（lite-git）：
我先检查当前状态。这个目录还不是 Git 仓库。
我发现了这些文件：
- 文档：季度汇报.pptx，预算说明.docx
- 图片：封面图.png，流程图.jpg
- 临时文件：~$季度汇报.pptx

建议：
1. 忽略 Office 临时锁文件和系统杂项
2. 文档和图片走 Git LFS
3. 生成首个快照提交

将执行：
- git init -b main
- 写入 .gitignore（忽略临时文件）
- 写入 .gitattributes（LFS 规则）
- git add + git commit

执行完成：
- 已初始化仓库
- 已忽略临时文件：~$季度汇报.pptx
- 已将 docx/pptx/png/jpg 规则设为 LFS
- 首次提交成功：Initial snapshot

下一步你可以继续说：保存当前版本。

### 场景 2：保存当前版本（自动检查 ignore 和 LFS）

用户：
保存当前版本。

助手（lite-git）：
我先检查变更。

发现：
- 已修改：季度汇报.pptx
- 新增：客户案例.png
- 新增：~$季度汇报.pptx（临时文件）

处理：
1. 临时文件不会提交（已忽略）
2. pptx/png 会按 LFS 提交
3. 其余无风险项将直接提交

提交完成：
- commit: Save current draft of slides
- 包含：季度汇报.pptx，客户案例.png
- 未包含：~$季度汇报.pptx（临时文件）

### 场景 3：切换到上周版本并继续改

用户：
切换到上周那个版本，我还要继续改。

助手（lite-git）：
我先检查当前工作区是否干净。
当前没有未提交改动，可以安全切换。

你说“继续改”，我不会让你停在 detached HEAD。
我会从目标提交创建分支并切过去。

执行：
- git log 定位上周提交
- git switch -c revise-from-last-week <commit>

结果：
- 你现在在分支 revise-from-last-week
- 可以直接继续编辑并提交
- 原 main 分支保持不变

## 示例：最短可复制用法

用户可直接输入：

- /版本助手 初始化这个文件夹（办公文件为主）
- /版本助手 保存当前版本并自动检查 ignore 和 lfs
- /版本助手 切到上周版本并继续编辑

也可以输入：

- /lite-git initialize this folder for office files
- /lite-git save current version with ignore/lfs checks
- /lite-git switch to last week version and keep editing
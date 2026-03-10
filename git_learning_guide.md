# Git 与 GitHub 全面学习指南

这份文档旨在帮助你从零开始掌握 Git 版本控制系统和 GitHub 平台的使用。

## 1. Git 基础配置与概念

### 1.1 安装与验证
- **检查安装**: 打开终端输入 `git --version`。
- **配置用户信息** (必须做，否则无法提交):
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your_email@example.com"
  ```
- **查看配置**: `git config --list`

### 1.2 核心概念
- **工作区 (Working Directory)**: 你在电脑里能看到的目录。
- **暂存区 (Staging Area/Index)**: 英文叫 stage 或 index。一般存放在 `.git` 目录下的 index 文件中，所以我们把暂存区有时也叫作索引（index）。
- **版本库 (Repository)**: 工作区有一个隐藏目录 `.git`，这个不算工作区，而是 Git 的版本库。

## 2. 核心命令详解 (本地操作)

### 2.1 初始化与提交
1.  **初始化仓库**: `git init` (在当前目录生成 `.git` 文件夹)
2.  **查看状态**: `git status` (时刻关注文件是在工作区、暂存区还是已提交)
3.  **添加到暂存区**:
    - `git add <filename>` (添加指定文件)
    - `git add .` (添加当前目录所有变更)
4.  **提交到版本库**:
    - `git commit -m "提交说明"` (必须写清楚做了什么修改)
5.  **查看提交历史**:
    - `git log` (详细历史)
    - `git log --oneline` (简洁历史)

### 2.2 撤销与修改
- **撤销工作区的修改**: `git checkout -- <filename>` (丢弃工作区修改，慎用)
- **撤销暂存区的修改**: `git reset HEAD <filename>` (将文件移出暂存区，保留工作区修改)
- **修改上一次提交**: `git commit --amend` (修改最后一次提交的注释或追加文件)

## 3. 分支管理 (Branching)

Git 的杀手级功能。

- **查看分支**: `git branch`
- **创建分支**: `git branch <branch-name>`
- **切换分支**: `git checkout <branch-name>` 或 `git switch <branch-name>`
- **创建并切换**: `git checkout -b <branch-name>`
- **合并分支**:
  1. 切换到目标分支 (如 master/main): `git checkout main`
  2. 合并 dev 分支: `git merge dev`
- **删除分支**: `git branch -d <branch-name>`

## 4. GitHub 远程仓库交互

### 4.1 关联远程仓库
假设你在 GitHub 上新建了一个空仓库。
- **添加远程地址**: `git remote add origin <github-repo-url>`
- **查看远程信息**: `git remote -v`

### 4.2 推送与拉取
- **推送到远程**: `git push -u origin main` (第一次推送需加 `-u` 关联分支，后续只需 `git push`)
- **克隆仓库**: `git clone <github-repo-url>` (下载远程代码到本地)
- **获取更新**: `git fetch` (下载远程更新但不合并)
- **拉取并合并**: `git pull` (等同于 `fetch` + `merge`)

## 5. 团队协作流程

1.  **Fork**: 在 GitHub 上将别人的仓库复制一份到自己的账号下。
2.  **Clone**: 将自己账号下的 Fork 仓库克隆到本地。
3.  **Branch**: 创建新分支开发功能 (不要直接在 main 分支开发)。
4.  **Push**: 将新分支推送到自己的 GitHub 仓库。
5.  **Pull Request (PR)**: 在 GitHub 页面点击 "New Pull Request"，请求原仓库维护者合并你的代码。
6.  **Code Review**: 维护者审查代码，提出意见或合并。

## 6. 常见场景实战

### 场景一：忽略文件 (.gitignore)
创建 `.gitignore` 文件，写入不需要 Git 管理的文件名或目录（如 `node_modules/`, `*.log`, `.env`）。

### 场景二：解决冲突
当合并分支时，如果同一文件的同一行被修改，会产生冲突。
1.  Git 会标记冲突内容 (`<<<<<<<`, `=======`, `>>>>>>>`)。
2.  手动编辑文件，保留想要的代码，删除标记符号。
3.  `git add <file>` 标记冲突已解决。
4.  `git commit` 完成合并。

---
**开始练习吧！** 建议按照文档顺序，先配置好用户信息，尝试创建文件并提交。

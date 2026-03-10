# Git 版本管理与分支策略详解

本文档深入讲解 Git 的核心功能：版本控制（历史回退）和分支管理（开发流）。

## 1. 版本管理：时光机

Git 让你能够查看过去的代码，并在需要时“穿越”回去。

### 1.1 查看历史 (Log)

- `**git log**`: 查看详细的提交历史（作者、日期、提交信息）。
- `**git log --oneline**`: **推荐**。每条日志只显示一行，包含 Commit ID（哈希值）和提交信息。
- `**git log -n 3`**: 只显示最近的 3 次提交。
- `**git log --graph**`: 以图形化方式显示分支合并历史。

### 1.2 回退版本 (Reset & Checkout)

**场景 A：我搞砸了，想丢弃现在的修改，回到上一次提交的状态。**

1. **未暂存 (Unstaged)**: `git checkout -- <filename>` (丢弃工作区修改)
2. **已暂存 (Staged)**: `git reset HEAD <filename>` (移出暂存区) -> `git checkout -- <filename>`

**场景 B：我想让整个项目回退到过去的某个版本。**
假设你想回退到 Commit ID 为 `a1b2c3d` 的版本：

1. **软回退 (Soft Reset)**:
  ```bash
    git reset --soft a1b2c3d
  ```
  - **效果**: 历史记录回退了，但**修改的内容保留在暂存区**。
  - **用途**: 你想重新提交（修改 commit message 或合并多个 commit）。
2. **硬回退 (Hard Reset)**: ⚠️ **危险操作**
  ```bash
    git reset --hard a1b2c3d
  ```
  - **效果**: 历史记录回退，**工作区和暂存区的所有修改全部丢失**，直接恢复到 `a1b2c3d` 的状态。
  - **用途**: 彻底放弃之后的修改。

**场景 C：我不想删除历史，只想撤销某次提交的修改。**

- `**git revert <commit-id>`**: 生成一个新的提交，内容是删除/抵消掉指定 `<commit-id>` 的修改。**推荐在公共分支使用**，因为它不破坏历史。

---

## 2. 分支管理：平行宇宙

分支（Branch）允许你在不影响主线（main/master）的情况下开发新功能。

### 2.1 为什么用分支？

- **并行开发**: 你在开发新功能 A，队友在修复 Bug B，互不干扰。
- **实验性代码**: 尝试新想法，失败了直接删掉分支，主线不受影响。
- **生产环境稳定**: `main` 分支永远保持可发布状态。

### 2.2 常用命令速查

- `git branch`: 查看本地分支。
- `git branch -r`: 查看远程分支。
- `git branch <name>`: 创建新分支。
- `git checkout <name>` / `git switch <name>`: 切换分支。
- `git checkout -b <name>`: 创建并切换。
- `git branch -d <name>`: 删除已合并的分支。
- `git branch -D <name>`: 强制删除分支（即使未合并）。

### 2.3 合并分支 (Merge)

假设你在 `dev` 分支开发完了功能，现在要合并回 `main`。

1. 切换回目标分支：`git checkout main`
2. 执行合并：`git merge dev`

**合并模式**:

- **Fast-forward (快进)**: 如果 `main` 在你创建 `dev` 后没有任何修改，Git 只是把 `main` 的指针直接移到 `dev` 的最新提交。
- **Recursive Strategy (递归)**: 如果 `main` 也有新的提交，Git 会自动创建一个新的 **Merge Commit** 把两条线连起来。

### 2.4 解决冲突 (Conflict)

当 Git 无法自动合并时（例如两人修改了同一行代码），会报 `CONFLICT`。

1. 运行 `git status` 查看冲突文件。
2. 打开文件，搜索 `<<<<<<<`。
  ```text
    <<<<<<< HEAD
    var a = 1; (main 分支的代码)
    =======
    var a = 2; (dev 分支的代码)
    >>>>>>> dev
  ```
3. **手动修改**: 删除标记符号，保留你想要的代码（或者合并两者的逻辑）。
4. 保存文件。
5. `git add <file>`
6. `git commit` (不需要 `-m`，Git 会自动生成合并信息)。

### 2.5 覆盖操作 (Force Push)

⚠️ **极度危险，仅限个人分支使用**

如果你重写了本地历史（比如用了 `git commit --amend` 或 `git rebase`），本地和远程仓库会分叉。
此时正常的 `git push` 会失败。

- `**git push --force`** (或 `-f`): 强制用本地仓库覆盖远程仓库。
- **后果**: 如果别人也在这个分支开发，他们的提交会被你**无情抹除**。
- **安全做法**: 使用 `git push --force-with-lease`，它会检查远端是否有别人新的提交，有的话会阻止覆盖。

---


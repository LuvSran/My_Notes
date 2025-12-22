### 1. 初始化和配置

- `git init`: 在当前目录创建一个新的 Git 仓库。
- `git clone <url>`: 从远程仓库（如 GitHub）克隆一个项目到本地。
- `git config`: 设置用户信息和其他配置项。
    - `git config --global user.name "Your Name"`: 设置全局用户名。
    - `git config --global user.email "your.email@example.com"`: 设置全局邮箱。
    - `git config --list`: 查看所有配置。

### 2. 检查状态和历史

- `git status`: 查看工作区、暂存区的状态（哪些文件被修改、新增或删除）。
- `git log`: 查看提交历史（按时间倒序）。
    - `git log --oneline`: 以简洁的一行形式显示提交历史。
    - `git log --graph`: 以图形化方式显示分支合并历史。
- `git diff`: 查看工作区与暂存区之间的差异，或查看暂存区与最近一次提交（HEAD）之间的差异。
    - `git diff --cached`: 查看暂存区与最近一次提交之间的差异。

### 3. 添加和提交

- `git add <file>`: 将指定文件的修改添加到暂存区。
- `git add .`: 将当前目录下所有被修改、新增或删除的文件添加到暂存区。
- `git commit -m "commit message"`: 将暂存区的文件快照提交到本地仓库历史。
- `git rm <file>`: 从工作区和暂存区删除文件，并准备在下次提交时将其从版本历史中移除。

### 4. 分支操作

- `git branch`: 列出本地所有分支。`*` 标记当前所在分支。
- `git branch <branch_name>`: 创建一个新分支。
- `git checkout <branch_name>`: 切换到指定分支。
- `git switch <branch_name>`: (Git 2.23+) 切换到指定分支，是 `git checkout` 的更清晰的替代命令。
- `git checkout -b <branch_name>`: 创建并切换到新分支。（等同于 `git branch <branch_name>` + `git switch <branch_name>`）
- `git switch -c <branch_name>`: (Git 2.23+) 创建并切换到新分支，是 `git checkout -b` 的替代命令。
- `git merge <branch_name>`: 将指定分支合并到当前分支。
- `git branch -d <branch_name>`: 删除指定的本地分支。

### 5. 远程仓库交互

- `git remote`: 查看当前配置的远程仓库别名。
- `git remote -v`: 查看远程仓库的详细 URL 信息。
- `git remote add <name> <url>`: 添加一个新的远程仓库，通常 `<name>` 为 `origin`。
- `git fetch <remote>`: 从远程仓库获取最新数据（不合并）。
- `git pull <remote> <branch>`: 从远程仓库获取最新数据并合并到当前本地分支。（等同于 `git fetch` + `git merge`）
- `git push <remote> <branch>`: 将本地分支的提交推送到远程仓库。
- `git push -u <remote> <branch>`: 推送并设置上游分支，以后可直接使用 `git push` 和 `git pull`。

### 6. 撤销操作

- `git restore <file>`: (Git 2.23+) 将工作区的文件恢复到最后一次提交（HEAD）的状态。如果文件已在暂存区，需要先用 `git restore --staged <file>` 将其移出暂存区。
- `git restore --staged <file>`: (Git 2.23+) 将文件从暂存区移回工作区（取消 `git add`）。
- `git reset --soft HEAD~1`: 将 `HEAD` 指针指向上一次提交，但保留工作区和暂存区的修改。
- `git reset --mixed HEAD~1`: (默认选项) 将 `HEAD` 指针指向上一次提交，保留工作区修改，但取消暂存区的修改。
- `git reset --hard HEAD~1`: 将 `HEAD` 指针指向上一次提交，并彻底丢弃工作区和暂存区的修改。**危险操作，请谨慎使用！**
- `git revert <commit-hash>`: 创建一个新的提交来撤销指定提交引入的更改。这是一种安全的、用于公共历史的撤销方法。

### 7. 标签（Tags）

- `git tag`: 列出所有标签。
- `git tag <tagname>`: 为当前 `HEAD` 创建一个轻量标签。
- `git tag -a <tagname> -m "annotation"`: 创建一个带注释的标签。
- `git push origin <tagname>`: 推送单个标签到远程。
- `git push origin --tags`: 推送所有标签到远程。

### 8. 其他常用

- `git help <command>`: 查看特定命令的帮助信息。
- `git show <commit-hash>`: 显示某次提交的详细信息及其引入的差异。
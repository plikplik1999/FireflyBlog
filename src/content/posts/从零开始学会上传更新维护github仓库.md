---
title: 从零开始，学会上传，更新，维护github仓库
published: 2026-08-30
pinned: false
description: 一份从头到尾、覆盖安装、配置、创建仓库、上传项目到 GitHub 的完整教程。
tags: [Github]
category: 工具书
author: plikplik
sourceLink: "https://github.com/plikplik1999"
seriesOrder: 1
---
# 从零开始学会上传、更新、维护 GitHub 仓库

一份从头到尾、覆盖安装、配置、创建仓库、上传项目到 GitHub 的完整教程。全程使用通用示例，不含任何具体的仓库链接。

---

## 一、准备工作

### 注册 GitHub 账号

1. 打开浏览器，访问 GitHub 官网（输入 "GitHub" 即可找到）。
2. 点击 **Sign up** 或 **注册** 按钮，按提示输入邮箱、用户名、密码，并完成邮箱验证。
3. 登录后进入个人主页，即可进行后续操作。

---

## 二、安装并配置 Git

### 1. 在不同操作系统上安装 Git

#### Windows

1. 打开浏览器，搜索 "Git Windows 安装"，找到官网下载页面。
2. 下载最新版本安装包（通常以 `.exe` 结尾），双击运行，一路 **Next** 完成安装。
3. 安装过程保持默认设置即可（除非有特殊需求）。

#### macOS

可通过包管理器安装（推荐 Homebrew），在终端执行：

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git
```

如不想使用 Homebrew，也可直接下载 macOS 安装包，按提示安装。

#### Linux（以 Ubuntu 为例）

打开终端，执行：

```shell
sudo apt update
sudo apt install git -y
```

安装完成后，在任意终端（Windows 下可打开 Git Bash）输入：

```shell
git --version
```

如果输出类似 `git version 2.x.x`，说明安装成功。

### 2. 全局配置 Git 用户信息

安装完成后，需要告诉 Git 是谁在提交代码。打开终端依次执行：

```shell
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱@example.com"
```

| 配置项 | 说明 |
| --- | --- |
| `user.name` | GitHub 上注册的用户名或昵称 |
| `user.email` | 与 GitHub 账号绑定的邮箱地址 |

配置完成后，用以下命令检查：

```shell
git config --global --list
```

会看到类似输出：

```text
user.name=你的用户名
user.email=你的邮箱@example.com
```

---

## 三、为安全验证配置 SSH（可选，但推荐）

使用 SSH 方式推送/拉取代码，可避免每次输入用户名和密码（Token）。

### 1. 生成 SSH 密钥对

1. 打开终端（或 Git Bash），执行：

   ```shell
   ssh-keygen -t ed25519 -C "你的邮箱@example.com"
   ```

   - 按提示一路回车即可，默认会在用户主目录生成 `.ssh/id_ed25519`（私钥）和 `.ssh/id_ed25519.pub`（公钥）。
   - 若提示"文件已存在"，可选择覆盖、另存为新文件名，或输入新名称。

2. 生成完成后，查看公钥内容：

   ```shell
   cat ~/.ssh/id_ed25519.pub
   ```

   终端会输出一串以 `ssh-ed25519` 开头的字符串，这就是公钥。

### 2. 将 SSH 公钥添加到 GitHub

1. 登录 GitHub，点击右上角头像，选择 **Settings**（设置）。
2. 在左侧栏找到 **SSH and GPG keys**，点击进入。
3. 点击 **New SSH key**，在 **Title** 框填写一个有意义的名称（如 "My Laptop"），将复制的公钥粘贴到文本框中，最后点击 **Add SSH key**。
4. 如 GitHub 要求输入账号密码或两步验证码，按提示输入即可。

添加成功后测试连接：

```shell
ssh -T git@github.com
```

如果提示类似 `Hi <用户名>! You've successfully authenticated...`，说明 SSH 验证配置正确。

---

## 四、在 GitHub 上创建一个空仓库

1. 登录 GitHub，点击右上角 **+** 按钮，选择 **New repository**（新建仓库）。
2. 在 **Create a new repository** 页面填写：

   | 字段 | 说明 |
   | --- | --- |
   | **Repository name** | 项目名称，例如 `my-project` |
   | **Description** | 可选，添加简短描述 |
   | **Public / Private** | 根据需要选择公开或私有 |
   | **Initialize this repository with a README** | 通常**不勾选**，否则会自动生成 README，后续需先拉取再推送 |

3. 点击 **Create repository** 创建仓库。页面会提供两种地址：

   - HTTPS：`https://github.com/你的用户名/my-project.git`
   - SSH：`git@github.com:你的用户名/my-project.git`

   记住其中一种，后续用于关联本地仓库。

---

## 五、将本地项目与远程仓库关联并上传

假设本地项目路径为 `~/projects/my-project`，目录下已有源代码或资源文件。

### 1. 在本地初始化 Git 仓库

1. 打开终端，切换到项目根目录：

   ```shell
   cd ~/projects/my-project
   ```

2. 在该目录下执行：

   ```shell
   git init
   ```

   这会创建隐藏的 `.git/` 文件夹，使该目录成为 Git 仓库。

3. 可运行 `ls -A` 或 `git status` 确认目录下已有普通文件，但 Git 还未追踪它们。

### 2. 将文件添加到暂存区并提交

1. 查看当前状态（可选）：

   ```shell
   git status
   ```

   此时 Git 会列出未跟踪的文件（Untracked files）。

2. 将所有文件添加到暂存区：

   ```shell
   git add .
   ```

   这会把当前目录（及子目录）下所有未被忽略的文件纳入暂存区。

3. 创建第一次提交：

   ```shell
   git commit -m "首次提交：项目初始化"
   ```

   以上操作会在本地生成一个提交记录，默认在 `master` 分支。如需将主分支命名为 `main`，执行：

   ```shell
   git branch -M main
   ```

   这样本地仓库主分支即为 `main`，与 GitHub 通用约定保持一致。

### 3. 将本地仓库与远程仓库关联

1. 将远程仓库地址添加为名为 `origin` 的远程源：

   ```shell
   # 使用 HTTPS（每次需输入用户名/密码或 Token）
   git remote add origin https://github.com/你的用户名/my-project.git

   # 使用 SSH（已配置 SSH 密钥则无需输入密码）
   git remote add origin git@github.com:你的用户名/my-project.git
   ```

   > 注意：命令开头务必是小写的 `git`，不要带任何隐藏字符或破折号。

2. 确认关联：

   ```shell
   git remote -v
   ```

   使用 HTTPS 时会看到：

   ```text
   origin  https://github.com/你的用户名/my-project.git (fetch)
   origin  https://github.com/你的用户名/my-project.git (push)
   ```

   使用 SSH 时则显示：

   ```text
   origin  git@github.com:你的用户名/my-project.git (fetch)
   origin  git@github.com:你的用户名/my-project.git (push)
   ```

### 4. 推送本地提交到远程仓库

1. 当前分支为 `main` 时：

   ```shell
   git push -u origin main
   ```

   当前分支为 `master`（未重命名）时：

   ```shell
   git push -u origin master
   ```

   > `-u origin main` 表示把本地 `main` 分支推送到远程 `origin` 并建立跟踪关系，之后只需直接执行 `git push`。

2. 推送过程中，使用 HTTPS 需输入 GitHub 用户名和密码（启用两步验证则输入 Personal Access Token）；使用 SSH 且密钥已配置则不再提示输入密码。

3. 推送成功后，终端会显示类似：

   ```text
   Enumerating objects: 10, done.
   Counting objects: 100% (10/10), done.
   Compressing objects: 100% (7/7), done.
   Writing objects: 100% (10/10), 1.23 KiB | 1.23 MiB/s, done.
   Total 10 (delta 0), reused 0 (delta 0)
   To https://github.com/你的用户名/my-project.git
    * [new branch]      main -> main
   branch 'main' set up to track 'origin/main'.
   ```

4. 刷新浏览器，打开 GitHub 仓库页面，即可看到刚上传的所有文件。

---

## 六、后续开发与同步

### 1. 修改/新增文件后上传

1. 在本地修改或新增文件。
2. 查看项目状态：

   ```shell
   git status
   ```

   可看到哪些文件被修改、哪些是新文件。

3. 添加改动到暂存区：

   ```shell
   # 一次添加所有改动
   git add .

   # 只添加某个文件
   git add path/to/yourfile.ext
   ```

4. 提交改动：

   ```shell
   git commit -m "本次更新：简要说明修改内容"
   ```

5. 推送到远程：

   ```shell
   git push
   ```

   由于首次已用 `-u origin main`（或 `master`）建立跟踪关系，后续直接 `git push` 即可。

### 2. 远程仓库有了新提交（多人协作时常见）

1. 推送前先拉取远程更新：

   ```shell
   git pull --rebase origin main
   ```

   或：

   ```shell
   git pull origin main
   ```

   - 带 `--rebase`：保持提交历史线性。
   - 不带：自动创建一个合并提交（Merge commit）。

2. 若出现冲突（Conflict），按提示打开冲突文件，手动保留、删除冲突标记（`<<<<<<<`、`=======`、`>>>>>>>`），然后执行：

   ```shell
   git add 冲突已解决的文件
   git rebase --continue   # 如果使用了 --rebase
   ```

   如果是普通 `git pull`，则执行：

   ```shell
   git add 冲突已解决的文件
   git commit
   ```

   冲突解决完成后再执行 `git push`，即可将本地改动与远程最新内容同步。

---

## 七、常见问题与注意事项

### 1. 命令前不要出现隐藏字符

如果复制粘贴时前面多了不可见破折号或空格，Git 会识别为非法命令，提示 "command not found"。此时按几次退格键清除前导隐藏字符，再手动输入 `git init`、`git add` 等命令。

### 2. `.gitignore` 文件

在项目根目录创建 `.gitignore` 文件，写入不想跟踪（即不想上传）的文件或文件夹规则。常见示例：

```gitignore
node_modules/
*.log
.env
.DS_Store
build/
*.pyc
```

这样可避免把编译生成的临时文件、IDE 配置、依赖包等无关文件上传到仓库。

### 3. 分支管理

默认在 `main`（或 `master`）分支上工作。若要开发新功能或修复 Bug，通常新建分支：

```shell
git checkout -b feature/新功能名称
```

完成开发并测试通过后，切回 `main` 分支并合并：

```shell
git checkout main
git merge feature/新功能名称
git branch -d feature/新功能名称
```

合并完成后再执行 `git push`，即可把合并结果推到远程。

### 4. SSH 与 HTTPS

- 若需频繁推送，推荐使用 SSH 方式：本地生成一次 SSH 密钥并添加公钥到 GitHub，之后无需每次输入用户名/密码或 Token。
- 若安全要求较高或不方便配置 SSH，可继续使用 HTTPS，但每次推送都可能需要输入 Token。

### 5. Token 和安全

- GitHub 已逐步弃用密码进行 HTTPS 验证，强制使用个人访问令牌（Personal Access Token）。
- 终端推送提示输入用户名和密码时，用户名填 GitHub 用户名，密码处粘贴个人访问令牌。令牌可在 GitHub 个人设置里新建。

### 6. 远程仓库已初始化 README 的情况

若新建仓库时勾选了"初始化 README"，远程仓库会自带一个提交记录，此时本地为空，直接 `git push` 会被拒绝。解决方法是先执行：

```shell
git pull --rebase origin main
```

把远程那次提交拉下来并合并到本地，再执行 `git push`。

---

## 八、小结

1. **安装 Git**，配置 `user.name`、`user.email`。
2. **（可选）配置 SSH 密钥**，将公钥添加到 GitHub。
3. **在 GitHub 创建空仓库**（不勾选初始化 README）。
4. **在本地项目目录执行 `git init`**，将项目转为 Git 仓库。
5. **执行 `git add .`、`git commit -m "描述"`**，完成首次提交。
6. **执行 `git remote add origin <远程仓库地址>`** 关联远程。
7. **执行 `git push -u origin main`**（或 `master`）推送本地提交到 GitHub。
8. **后续开发**：用 `git add` → `git commit` → `git push` 完成更新；若需合并远程改动，先 `git pull --rebase` 再 `git push`。

以上步骤覆盖了从安装软件、首次上传，到后续代码同步的完整流程。
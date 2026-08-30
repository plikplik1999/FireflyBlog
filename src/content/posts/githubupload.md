# 从零开始，学会上传，更新，维护github仓库

以下是一份从头到尾、覆盖安装、配置、创建仓库、上传项目到 GitHub 的完整教程。全程使用通用示例，不包含任何具体的仓库链接，仅供参考。

---

## 一、准备工作

### 1. 注册 GitHub 账号

1. 打开浏览器，访问 GitHub 官网（输入 “GitHub” 即可找到）。

2. 点击“Sign up”或“注册”按钮，按提示输入邮箱、用户名、密码，并完成邮箱验证。

3. 登陆后，进入个人主页，即可进行后续操作。

---

## 二、安装并配置 Git

### 1. 安装 Git

  1. 打开浏览器，搜索“Git Windows安装”并找到官网的下载页面。

  2. 下载最新版本的安装包（通常是以 `.exe` 结尾的文件），双击运行后一路“Next”完成安装。

  3. 安装过程中保持默认设置即可（除非你有特殊需求）。

  ```

安装完成后，在任意终端里输入：

```
git --version
```

如果输出类似 `git version 2.x.x`，说明安装成功。

### 2. 全局配置 Git 用户信息

安装完成后，需要告诉 Git 是谁在提交代码。打开终端，依次执行：

```
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱@example.com"
```

- `user.name`：填写你在 GitHub 上注册的用户名或昵称。

- `user.email`：填写与你 GitHub 帐号绑定的邮箱地址。

配置完毕后，可以用下面命令检查：

```
git config --global --list
```

会看到类似：

```
user.name=你的用户名
user.email=你的邮箱@example.com
```

---

## 三、为安全验证配置 SSH（可选，但推荐）

使用 SSH 方式推送/拉取代码，可以避免每次输入用户名和密码（Token）。下面介绍 SSH 公钥的生成与添加。

### 1. 生成 SSH 密钥对

1. 打开终端（或 Git Bash），执行：

   ```
   ssh-keygen -t ed25519 -C "你的邮箱@example.com"
   ```

   - 按提示一路回车即可，默认会在用户主目录下生成 `.ssh/id_ed25519`（私钥）和 `.ssh/id_ed25519.pub`（公钥）两个文件。

   - 如果提示 “文件已存在”，可选择覆盖或另存为其他文件名，也可以按提示输入一个新的名称。

2. 生成完成后，执行以下命令查看公钥内容：

   ```
   cat ~/.ssh/id_ed25519.pub
   ```

   终端会输出一串以 `ssh-ed25519` 开头的字符串，这就是公钥。

### 2. 将 SSH 公钥添加到 GitHub

1. 在浏览器中，登陆 GitHub，点击右上角头像，选择“Settings”（设置）。

2. 在左侧栏中找到 **SSH and GPG keys**，点击进入。

3. 点击 **New SSH key**（新增 SSH 密钥），在“Title”框里可以填写一个有意义的名字（例如 “My Laptop”），然后把刚才复制的公钥粘贴到下面的文本框中，最后点击 **Add SSH key**。

4. 如果 GitHub 要求你输入账号密码或两步验证码，按提示输入即可。

添加成功后，SSH 验证就设置完毕。可以测试连接：

```
ssh -T git@github.com
```

如果提示类似 “Hi <用户名>! You've successfully authenticated...” 则说明 SSH 验证配置正确。

---

## 四、在 GitHub 上创建一个空仓库

1. 登陆 GitHub 后，点击右上角的 “+” 按钮，选择 **New repository**（新建仓库）。

2. 在 “Create a new repository” 页面填写：

   - **Repository name**：为你的项目起个名字，例如 `my-project`。

   - **Description**（可选）：添加简短描述。

   - **Public** 或 **Private**：根据需要选择公开或私有。

   - **Initialize this repository with a README**：通常**不勾选**，否则会自动生成一个 README 文件，后续需要先拉取再推送。

3. 点击下方的 **Create repository**（创建仓库），稍等片刻后就会进入新仓库页面。页面上会提供 HTTPS 和 SSH 两种地址，如：

   - HTTPS: `https://github.com/你的用户名/my-project.git`

   - SSH: `git@github.com:你的用户名/my-project.git`
     记住这两个地址中的一种，后续用来关联本地仓库。

---

## 五、将本地项目与远程仓库关联并上传

下面以一个已存在本地项目的目录为例，演示如何上传到 GitHub。假设本地项目文件夹路径为 `~/projects/my-project`，且目录下已有若干源代码或资源文件。

### 1. 在本地初始化 Git 仓库

1. 打开终端，切换到项目根目录：

   ```
   cd ~/projects/my-project
   ```

2. 在该目录下执行：

   ```
   git init
   ```

   这会创建一个隐藏的 `.git/` 文件夹，使该目录成为一个 Git 仓库。

3. 你可以运行 `ls -A` 或 `git status` 来确认目录下已有普通文件，但 Git 还没追踪它们。

### 2. 将文件添加到暂存区并提交

1. **查看当前状态**（可选）：

   ```
   git status
   ```

   这时 Git 会告诉你哪些文件是 “Untracked files”（未跟踪文件）。

2. **把所有文件添加到暂存区**：

   ```
   git add .
   ```

   这样做会把当前目录（及子目录）下所有未被忽略的文件都纳入暂存区。

3. **创建第一次提交**：

   ```
   git commit -m "首次提交：项目初始化"
   ```

   以上操作会在本地生成一个提交记录（commit），大多数情况下会默认在 `master` 分支上。如果想直接把主分支命名为 `main`，可以执行：

   ```
   git branch -M main
   ```

   这样以后本地仓库的主分支就是 `main`（与 GitHub 通用约定保持一致）。

### 3. 将本地仓库与远程仓库关联

1. 在本地执行下面命令，将远程仓库地址添加为名为 `origin` 的远程源：

   - **使用 HTTPS（需要每次输入用户名/Token）：**

     ```
     git remote add origin https://github.com/你的用户名/my-project.git
     ```

   - **使用 SSH（已配置 SSH 密钥则无需输入密码）：**

     ```
     git remote add origin git@github.com:你的用户名/my-project.git
     ```

   注意：命令开头务必是小写的 `git`，不要带任何隐藏字符或破折号。

2. 添加成功后，可用下面命令确认：

   ```
   git remote -v
   ```

   你会看到类似：

   ```
   origin  https://github.com/你的用户名/my-project.git (fetch)
   origin  https://github.com/你的用户名/my-project.git (push)
   ```

   或者如果用的是 SSH，则显示：

   ```
   origin  git@github.com:你的用户名/my-project.git (fetch)
   origin  git@github.com:你的用户名/my-project.git (push)
   ```

### 4. 推送本地提交到远程仓库

1. 如果当前分支是 `main`：

   ```
   git push -u origin main
   ```

   如果当前分支是 `master`（没有重命名为 `main`），则：

   ```
   git push -u origin master
   ```

   - `-u origin main` 的含义是：把本地 `main` 分支推送到远程 `origin`，并建立跟踪关系。以后只需直接执行 `git push` 即可。

2. 在推送过程中，如果使用 HTTPS，需要输入 GitHub 用户名和密码（如果启用了两步验证，则输入 Personal Access Token）；如果使用 SSH，且 SSH key 已配置好，则不会再提示输入密码。

3. 推送成功后，终端会显示类似：

   ```
   Enumerating objects: 10, done.
   Counting objects: 100% (10/10), done.
   Compressing objects: 100% (7/7), done.
   Writing objects: 100% (10/10), 1.23 KiB | 1.23 MiB/s, done.
   Total 10 (delta 0), reused 0 (delta 0)
   To https://github.com/你的用户名/my-project.git
    * [new branch]      main -> main
   branch 'main' set up to track 'origin/main'.
   ```

4. 刷新浏览器，打开 GitHub 上该仓库页面，就能看到你刚才上传的所有文件。

---

## 六、后续开发与同步

项目上传成功后，后续只需按照下面步骤更新远程仓库即可。

### 1. 修改/新增文件后上传

1. 在本地对文件进行修改或新增文件。

2. 查看项目状态：

   ```
   git status
   ```

   可以看到哪些文件被修改、哪些是新文件。

3. 添加改动到暂存区：

   - 如要一次添加所有改动：

     ```
     git add .
     ```

   - 如果只想添加某个文件：

     ```
     git add path/to/yourfile.ext
     ```

4. 提交改动：

   ```
   git commit -m "本次更新：简要说明修改内容"
   ```

5. 推送到远程：

   ```
   git push
   ```

   由于第一次已经用 `-u origin main` 或 `-u origin master` 与远程分支建立了跟踪关系，后续直接 `git push` 即可。

### 2. 如果远程仓库有了新的提交（多人协作时常见）

1. 在推送之前，可以先拉取远程更新：

   ```
   git pull --rebase origin main
   ```

   或者：

   ```
   git pull origin main
   ```

   - 带 `--rebase` 可以保持提交历史线性；不带则会自动创建一个合并提交（Merge commit）。

2. 如果出现冲突（Conflict），按提示打开冲突文件，手动保留、删除冲突标记（`<<<<<<<`、`=======`、`>>>>>>>`），然后执行：

   ```
   git add 冲突已解决的文件
   git rebase --continue   # 如果使用了 --rebase
   ```

   或者如果是普通 `git pull`：

   ```
   git add 冲突已解决的文件
   git commit
   ```

   冲突解决完成后再执行 `git push`，即可把本地改动与远程最新内容同步。

---

## 七、常见问题与注意事项

1. **命令前不要出现隐藏字符**

   - 如果复制粘贴时前面多了不可见破折号（如 “”）或空格，Git 会识别为非法命令，从而提示 “command not found”。遇到这种情况，请按几次退格键把前导的隐藏字符清除，然后再手动输入 `git init`、`git add` 等命令。

2. **.gitignore 文件**

   - 在项目根目录创建一个名为 `.gitignore` 的文件，写入你不想跟踪（即不想上传到远程）的文件或文件夹规则。常见示例：

     ```
     node_modules/
     *.log
     .env
     .DS_Store
     build/
     *.pyc
     ```

   - 这样做可以避免把编译生成的临时文件、IDE 配置、依赖包等无关文件上传到仓库。

3. **分支管理**

   - 默认情况下，你会在 `main`（或 `master`）分支上工作。如果想开发新功能、修复 Bug，通常会新建分支：

     ```
     git checkout -b feature/新功能名称
     ```

   - 完成开发并测试通过后，切回 `main` 分支并合并：

     ```
     git checkout main
     git merge feature/新功能名称
     git branch -d feature/新功能名称
     ```

   - 合并完成后，再执行 `git push` 即可把合并后的结果推到远程。

4. **SSH 与 HTTPS**

   - 如果长时间需要频繁推送，推荐使用 SSH 方式，只需在本地生成过一次 SSH 密钥，并把公钥添加到 GitHub，就不需要每次输入用户名/密码或 Token。

   - 如果安全要求较高或不方便配置 SSH，也可以继续使用 HTTPS，但每次推送时都可能需要输入 Token。

5. **Token 和安全**

   - GitHub 已逐步弃用直接使用密码进行 HTTPS 验证，强制使用个人访问令牌（Personal Access Token）。

   - 在终端推送时，如果提示要求用户名和密码，你可以把用户名填为 GitHub 用户名，然后把个人访问令牌粘贴到密码处。访问令牌可以在 GitHub 个人设置里新建。

6. **远程仓库已初始化 README 情况**

   - 如果你在新建仓库时勾选了“初始化 README”，远程仓库就会自带一个提交记录。这时本地一开始是空的，如果你直接执行 `git push` 会被拒绝（提示需要先拉取远程更改）。解决方法是先执行：

     ```
     git pull --rebase origin main
     ```

     把远程的那次提交拉下来并合并到本地，然后再 `git push`。

---

## 八、小结

1. **安装 Git** 并配置 `user.name`、`user.email`。

2. **（可选）配置 SSH 密钥**，将公钥添加到 GitHub。

3. **在 GitHub 上创建一个空仓库**（不勾选初始化 README）。

4. **在本地项目目录执行 `git init`**，将项目转为 Git 仓库。

5. **执行 `git add .`、`git commit -m "描述"`**，完成首次提交。

6. **使用 `git remote add origin <远程仓库地址>`** 关联远程。

7. **执行 `git push -u origin main`**（或 `master`）将本地提交推送到 GitHub。

8. **后续开发**：用 `git add` → `git commit` → `git push` 完成更新。若需合并远程改动，先 `git pull --rebase` 再 `git push`。

以上步骤涵盖了从安装软件到首次上传，再到后续代码同步的完整流程。只要每一步都按照说明并自己手动输入命令，就能顺利把项目上传并保持与 GitHub 的同步。

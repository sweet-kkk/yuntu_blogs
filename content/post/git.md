---
title: Git 工具使用
description: Git 是一个分布式版本控制系统，用于跟踪代码变更、协作开发和管理代码版本。此文档主要描述常用的 `Git` 使用场景，非所有 `Git` 命令讲解。

date: 2025-04-03T22:51:52+08:00
lastmod: 2025-04-03T22:51:52+08:00
tags:
  - tools
categories:
  - tools
math: true
mermaid: true
---


## Git(必备)

Git 是一个分布式版本控制系统，用于跟踪代码变更、协作开发和管理代码版本。此文档主要描述常用的 `Git` 使用场景，非所有 `Git` 命令讲解，关于 `Git` 命令大全请转至[吐血整理，全网最全Git命令手册 - 知乎](https://zhuanlan.zhihu.com/p/389814854)。

```bash
# 初始化和克隆仓库
git init					# 在当前目录初始化一个 Git 仓库
git clone <repo>			# 克隆远程仓库到本地

# 代码提交和管理
git add <file>				# 添加文件到暂存区（index）
git commit -m "message"		# 提交代码到本地仓库
git status					# 查看当前工作区状态
git log						# 查看提交历史

# 分支管理
git branch					# 查看本地分支
git branch <name>			# 创建新分支
git checkout <branch>		# 切换分支
git merge <branch>			# 合并分支
git rebase <branch>			# 变基合并

# 远程仓库操作
git remote -v				# 查看远程仓库信息
git remote add origin <url>	# 添加远程仓库
git push origin <branch>	# 推送本地代码到远程仓库
git pull origin <branch>	# 拉取远程仓库最新代码

# 代码回退和撤销
git reset --hard <commit>	# 回退到指定提交，并清除所有更改
git revert <commit>			# 反向提交，撤销指定提交的更改
git stash					# 暂存当前修改，恢复到干净工作区
```

### 1. Git信息配置

此信息用于推送到远程仓库的提交信息。

```powershell
# 查看 Git 所有配置
git config --list

# 全局信息配置
git config --global user.name "Administrator" 
git config --global user.email "admin@example.com" 

# 当前 Git 仓库配置 
git config --local user.name "你的用户名"
git config --local user.email "你的邮箱"
```

- `git config --global user.name "Administrator"`：设置 Git 全局用户名为 "Administrator"。
- `git config --global user.email "admin@example.com"`：设置 Git 全局用户邮箱为 "[admin@example.com](mailto:admin@example.com)"。

### 2. 克隆并推送一个仓库

```powershell
git clone http://b5bd5b9a12c2/root/springcloud.git 
cd springcloud 
touch README.md 
git add README.md 
git commit -m "add: README" 
git push -u origin master
```

- `git clone http://b5bd5b9a12c2/root/springcloud.git`：将远程仓库的代码克隆到本地。
- `cd springcloud`：进入本地代码仓库目录。
- `touch README.md`：在本地代码仓库中创建一个名为 README.md 的文件。
- `git add README.md`：将新创建的文件添加到 Git 仓库中。
- `git commit -m "add: README"`：提交新文件的修改，并添加提交信息 "add README"。
- `git push -u origin master`：将本地代码仓库的修改推送到远程仓库的 master 分支。

### 3. 推送一个已有的文件夹

```powershell
cd existing_folder 
rm -rf .git		
git init 
git remote add origin http://b5bd5b9a12c2/root/springcloud.git 
git add . 
git commit -m "Initial commit" 
git push -u origin master
```

在已有本地仓库的情况下，将其与远程仓库关联：

- `cd existing_folder`：进入已有本地仓库的目录。
- `rm -rf .git`：删除原仓库的 `git` 信息配置。
- `git init`：将该目录初始化为 Git 仓库。
- `git remote add origin http://b5bd5b9a12c2/root/springcloud.git`：将远程仓库的地址添加为本地仓库的远程地址。
- `git add .`：将本地仓库中所有未被 Git 忽略的文件添加到 Git 仓库中。
- `git commit -m "Initial commit"`：提交修改，并添加提交信息 `"Initial commit"`。
- `git push -u origin master`：将本地代码仓库的修改推送到远程仓库的 master 分支。

> 注意：这将会本地文件夹替换远程仓库的内容，适合对空白仓库的提交。
>

### 4. 关联并推送已有 Git 仓库

```powershell
cd existing_repo 
git remote rename origin old-origin 
git remote add origin http://b5bd5b9a12c2/root/springcloud.git 
git push -u origin --all 
git push -u origin --tags
```

最后是在已有本地仓库的情况下，将其与一个新的远程仓库关联：

- `cd existing_repo`：进入已有本地仓库的目录。
- `git remote rename origin old-origin`：将原本的远程仓库地址重命名为 old-origin。
- `git remote add origin http://b5bd5b9a12c2/root/springcloud.git`：将新的远程仓库地址添加为本地仓库的远程地址。
- `git push -u origin --all`：将本地仓库的所有分支推送到新的远程仓库。
- `git push -u origin --tags`：将本地仓库的所有标签推送到新的远程仓库。

> 也可以删除仓库目录下的 `.git` 文件，然后初始化 `Git` 仓库，执行 [3. 推送一个已有的文件夹](#3. 推送一个已有的文件夹)

### 5. 分支管理

首先，确保你本地有一个 `Git` 仓库，可以通过 `git clond <url>` 或 `git clond -b <branch> <url>` 拉取主分支或者指定分支的仓库。以下通过 `yuntu-dev` 分支做演练。

**查看分支：**

如果你不确定远程仓库 `origin` 上是否有 `yuntu-dev` 分支，可以通过以下命令查看远程分支：

```bash
# 查看远程分支
git branch -r

# 查看所有本地分支
git branch

# 查看所有分支
git branch -a
```

**创建分支：**

```bash
# 基于当前分支创建新分支
git checkout -b yuntu-dev  
 
# 在当前仓库中，从远程分支拉取分支（如果远程有 yuntu-dev 分支）  
git checkout -b yuntu-dev origin/yuntu-dev
```

```bash
# 添加修改到提交
# git add .
# git commit -m "add: 添加了单元测试"

# 推送分支到远程仓库
git push -u origin yuntu-dev
```

- 因为 `yuntu-dev` 分支在远程仓库中可能还不存在，所以你需要使用 `-u` 或 `--set-upstream` 选项来设置上游（即跟踪）分支。
- 这条命令会推送 `yuntu-dev` 分支到远程仓库，并将其设置为本地 `yuntu-dev` 分支的上游分支。以后，你可以直接使用 `git pull` 和 `git push` 而不必指定远程仓库和分支名。

> 注意：在提交分支时，`yuntu-dev` 分支应包含了你想要推送的更改并添加到提交。

**远程仓库 url 配置：**
如果上述步骤都正确无误，但你还是遇到了问题，那么请检查你的远程仓库 URL 是否正确。

```bash
# 查看当前仓库的远程仓库信息
git remote -v
```

如果远程仓库并不是目录仓库，可以通过 `set-url` 进行修改。

```bash
# 修改远程仓库 url
git remote set-url origin git@gitlab.dev.wiqun.com:DP/pipelined_script/ops.git
```

> 注意：上面的 URL 是一个 SSH 格式的示例，如果你的 GitLab 支持 HTTPS，并且你之前是通过 HTTPS 克隆的仓库，那么你的 URL 可能以 http:// 或 https:// 开头。

**修改本地分支名称：**

常见错误：`error: src refspec xxx does not match any / error: failed to push some refs to`

```bash
# 由于仓库名称不一样，导致远程和本地的仓库不能关联上
# 统一远程和本地的仓库名称即可
# 把本地的 master 仓库名称修改为远端的 main
git branch -m master main
git push -u origin main
```

### 6. 推送信息

**修改最新提交信息:**

如果只是提交信息错了，可以直接修改.

```bash
git commit --amend -m "正确的提交信息"
git push --force origin 分支名  	  # 强制推送
```

> ⚠ **注意**：强制推送 (`--force`) 可能会影响其他人同步代码，建议确保团队成员未拉取你的错误提交。

**回退提交并重新提交:**

如果提交已经被推送，回退之后提交需要添加 `--force` ，如果没有提交则不需要。

```bash
git reset --soft HEAD~1  			# 回退到上一次提交，保留修改
git add .   						# 重新添加修改
git commit -m "正确的提交信息"
git push --force origin yuntu-dev
```


### 7. Git 的 ssh 连接

`Gitlab` 和 `Github` 都是 `Git` 远程仓库，添加 `ssh` 密钥方法一致，这里以 `Github` 为例。`ssh` 密钥是用于与 `GitHub` 仓库安全通信的加密公钥。没有公开的 `ssh` 密钥，你将无法通过 `ssh` 协议克隆、推送或拉取 **GitHub** 上的仓库。

```bash
# 生成 ssh 密钥
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# 启动 SSH 代理并添加私钥
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

# 查看 ssh 公钥文件
cat ~/.ssh/id_rsa.pub
```

- 生成一个新的 ssh 密钥。在终端中运行以下命令，并按提示操作。如果你没有特别指定的邮箱地址，请使用你的GitHub注册邮箱。
- 确保你的 ssh agent正在运行，并且已经添加了你的新 ssh 密钥。可以使用以下命令启动 ssh agent（如果已经启动，则跳过）并添加你的 ssh 密钥：
- 将你的 ssh 公钥添加到 `GitHub` 账户。

如果你使用的是Linux或macOS，可能需要使用xclip或xsel命令查看公钥。

1. 转到你的 GitHub 账户设置，点击 SSH 和 GPG 密钥菜单项，然后点击"New SSH key"。
2. 在弹出的表单中，为你的密钥起一个名字，然后粘贴你的 SSH 公钥内容到 `Key` 文本框中。
3. 点击"Add SSH key"按钮保存你的密钥。

完成这些步骤后，你能够通过SSH与GitHub通信，并且不再看到原来的错误信息。
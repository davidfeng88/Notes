# Pro Git

![](../.gitbook/assets/progit2.png)

## 第一章 起步

```bash
# check if git is installed
which git

git --version
```

### Git config

| **级别** | **文件位置** | **flag** |
| :--- | :--- | :--- |
| 系统级 | /etc/gitconfig | --system |
| 用户级 | ~/.gitconfig | --global |
| repo | .git/config | --local \(default\) |

```bash
# set user name and email
git config --system user.name "David Feng"
git config user.email davidfeng@abc.com

# see the config
git config --list

# get help
git help config
# same as man git-log

# other common configs
core.editor "vim"
color.ui true
```

**Note**: env variables such as `GIT_AUTHOR_NAME`, `GIT_AUTHOR_EMAIL`, `GIT_COMMITTER_NAME`, `GIT_COMMITTER_EMAIL` will override the user name and email in config.

### Git completion

{% embed url="https://github.com/git/git/tree/master/contrib/completion" %}

## 第二章 Git基础

### 查看修改

`git status`

`git diff`: difference between working directory vs. staged files.

`git diff --staged`: staged files vs. committed files.

### Commit

`git commit -a -m "msg"`: commit all tracking files, skip staging \(no need for `git add`\)

### 移除和移动文件

Remove a file

* Method 1: remove it in OS, then git add/rm.
* Method 2: `git rm file`. It removes the file \(not in trash\) and adds this change to staging.

Rename a file: method 1: do it in OS, or use `git mv`  
`git mv file1 file2`: same as three commands - `mv file1 file2; git rm file1; git add file2`



---- In progress ----

Git log

show git diff of the commits

git log -p  
限制输出长度 --since/after/until/before  
only show changes for : git log path

only show commits that add/delete a function or variable: git log -S function\_name

#### 撤销操作

replace the previous commit: git commit —amend  
discard changes for a file: git checkout —filename

#### Remote Repo

List all of them: git remote  
add a remote repo: git remote add alias url  
fetch \(not merged yet\): git fetch &lt;remote name&gt;  
push: git push origin master  
show all branches of remote repo and their relations with local branches: git remote show origin

#### Git alias

git config --global alias.co checkout

then git co =&gt; git checkout

### **第三章 Git分支**

**Branches are pointer to commit objects  
HEAD: an alias for current branch**

#### **Create a new branch and switch to it**

**git checkout -b newbranch =&gt; 1. git branch newbranch \(create a new branch\); 2. git co newbranch**

#### **Merge a new branch**

**git checkout master  
git merge js1234**

#### **Branch Dev workflow**

**multiple branches.**

**More stable, less feature master/next/feature less stable, more feature  
prod\_an/mass-release**

#### **Remote Branches**

**fetch: git fetch origin  
push: git push origin js1234**

**show local branches and tracked remote branches: git branch -vv**

**show all local branches and last commits: git branch -v**

**pull: git pull \(fetch + merge\)**

#### **Rebase**

**Calculate changes in commits in the new branch; discard those commits; generate new commits containing those changes on top of the new based branch. Basically rewrite commit history so no branch happend. Everything is linear.**

1. **git checkout feature**
2. **git rebase master**
3. **git checkout master**
4. **git merge feature \(fast forward\)**

**shortcut: git rebase \[new base branch\] \[topic branch\]**  


**A more advanced example:**

**master branch out server. On server, branch out client. How can I merge client into master, but don’t merge server?**

**git rebase --onto master server client**

**=&gt; get client branch, find client specific change \(after the common ancestor of client and server\), add those changes to master.  
Don't rebase branches that have copies outside of your local repo.  
A common fix: git pull --rebase**

#### **Merge vs. Rebase**

**Merge: commit history should show what happened. We should not change it.**

**Rebase: commit history is a story. No one wants to read the first draft of your novel.**

### **第四章 Git on Server**

**There are several protocols, each with pros and cons: local, http, ssh, git.**

**bare repo: no working directory, just .git folder.**

### **第五章 分布式Git**

**git diff -- check: run to show common errors such as whitespaces at the end of line.  
  
Commit message:  first line: summary , empty line, followed by explanation.  
  
git log --no-merge issue..origin/master only show commits in origin/master but not in issue  
  
git cherry-pick hash: pull one commit into current branch \(by creating a new commit\)  
On master, pick one of the commits on feature branch**

### **第六章 GitHub**

### **第七章 Git工具（高级命令）**

#### **Parent commit**

**Git show hash  
HEAD~ HEAD~2 \(HEAD^2 is for merge commit, which has two parents\)  
Or HEAD~~^**

#### **Commit intervals**

**Git log master..feature  
Log commits that are in feature but not master  
Git log origin/master..HEAD  
Git log refA refB ^refC \(show commits that are in refA, in refB, not in refC\)  
Git log --left-right master...feature \(show commits in either branch but not both\)**

#### **Remove untracked files**

**Git clean  
-n dryrun  
-f force  
-d also removes empty directory**

#### **Search string**

**Git log -S string  
Show commits that adds or modifies the string**

**Git log -L :function:file.c  
Git will get the lines of the function and search for the change history of those lines**

#### **Rewrite history**

**just like rebase, don't do it if you already pushed.  
Modify the last commit message:  
Git commit --amend**

#### **Reset and checkout**

**reset: move where HEAD points to. checkout: change HEAD.**

**i.e. if we are on master, \(HEAD is on master\), and we do git reset hash, then master/HEAD will be on that hash.**  
  


**the model of three trees:  
HEAD, index/staging, working directory  
HEAD is last commit, the parent of next commit.**

  
**Git reset hash: try to move Head to that commit**

  
**Git reset --soft HEAD~  
Move head to its parent. No change to index and working directory  
Now we can modify staging  
Just revert HEAD \(revert git commit\)**

  
**--mixed default behavior revert head and staging \(revert git add\)  
--hard revert all three, including working directory  
  
Git reset file.txt \(default hash is HEAD\)  
Effectively unadd file.txt  
Git reset hash file.txt  
  
Git reset --soft HEAD~2  
Reset HEAD but no changes in index and working directory  
So for commit will effectively squash commits  
Then push will only push reachable history**  
  


**Checkout:**

**Gco \[branch\] =&gt; git reset --hard \[branch\] but safer. Will stop if working directory is not clean.  
Checkout: move HEAD to another branch  
Reset: move HEAD with the current branch  
Checkout file.txt: reset --hard**

#### **Advanced Merge**

**Git merge --abort abort the merge  
Git checkout --conflict=diff3 hello.rb shows ours, theirs, and the base  
  
Ours option \(just use our code, discard their changes. Useful when we know that we should always use one side’s code, i.e. generated file\) vs. ours strategy \(lie to git, let it think we merged but no code is changed\)**

#### **Debug**

**Git blame -L 12,20 file.txt  
^hash means it's the first added line and never modified afterwards  
-C also get lines that added from other files  
Git bisect can help you find out which commit introduced the error**

#### **Submodules**

**To include other repo \(may be some dependencies\) in your repo you want to distribute within your software**

#### **Git bundle**

**Bundle git data into an binary so that we can email it.**

#### **Git replace**

**Use an object \(commit\) to replace another object. Can be used to create a shorter commit history.**

### **第八章 配置Git**

**help.autocorrect 50**

**if you input a command that does not exist, git will guess. If there is only one candidate, git will run it after 5s.**

#### **git attributes**

**apply certain configs in certain paths**

#### **git hooks**

**any script that will be run at certain times, e.g. before commits.**

### **第九章 Git与其他VCS**

### **第十章 Git内部原理**

**Git对象**

**blob对象 =&gt; 文件的不同版本**

**树对象 =&gt; 目录项**

**commit对象 =&gt; 指明顶层树对象和父提交对象的提交对象**

**Git引用**

 **branch: a pointer to the head of a series of commits.  
 HEAD: a pointer to the current branch  
  
Commits: pointer to tree.  
Tag object: pointer to commit. A branch that can't move  
  
Search files in path  
find .git/objects -type f**  
  


**loose: store multiple copies of slightly different versions of a file**

**pack: Store the latest version of files and delta to previous versions  
git gc manual pack, remove object that are not connected with any commits.**

#### **How to recover from hard reset**

**method1:**

**git reflog or git log -g**

**shows the historical value of HEAD. to get lost commit hash**

**git branch newBranch hash  
  
method2: \(big gun\) git fsck --full  
 show dangling objects**  
  
  


**---- 整理至此 ----  
git prune  
  
Git bash utility! Auto complete?  
  
Prompt??**  


**常用命令**

**Git short log**  


**整理git reset 整理笔记**  
  
  
  


**low level commands: plumbing. high level: porcelain  
git init projectname  
  
Git对象  
Save object to git  
git hash-object -w --stdin \# --stdin: use stdin as source. default: at the end, put the source \(file\)’s path. -w: save the object.  
it produces a 40-character hash. first 2 are directory name, last 38 are filename.  
Retrieve data:  
git cat-file -p HASH \# -p: pretty-print object's content  
git cat-file -t HASH \# give the object’s type: blob 数据对象  
  
树对象 （类似于目录）  
git cat-file -p master^{tree} \# master分支上最新的提交所指向的树对象。  
创建树对象：git 根据暂存区状态创建并记录一个对应的树对象。1. 创建暂存区。2. update-index.  
git update-index --add --cacheinfo 100644 \  
83baae61804e65cc73a7201a7252750c76066a30 test.txt  
\# --add 将该文件加入暂存区 --cacheinfo要添加的文件位于git数据库中，而不是当前目录下，文件模式100644普通文件，100755可执行文件，120000符号链接。  
git write-tree \#将暂存区内容写入一个树对象 不需要-w,如果某个树对象此前不存在，会自动创建  
git read-tree --prefix=bak HASH: 可以把HASH代表的树对象读入暂存区。若基于该暂存区创建树对象  
git write-tree  
再基于此树对象创建一个工作目录，则这个工作目录会包含bak子目录  
  
提交对象commit object  
创建提交对象: 内容：一个树对象，注释，父提交对象  
echo ‘first commit’ \| git commit-tree TREEHASH  
echo ‘second commit’ \| git commit-tree TREEHASH \[ -p PARENT COMMIT HASH\]  
  
git log --stat COMMITHASH  
  
git add & git commit的底层命令：将被改写的文件保存为数据对象，更新暂存区，记录树对象，最后创建提交对象（明确顶层树对象和父提交对象）。  
  
Git引用Refs  
commit的代号  
echo COMMITHASH &gt; .git/refs/heads/master  
编辑引用文件的推荐方法：Do not edit ref file. Use update-ref instead.  
git update-ref refs/heads/master NEWCOMMITHASH  
  
git branch \(branchname\)的底层命令：update-ref  
  
HEAD: 符号引用。不像普通引用一样包含最新commit对象的SHA-1.它是一个指向其他引用的指针。  
  
git checkout的底层命令：更新.git/HEAD  
git commit: 创建commit object, 并用HEAD文件中那个引用指向的SHA-1设置成父commit object.  
编辑HEAD文件的推荐方法：symbolic-ref  
git symbolic-ref HEAD \#读  
git symbolic-ref HEAD refs/heads/test \#写  
  
ToDo: 标签引用**  


**git prune ????**

**git gc ????**  


**Git config --list show me this: what do they mean?**

**credential.helper=osxkeychain**

**alias.a=add**

**alias.aa=add --all :/**

**alias.alias=config --get-regexp ^alias\.**

**alias.b=branch**

**alias.ci=commit**

**alias.co=checkout**

**alias.d=diff --ignore-space-change**

**alias.l=log --pretty=format:"%C\(green\)%h%C\(reset\) %C\(blue\)%ad%C\(reset\) %s%C\(yellow\)%d%C\(reset\) %C\(blue\)\[%an\]%C\(reset\)" --graph --date=short**

**alias.s=status**

**alias.r=remote**

**color.ui=true**

**core.excludesfile=~/.gitignore**

**push.default=simple**

**user.name=davidfeng88**

**user.email=david.feng.ge@gmail.com**

**core.repositoryformatversion=0**

**core.filemode=true**

**core.bare=false**

**core.logallrefupdates=true**

**core.ignorecase=false**

**core.precomposeunicode=true**

**remote.origin.url=https://github.com/davidfeng88/bara.git**

**remote.origin.fetch=+refs/heads/\*:refs/remotes/origin/\***

**branch.master.remote=origin**

**branch.master.merge=refs/heads/master**

**user.name=davidfeng88**

**remote.heroku.url=https://git.heroku.com/bara1.git**

**remote.heroku.fetch=+refs/heads/\*:refs/remotes/heroku/\***  



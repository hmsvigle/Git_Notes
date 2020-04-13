# Git_Notes

1. [Uninstall/Remove Git from ubuntu machine](https://github.com/hmsvigle/My_Git_Notes#1-uninstallremove-git-from-ubuntu-machine)
2. [Install git in Ubuntu machine](https://github.com/hmsvigle/My_Git_Notes#2-Install-git-in-Ubuntu-machine)
3. [Configure git parameters](https://github.com/hmsvigle/My_Git_Notes#3-configure-git-parameters)
4. [Create local Repository](https://github.com/hmsvigle/My_Git_Notes#4-create-local-repository)
5. [Sync remote repository](https://github.com/hmsvigle/My_Git_Notes#5-sync-remote-repository)
6. [Make necessary code changes](https://github.com/hmsvigle/My_Git_Notes#6-make-necessary-code-changes)
7. [Parallel Developement in Git](https://github.com/hmsvigle/My_Git_Notes#7-parallel-developement-in-git)
8. [Pull-Fetch & Rebase-Merge](https://github.com/hmsvigle/My_Git_Notes#8-difference-between-pull-fetch--rebase-merge)
9. [Merge Branch to master](https://github.com/hmsvigle/My_Git_Notes#9-merge-a-branch-to-master)
10. [Git Push](https://github.com/hmsvigle/My_Git_Notes#10-push-the-branch-any-branch-even-master-to-the-remote-repo)

### 1. Uninstall/Remove Git from ubuntu machine
```sh
$ sudo apt-get autoremove purge -y git
$ sudo rm -rf ~/.git
$ sudo rm -rf ~/.gitconfig
```
- Check if git is still installed or not !!
```sh
$ which git '# no result'
```
### 2. Install git in Ubuntu machine
```sh
$ sudo apt-get -y update
$ sudo apt-get install -y git
$ git --version '# git version 2.7.4'
```
### 3. Configure git parameters
```sh
$ git config --list '# Lists all global configurations for git --> None for now'
$ git config --global user.name "hmsvigle"
$ git config --global user.email "hmsvigle@gmail.com"
$ git config --global core.autocrlf input '# ensures consistent input behavior for all contributers'
$ git config --global color.ui auto
```
  - Display all config of git
```yaml
$ cat ~/.gitconfig 
[user]
	name = hmsvigle
	email = hmsvigle@gmail.com
[core]
	autocrlf = input
[color]
	ui = auto
```
> Notes !!
```sh
 $ ~/.git/config --> Contains each specific repository related configurations
 $ ~/.gitconfig --> default git configurations for the user
```

### 4. Create local Repository
```sh
$ git init
```
 - This will add .git directory under the current directory. 
 - Below is the tree structure of it.
```sh
$ tree .
.git
├── branches
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

9 directories, 13 files
```
### 5. Sync remote repository
  - git should identify to which remote directory data sync (pull/push) happen.
  - add the remote repo as origin
  - pull files
  - Make changes
  - push 
  
```sh
$ git remote add origin <git-repo-url>
$ git pull origin master
$ ls  --> the remote repository is synced to local
  READEME.md  .git
```
### 6. Make Necessary code changes
  - git status --> Status of files added/staged/ready to commit
  - git add -A --> Stage required files 
  - git commit -m "msg" -->  commit staged files
  - git status   
```sh
$ git status
On branch master
$ cp -r ../<>/example1 .
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	example1/
```
  - after copying a directory where my actual data resides to the git directory, git status identifies it as a new file, which is still untracked as its is not added to git index, but available in the git directory.
```sh
$ git add example1
$ git status
  On branch master
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

	new file:   example1/Dockerfile
	new file:   example1/README.md
	new file:   example1/api/Dockerfile
	new file:   example1/api/app-1.py
	new file:   example1/api/app-flask.py
	new file:   example1/api/requirements.txt
	new file:   example1/redis/Dockerfile
```
  - Now the new data has been staged for commit & reflects as "changes to commit" with `$ git status` command
  - if there are multiple files to be staged use : `$ git add -A`
```sh
$ git commit -m "add_word & autocomplete redis API exposure through Flask"
 [master xxxx] add_word & autocomplete redis API exposure through Flask
   7 files changed, 218 insertions(+)
   create mode 100644 example1/Dockerfile
   create mode 100644 example1/README.md
   create mode 100644 example1/api/Dockerfile
   create mode 100755 example1/api/app-1.py
   create mode 100755 example1/api/app-flask.py
   create mode 100644 example1/api/requirements.txt
   create mode 100644 example1/redis/Dockerfile
```
  - Track all the changes made to git index
```sh
$ git log
 commit bc56ecfebcef3b3cdde4135a4b7a287fde44bfd2
 Author: hmsvigle <hmsvigle@gmail.com>
 Date:   Sun Apr 12 19:20:26 2020 +0530
 
    add_word & autocomplete redis API exposure through Flask

 commit 970c4dad9efb0fbe2ffb4c21c082b385de657ab9
 Author: hmsvigle <hmsvigle@gmail.com>
 Date:   Mon Apr 6 13:49:55 2020 +0530

    draft
```
> Note: You cant add an empty file to git index.

```sh
$ mkdir -p example2
$ git status
 On branch master
 nothing to commit, working directory clean
$ echo "example2" > example2/README.md
$ git status
 On branch master
 Untracked files:
  (use "git add <file>..." to include in what will be committed)
	example2/
nothing added to commit but untracked files present (use "git add" to track)
```
  - add new file/directory & the commit

```sh
$ git add -A
$ git status
 On branch master
 Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)
     new file:   example2/README.md
$  git commit -a -m "example2 draft"
 [master xxxx] example2 draft
   1 file changed, 1 insertion(+)
   create mode 100644 example2/README.md
```
  - similarly to commit all files/directories use : '$ git commit -a -m "multiple files committed" '

### 7. Parallel Developement in Git 

 * Git Branches: 
   Git branch is like pointers pointing at changes. Master is the main referral branch in Git.
 	- Local Branch: Only created in local repository
 	- Remote branch: Connect from Local to remote repository

 * Create a branch  
```sh
 $ git branch firstbranch
```
 * Switch to first branch
```sh
 $ git checkout firstbranch
```
 * Directly create & switch branch. It will create the branch if not created.
```sh
 $ git checkout -b firstbranch
```
 * Currently to the branch make necessary changes. Then that can be merged to Master branch.
 * Merging to master branch can be done either through cli or in git ui. Ideally at enterprise label, branch is pushed to the remote repository & requested for merge by adding few reviewers/approvers. After reviwer reviews the changes, approver merges the branch to master. (We will cover this later in detail.) 

### 8. Difference between Pull-Fetch & Rebase-Merge 
 
 - Pull: Pulls all new changes from repo & connects to your master branch. => `'git pull' = 'git fetch' + 'git merge'`
 - Fetch: Fetches all changed files & stores to the current branch in local repo rather than Master branch.
 
 - Rebase:  The branch is merged to the master branch at the head. Rebase makes the history very clean. It is used to   	    cleanup the branches.
 - Merge: The branch is merged to the master as a side branch. `Demontsration will be done afterwards.`

### 9. Merge a branch to Master
```sh
 # before merge operation, switch to master branch
 $ git checkout master
 # perform merge operation
 $ git merge feature/branch0
```
### 10. Push the branch (any branch even master) to the remote repo 
```sh
 $ git push origin master
```

### References:
 [github-help](https://help.github.com/en/enterprise/2.19/user/github/using-git/git-workflows)
 [edureka]()
 [github learningkit](https://github.github.com/training-kit/downloads/github-git-cheat-sheet/)

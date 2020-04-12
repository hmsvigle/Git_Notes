# Git_Notes

> Uninstall/Remove Git from ubuntu machine
```sh
$ sudo apt-get autoremove purge -y git
$ sudo rm -rf ~/.git
$ sudo rm -rf ~/.gitconfig
```
> Check if git is still installed or not !!
```sh
$ which git '# no result'
```
> Install git in Ubuntu machine
```sh
$ sudo apt-get -y update
$ sudo apt-get install -y git
$ git --version '# git version 2.7.4'
```
> configure git parameters
```sh
$ git config --list '# Lists all global configurations for git --> None for now'
$ git config --global user.name "hmsvigle"
$ git config --global user.email "hmsvigle@gmail.com"
$ git config --global core.autocrlf input '# ensures consistent input behavior for all contributers'
$ git config --global color.ui auto
```
> display all config of git
```sh
$ cat ~/.gitconfig 
[user]
	name = hmsvigle
	email = hmsvigle@gmail.com
[core]
	autocrlf = input
[color]
	ui = auto
```
### Notes !!
```sh
  ---/.git/config --> Contains each specific repository related configurations
 ~/.gitconfig --> default git configurations for the user
```

> Create local Repository
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
> Syncing remote repository:
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
> Make changes:
  - git status --> Status of files added/staged/ready to commit
  - git add -A --> Stage required files 
  - git commit -m "msg" -->  commit staged files
  - git status 
  - git push origin
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
> Track all the changes made to git index
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
#### Note: - You cant add an empty file to git index.
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
  - git push '### has to be updated'
  
### Parallel Developement in Git 

#### Git Branches: 
> Git branch is like pointers pointing at changes. Master is the main referral branch in Git.
 - Local Branch: Only created in local repository
 - Remote branch: Connect from Local to remote repository

> create a branch  
```sh
 $ git branch firstbranch
```
> switch to first branch
```sh
 $ git checkout firstbranch
```
> directly create & switch branch. It will create the branch if not created.
```sh
 $ git checkout -b firstbranch
```
 - Currently to the branch make necessary changes. Then that can be merged to Master branch.
 - Merging to master branch can be done either through cli or in git ui. Ideally at enterprise label, branch is pushed to the remote repository & requested for merge by adding few reviewers/approvers. After reviwer reviews the changes, approver merges the branch to master. (We will cover this later in detail.) 
 
 - Pull: Pulls all new changes from repo & connects to your master branch. => `'git pull' = 'git fetch' + 'git merge'`
 - Fetch: Fetches all changed files & stores to the current branch in local repo rather than Master branch.
 
#### Clone remote repository to Local
$ git clone 

#### List the git branches
$ git branch

#### create a new feature branch called 'feature-project-template'
$ git checkout -b feature/project-template

#### switch to the feature branch
$ git checkout  <feature_branch>

#### stage all the files you have changed
$ git add .

#### check the files/directories are staged for commit.
$ git status

#### commit changes to git with an instructive message
$ git commit -m 'Create project template'

#### push changes to remote branch
$ git push origin feature/project-template

#### List all branches
$ git branch -a

#### List Remote branches :
$ git branch -r

#### git view all your settings
$ git config --list

$ git config --global user.name "HPANIGR"
$ git config --global user.email himansu.panigrahy@daimler.com

$ git clean [-f]  --> force Remobve 
$ git clean [-d]  --> remove untracked files
$ git clean [-n]  --> dry-run before actual removal command


%% Exception %% :  
	fatal: 'origin' does not appear to be a git repository
	Cause: This is typically because you have not set the origin alias on your Git repository.
	Resolution : 
		$ git remote add origin URL_TO_YOUR_REPO

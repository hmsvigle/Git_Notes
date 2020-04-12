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
> configure git 
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
  - update data
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

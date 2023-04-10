# Git_Notes

### Git Stages:

![Git Stages](https://user-images.githubusercontent.com/24938159/230939160-7586c846-b801-428d-965d-4ce3a989cf72.png)


1. [Uninstall/Remove Git from ubuntu machine](./README.md#1-uninstallremove-git-from-ubuntu-machine)
2. [Install git in Ubuntu machine](./README.md#2-Install-git-in-Ubuntu-machine)
3. [Configure git parameters](./README.md#3-configure-git-parameters)
4. [Create local Repository](./README.md#4-create-local-repository)
5. [Sync remote repository](./README.md#5-sync-remote-repository)
6. [Make necessary code changes](./README.md#6-make-necessary-code-changes)
7. [Parallel Developement in Git](./README.md#7-parallel-developement-in-git)
8. [Pull-Fetch & Rebase-Merge](./README.md#8-difference-between-pull-fetch--rebase-merge)
9. [Merge Branch to master](./README.md#9-merge-a-branch-to-master)
10. [Git Push](./README.md#10-push-the-branch-any-branch-even-master-to-the-remote-repo)
11. [Git fork](./README.md#11)
12. Fast-Forward Merge
13. `git logs --graph`
  * `git log --graph --decorate --online` --> Better graphical representation
15. Explicicte Merge/Recursive Merge
  * History of branch also gets added to master
17. Squash Merge
  * Clean master history 
18. Resolve git conflict while merging
  *  When we make changes in same file in both the branches, conflict arises.
  *  To resolve the issue, review & keep only one content, that needs to be there.
  *  Then attempt to merge.

19. [Issues](#README.md/Issues-&-Resolutions)

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
# git remote add <Your-Custom_name_of_Repository> <git_rep_url>
$ git remote add origin <git-repo-url>
#
$ git fetch 
#
$ git checkout master
# set upstream
$ git branch set-upstream-to=orgini/master

# Then Pull the Branch 
# git pull <Your-Custom_name_of_Repository> <Branch>
$ git pull origin master
$ ls  --> the remote repository is synced to local
  READEME.md  .git
```
  - check currently configured remote repositories
```sh
$ git remote -v
```
### 6. Make Necessary code changes
  - git status --> Status of files added/staged/ready to commit
  - git add -A --> Stage required files 
  - git commit -m "msg" -->  commit staged files
  - git status   
```sh
# check status of staged files
$ git status
On branch master

# check details of local & remote branches. * will notify the current branch. remotes/=>remote branch, others=>local branch
$ git branch --list -a
  feature/i3-access00
* master
  remotes/expat-calender/master

# check git status
$ git status
On branch master

# Make changes
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
  - Remove the staged file from git. This step can be performed before commiting the changes to remove unwanted added file from staging.
```sh
 $ git rm --chached <filename>
 # if this doesnt work fue to HEAD pointed to diff position, you can always execute force removal.(PS: file/directory)
  error: the following file has staged content different from both the
    file and the HEAD:
       <filename>
(use -f to force removal)
 $ git rm -f --chached <filename>
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
#### Note: similarly to commit all files/directories use : `$ git commit -a -m "multiple files committed" '

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
 * Fetch fresh changes from `Master` branch to `<Current-Branch>`
```sh
 $ git checkout firstbranch
 $ git rebase master
```
 * Make necessary changes to current branch. Then that can be merged to Master branch.
 * Merging to master branch can be done either through cli or in git ui. Ideally at enterprise label, branch is pushed to the remote repository & requested for merge by adding few reviewers/approvers. After reviwer reviews the changes, approver merges the branch to master. (We will cover this later in detail.) 
 * Delete Local Branch
```sh 
 $ git branch -d <branch> 
 $ git branch -D <branch> # Force delete the branch
```
 * Delete remote Branch
```sh
 $ git push origin --delete
```
 * Synchronize your branch list from Remote with below command. The `-p` flag means "prune". After fetching, branches which no longer exist on the remote will be deleted.
```sh
 $ git fetch -p
```

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

### 11. Fork Public repo to private repo & update

To make an exact duplicate, you need to perform both a bare-clone and a mirror-push.

Open up the command line, and type these commands:

```sh

# Clone the public repo.
$ git clone --bare https://github.com/exampleuser/old-repository.git

# Make a bare clone of the repository
$ cd old-repository.git

# Mirror-push to the new repository
$ git push --mirror https://github.com/exampleuser/new-repository.git

# Remove our temporary local repository
$ cd ..
$ rm -rf old-repository.git

```


### Issues & Resolutions:
**1. How to remove staged files from git**
 - Issue: 
 - Description: Files are added to git branch of a repository are called as staged files. Issue arises, if the staged files are not commited & further action is initiated on any other repository/branch.   
 - Resolution:  
    * i. Commit the changes & push the changes to target repository
    ```sh
      $ git commit -m -a "Message"
      $ git push <reponame> master 
    ```
    * ii. Remove the staged files from the staging.
    ```sh
      $ git rm 
    ```
    * iii. Reset the HEAD itself
    ```sh 
       $ git reset HEAD
    ```
      iv. Revert the file back to the state it was in before the changes we can use:
    ```sh 
       $ git checkout -- <file>    
    ```
      v. Remove file/directory from git repo & retry
    ```sh
       $ git rm -r <file>/<directory>
    ```
      vi. Remove a file from the repo but keep it on disk, say we forgot to add it to our .gitignore file then use --cache
    ```sh
       $ git rm <filename> --cache
    ```
**2. Git push is rejected**
  - Issue: " Updates were rejected because the remote contains work that you do"
  ```sh
   ! [rejected]        master -> master (fetch first)
   error: failed to push some refs to <remote repo url>
   hint: Updates were rejected because the remote contains work that you do
   hint: not have locally. This is usually caused by another repository pushing
   hint: to the same ref. You may want to first integrate the remote changes
   hint: (e.g., 'git pull ...') before pushing again.
   hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  ```
  - Description: After we pull the repo, there could be some changed made to repository (possibly through UI/by someoneelse). So the push action is failing.
  - Resolution: The local repo has to be synched with remote repo. Then try to stage changes & push it again.

**3. Fatal Error: `https` protocol not supported**
  - git remote set-url origin https://github.com/Saifou/testForge.git
  - in git bach command, try to paste the url by rightclick/paste, instead of any shortcuts to paste.
 
** 4. CRLF will be replaced by LF in <File>
  - While `git add .` command, above exception is thrown.
  - `Solution:` If you are a single developer working on a windows machine, and you don't care that git automatically replaces LFs to CRLFs, disable the parameter
  ```sh
  git config core.autocrlf true
  ```
### References:
 * [github-help](https://help.github.com/en/enterprise/2.19/user/github/using-git/git-workflows)
 * [edureka]()
 * [github learningkit](https://github.github.com/training-kit/downloads/github-git-cheat-sheet/)
 * [git-scm](https://git-scm.com/book/en/v2/)
 * [gitlab-docs](https://docs.gitlab.com/ee/README.html)
 * [freecodecamp](https://www.freecodecamp.org/news/how-to-delete-a-git-branch-both-locally-and-remotely)

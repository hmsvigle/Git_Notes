# Git_Notes

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

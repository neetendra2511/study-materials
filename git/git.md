# Some useful git things:-

---

---
## Git settings:

* To view all of our settings:-
git config --list
* There are three levels of settings- 
	* local (default): for a specific git repository
	* global (use --global flag): for a specific user
	* system (use --system flag): for all users
	
	*Note*: local dominates global, which dominates system

* To see from where are these settings coming, use git config --list --show-origin

---
## Getting help:

* git help <keyword>
* git <keyword> --help
* man git-<keyword>

---
## Git basics:

* Each file in the repo can be in one of the two states:-
	* *Untracked*:- git doesn't know about the file. (*any files in your working directory that were not in your last
snapshot and are not in your staging area.*)
	* *Tracked:- git knows about them. (*any files that were in last snapshot, as well as newly staged files.)
		* Unmodified
		* Modified
		* Staged

### Status of files: 
* To check the *status* of files run git status
* For *short status* run git status -s

### Ignoring files:
* All the *files to be ignored by git* can be added in .gitignore file:
	* All the patterns are applied recursively throughout the working directory.
	* Regex works
	* start with forward slash (/) to avoid recursivity.
	* end with forward slash (/) to specify directories like node_modules/.
	* !lib.a will track lib.a file.
	
### Have a look at changes:
* To *see changes* you have made but not staged yet, use
	git diff
* To see changes you have staged (that will go in next commit), use 
	git diff --staged
	
### Commit and history
* To *commit* : git commit
* Commit with inline message: git commit -m "message"
* To *skip the staging area*, and committing all the changes:
	git commit -am "message"
	
* To *remove a file*:  git rm <filename>
* Viewing the *commit history*:
	* git log :lists the commits (one page at a time) in reverse chronological order.
	* git log --oneline: shows short commit hashes and messages only .
	* git log --graph: better when there are branches.
	* git log --pretty=short: full and fuller are other options too.
	* git log -n: see past n recent commits only.

* To *see only those commits which modified a particular file/directory*, specify the pathname along with log command as:
	git log --path/to/file

* To *modify the last commit*:
	git commit --amend	(after staging all required changes)

* *Unstaging a staged file*:
	* git reset HEAD <filename>
	* git restore --staged <filename>
	
* *Unmodifying a modified file*(takes the file to the last snapshot, discarding all the changes.):
	* git checkout -- <filename>
	* git restore <filename>
	
### Working with remotes

*Remote repositories are versions of your project that are hosted on the Internet or
network somewhere.*

* See your remotes: -
	* git remote : lists all the remotes' shortname.
	* git remote -v: lists urls along with shornames.

* Adding remote repos: -
	git remote add <shortname> <url>

* To get data from your remote projects, use
	* git fetch <remote>: you need to merge the data.
	* git pull: it merges automatically (if there are no conflicts.)
	
* Pushing to remotes:-
	git push <remote> <branch>: remote and branch are by default set when cloning.
	
* Inspecting a remote:-
	git remote show <remote>

* Renaming a remote:-
	git remote rename <old> <new>

* Removing a remote:-
	git remote remove <remote>
	
### Tagging:

mark some specific points in a repository's history as important, generally release points

* Git supports two types of tags:
	* *lightweight*: *A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific
commit.*
	* *annotated: *They are stored as full objects in the Git database, contain the tagger name, email, and date; have a tagging message, etc.

* To list all the tags: git tag

* To create an annotated tag in Git:
	git tag -a <tagname> -m "message"

* To create a lightweight tag:
	git tag <tagname>

* We can also create tags later like:
	git tag -a <tagname> <commit_hash>

* *Tags are not pushed by default to the remotes*,  
 we can do so by:  
	git push <remote> <tagname> to push a particular tag   
	OR git push <remote> --tags to push all the tags.
	
* To delete a tag, git tag -d <tagname>  
	and push these changes to remote by: git push <remote> --delete <tagname>
	
* Checking out tags: git checkout <tagname>

### Git Aliases:

to make git experience easier, and more familiar. 
  
* Can create as: git config --global alias.st status

---

---

## Git Branching

A branch in Git is simply a lightweight movable pointer to one of the commits.

*HEAD*: a pointer to the local branch you’re currently on.

* To get a list of the branches:   
	`git branch`  
* To create a new branch:   
	git branch <branch_name>  this doesn't switches to the newly created branch.

* To switch to an existing branch:    
	git checkout <branch_name>     
	OR    
	git switch <branch_name>
	
* Create a new branch and switch to that branch:   
	git checkout -b <newBranchName>   
	OR    
	git switch -c <newBranchName>  
	
* To merge current branch with another branch `<branch_name>`:   
	`git merge <branch_name>`   
	
* To delete a branch:  
	`git branch -d <branch_name>` 
	
* To get a list of last commits on each branch:   
	git branch -v  

* To see which branches are already merged in a branch:   
	`git branch --merged <branch_name>` (if <branch_name is not provided, current branch is assumed)     

* To see which branches are not merged in a branch:  
	`git branch --no-merged <branch_name>`  (if <branch_name is not provided, current branch is assumed)     

* *Renaming a branch*:  
	`git branch --move old-name new-branch-name`     
	and push it to the remote    
	`git push --set-upstream <remote> new-branch-name` 
	and we need to delete the old-name branch from the remote as :     
	`git push <remote> --delete old-name`  
* To change the default remote name (i.e. origin) while cloning, use:     
	`git clone -o <new-name> <remote-url>`   
	
* To name a branch differently on remote while pushing it:   
	`git push <remote> branch:new-name`     
* If you already have a local branch and want to set it to a remote branch you just pulled down, or
want to change the upstream branch you’re tracking:    
	`git branch -u <remote>/<branch>`   
	OR    
	`git branch --set-upstream-to <remote>/<branch>`   

* To see all branches and the upstreams they are tracking:   
	`git branch -vv`     
	
*  To *rebase* current branch on another (suppose experiment on master), use:  
	`git checkout experiment`     
	`git rebase master`    

* `git rebase --onto main topic subtopic`   
	topic is diverged from main, and subtopic has diverged from topic and    
	we want to rebase subtopic onto main while holding off the topic branch changes..
	
* **Do not rebase commits that exist outside your repository and that people may have based
work on.**

* *When you rebase stuff, you’re abandoning existing commits and creating new ones that are similar
but different.*

### Stashing:

Suppose we have done half-work in one branch and now want to work on another branch but don't want to commit half-work, then we can stash our changes to make working directory clean and then switch branches safely

* To stash the changes:   
    `git stash`   
    OR       
    `git stash push`

*Note:* The above commands do not stash the untracked files, to stash them too, use:   
 `git stash -u`

* To see a list of all stashes:  
    `git stash list`  

* To apply any stash to current branch:   
    `git stash apply stash@{n}`   
 :n is a number.

* To apply the most recent stash:   
    `git stash apply` 

*Note: *above two commands do not pop the stash from the stack   

* To drop the stash from the stack, use:  
    `git stash drop`    

* To apply and drop the stash simultaneously use:    
    `git stash pop`    

#### Cleaning your Working Directory :
* Remove all files that are not tracked:   
    `git clean` 

* To remove every changes and save it on a stash:   
    `git stash --all`
---


### Reset
* Move the branch HEAD points to (stop here if `--soft`).
2. Make the index look like HEAD (stop here unless `--hard`).
3. Make the working directory look like the index.
---

### Advanced Merging

#### Types of merges

---
## Miscallaneous:

* To *search* for a particular term, see `git grep` command and `git log` command and various options available with them.

* To change the last commit, we can use `git commit --amend`
* To change multiple previous commits, use `git rebase -i HEAD~n`   
n is a number indicating that the last n commits will be modified.
* We can also squash, split, reorder, delete commits via `git rebase` :   
    [See Here](https://git-rebase.io/)

* *Removing* a file from every commit:(suppose the file is passwords.txt)   
    `git filter-branch --tree-filter 'rm -f passwords.txt' HEAD`         

* `git revert -m 1 HEAD` 
*
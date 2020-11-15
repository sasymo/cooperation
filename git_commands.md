Week 1
--------
### Installation and Get started
* git --version : Verify that Git is installed git : Overall Git help at command line 
* git help <command> : for example : git help commit 
* git config user.name : View the current setting of username 
* git config --global user.name "Your name" : Set your user name git config user.email : View the current setting of email 
* git config --global user.email "yourID@email.com" : Set your email address

### Initialize a Git repository
* mkdir repos : Create a directory called "repos"
* cd repos : Change directory to "repos"
* git init : Initialize a repository  

### Commit to Local Repository
* git status : View file status, the newly created files are 'untracked' 
* git add : Adds the file to the staged area
* git commit -m "A message comes here" : Add the staged file to the local repo
* git log : Shows the commit details of the project
* git log --oneline : Concise version of the history

### Steps for Cloning a remote repo
* git clone <https address of the repository in Github or Bitbucket> : Clone a remote repository
* ls -a : Shows all the directories including hidden ones in the current directory
* git remote -v : Shows the URL of the remote repository 

### Pushing the changes of local repo to the remote repo:
* git push -u origin master : Origin is the name of the remote repository, master is the default branch that we want to push 

Week 2 
----------
### Show details of commit objects
* git log --oneline : Shows the concise list of commit objects(can be used in the next command)
* git show <SHA-1(hash number of a commit)> : Shows the details of the specified commit
* git branch --list : Shows the list of branches in the project

### Manage tags
tag(definition):Git has the ability to tag specific points in a repository's history as being important.
Typically, people use this functionality to mark release points(v1.0 , v2.0 and so on)
* git tag : Shows the tags of the project
* git tag -a -m "your message here" <name of the tag> : (-a for annotated tag , -m : message )
* git show <name of the tag> : Shows the tag information & its associated commit(most recent commit on the HEAD)
* git push origin <name of the tag> : Pushes the mentioned tag to the remote repository
* git tag -a -m "message comes here" <name of the tag> HAED~ : creates a tag & associate it with the parent commit of the recent commit on the HEAD
* git tag -d <name of the tag> : Delete a tag on the repo

### Branch
* git branch <branch1> : Creates a branch called branch1
                                                                        == git checkout -b <branch1> : this command executes the two previous command at once 
* git checkout <branch1> : Checkouts the aformentioned branch(branch)
* git log --oneline : HEAD -> branch1 : this shows that the branch1 is currently checkedout and active(any new commit will be on this branch)
* git branch : Shows the existing branches in a project and adds a * to the left of the checked out branch name
* git branch -d <branch name>: delete a branch(When changes have been merged)
* git branch -D <branch name>: Forces deleting the branch although the changes have not been merged
   **Undo deleting a branch:**
* git reflog : Shows the local history of HEAD references -->> find & copy the SHA-1 of the most recent deleted branch name 
* git checkout -b <branch name> [SHA-1 YOU COPIED] : Returns the deleted branch -->> Check the git log to make sure the branch is back


### Tracking Branches
If we create a remote repository with at least one commit, the tracking branch is automatically set up. 
* git branch --all : Shows the local and tracking branch names
* git log --all --oneline --graph : Shows the full log of all local & tracking branches


### Manage merge confllict
* git merge <branch1> : Merges the branch1 with the current checkedout branch of the project. If the two branches had changed the same hunck of the same file, a conflict will appear.
When a conflict occurs : One possible approach is to abort: 
* git merge --abort : Abort the merge process
The other approach involves editing the file containing conflict and the add, commit so the merge process can be completed.


### Fetch, Pull and Push
* git fetch : Retrieves new objects & refrences to the remote repository 
* git pull : Fetches and merges commits locally , if no new commit has been made to the local repository, a fast forward merge takes place, otherwise at first a commit for the merge is created and then the pull gets executed
* git push : Adds new objects and references to the remote repository(To be on the safe side, first perform pull and then push)

### Rebase (rebasing is a form of merge therefore a conflict can happen)
Move commits to a new parent(base)
 - The unique commits of the featureX branch (B and C) are reapplied to the tip of the master branch(commit D)
 - Because the ancestor chain is different, each of the reapplied commits has a different commit ID(B' and C')

Do not rewrite history that has been shared with others

The are 2 types of rebase:
 -Rebase
 -Interactive rebase

**Regular rebase**
* git checkout <featureX branch>: Acivates(checks out) featureX branch
* git rebase master : Rebases the commits on the featureX branch on the tip of master branch(Upstream:usually refers the parent branch of the rebased branch)


**Rebase with resolving a merge conflict**
(Two branches(master & featureX) changed the same part of one file(fileB.txt))

	checkout <featureX branch> : 
	git rebase master : Merge conflicts appear in this step
	git status : View the status of the rebase ; resolve the conflict on the file(change the content of the file(fileB.txt))
	git add <fileB.txt> : Stage the fixed file(fileB.txt)
	git rebase --continue : Continues the rebase process
	git log --oneline --graph : Should show a linear git graph after the rebase 

**Rebase containing conflict and we decide to cancel the rebase**
	checkout <featureX branch>
	git rebase master(Here we might face a conflict & want to cancel)
	git rebase --abort

**Comparison between merge and rebase in steps**

| Merge                      | Rebase                         |  
| -----------------------    | ------------------------------ |
| 1. git checkout master     | 1. git checkout featureX       |
| 2. git merge featureX      | 2. git rebase master           |
| a. CONFLICT                | a. CONFLICT                    |
| 3. git status              | 3. git status                  |
| a. Both modified fileB.txt | a. Both modified fileB.txt     |
| 4. Fix fileB.txt           | 4. Fix fileB.txt               |
| 5. git add fileB.txt       | 5. git add fileB.txt           |
| 6. git commit              | 6. git rebase --continue       |   
                             


**Amend a commit**
One can change the most recent commit
	* change the commit message
	* change the project files
The amend creates a new SHA-1(rewrite the history)
Let's imagine the fileA.txt which we added and commited contains an error and we want to rewrite the history. The procedure is as follows:
	* Modify fileA.txt
	* Add fileA.txt to the staging area
	* Amend the previous commit by executing ' git commit --amend -m "add fileA.txt" '
	* In case you want to keep the old commit message, the command is : 'git commit --amend --no-edit 
If you execute 'git log --oneline ', you will realize that the SHA-1 of the amended commit is different from the previous one.
Let's say only the commit message of the last commit has a typo, in this case the amend procedure is as follows:
	* git commit --amend -m "Corrected commit message"



**Interactive rebase**
During interactive rebase you can edit commits using commands
	* The commits can belong to any branch 
	* The commit history is changed(Vorsicht: Do NOT use for shared commits)

**Interactive rebase options**(to change a branch in many ways)
	* Use the commit as is
	* Edit the commit message
	* Stop and edit the commit
	* Drop/delete the commit
	* Squash
	* Fixup	
	* Reorder commits
	* Execute shell commands
	

1. checkout <branch_name>
2. git rebase -i <SHA1-commit> : i stands for interactive and the children of the mentioned commit can be accessed within interactive rebase 
3.

### Squash 
Squash a commit from history using interactive rebase:[squash in dictionary = to press sth into a flat or flatter shape]
	* checkout the "featureX" branch(this is the branch where the commit to be deleted is located)
	* view the commit graph of the "featureX" branch
	* git rebase -i <SHA-1>
	* Explore the interactive rebase editor
	* Remove the commit by replacing the "pick" with "squash". Save the file
	* An editor opens for the commit message of the squash commit(change it if you like)
	* View the commit graph with "git log --oneline" : the squashed commit is removed 

**Perform a squash merge**
	1. Create a "featureX" branch (git branch featureX)
	2. Create a commit on the featureX branch (commit A)
	3. Create another commit on the featureX branch (commit B)
	4. Perform a squash merge of featureX branch into master branch
		* checkout the master branch : (git checkout master)
		* git merge --squash featureX
		* git commit : in the opened editor specify the commit message
	5. View your commit graph : git graph --oneline (The message of commit A will not be part of the master branch)
	6. Delete the featureX branch label:(git branch -d feaureX)
	
### Pull Request 1 



### Git Workflow







*summarized version of "Version Control with Git by Atlassian"*





















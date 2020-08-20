# Git

#### Content:
+ [Create a Repository](#createRepository)
+ [Observe your Repository](#observeRepository)
+ [Working with Branches](#workingBranches)
+ [Make a change](#makeChange)
+ [Synchronize](#synchronize)


<a name="createRepository"><h2>Create a Repository</h2></a>

+ Create a new local repository -- `git init [project name]`;
+ Download from an existing repository -- `git clone [link]`;

<a name="observeRepository"><h2>Observe your Repository</h2></a>
+ List new or modified files not yet committed -- `git status`;
+ Show the changes to files not yet staged -- `git diff`;
+ Show full change history -- `git log`;

<a name="workingBranches"><h2>Working with Branches</h2></a>
+ List all local branches -- `git branch`;
+ Switch to a branch, my_branch, and update working directory -- `gsait checkout my_branch`;
+ Create a new branch called new_branch -- `git branch -d my_branch`;
+ Delete the branch called my_branch -- `git branch -d my_branch`;
+ Merge branch_a into branch_b :
```
git checkout branch_b
git merge branch_a
```

<a name="makeChange"><h2>Make a change</h2></a>

+ Stages the file, ready for commit -- `git add [file]`;
+ Stage all changed files, ready for commit -- `git add`;
+ Commit all staged files to versioned history -- `git commit -m “commit message”`;
+ Commit all your tracked files to versioned history --`git commit -am “commit message”`;
+ Unstages file, keeping the file changes -- `git reset [file]`;
+ Revert everything to the last commit -- `git reset --hard`;


<a name="synchronize"><h2>Synchronize</h2></a>

+ Get the latest changes from origin (no merge) -- `git fetch`;
+ Fetch the latest changes from origin and merge -- `git pull`;
+ Push local changes to the origin -- `git push`;
+ Removing a commit from a git :

```
git reset --hard HEAD^
git push --force
```
+ Merging two project-a repositories with project-b:

```
cd path/to/project-b
git remote add project-a path/to/project-a
git fetch project-a --tags
git merge --allow-unrelated-histories project-a/master # or whichever branch you want to merge
git remote remove project-a
```

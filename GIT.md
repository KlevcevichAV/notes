# Git

+ Download from an existing repository -- `git clone [link]`;
+ Create a new local repository -- `git init [project name]`;
+ List new or modified files not yet committed -- `git status`;
+ Stages the file, ready for commit -- `git add [file]`;
+ `git commit` -- берёт все данные, добавленные в индекс и сохраняет их слепок во внутренней базе данных, а затем сдвигает указатель текущей ветки на этот слепок.
+ Unstages file, keeping the file changes -- `git reset [file]`;
+ Push local changes to the origin -- `git push`;
+ Fetch the latest changes from origin and merge -- `git pull`;
+ You stage a file for removal, but you don't remove it from the working dir. The file will then be shown as untracked. -- `git rm [file]`;
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

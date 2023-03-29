# Git

## Overview

Git is a free and open source distributed version control system.<br>
git uses branches to manage the versions of the files and allows to easily change between them and update them, modify them, o simply take a look at them without changing anything. another aspect is git index and commits which represent changes made to files over time and saved as snapshots that can be checked. <br>
<br>
One way to look at it is to think of branches as alternative universes of the code, and commits as points in time. so if you have only one branch but multiple commits means that you have made changes over time, but if you also have multiple branches there maybe multiple versions of your code that you can swtch between.<br>
<br>
You can switch between these different version with the checkout command which moves something called Head, which is like a pointer which show you the version of the code its pointing at.<br>
git also allows to revert any changes made and go back to an earlier version f the files to a specific commit.

### Tree system

To understand git better its necessary understand the three structures that git uses to keep track of files. this concept will help to understand what happens at navigating through older commits etc.<br>
Git structure has 3 components or "three trees" as they are called;  one keeps track of the changes on disk of the files, these are the versions of the code you can switch between, the second keeps track of the stages of the files, when you use add git creates a ref and a hash etc, this is the index. and the third is the commit tree using the Head, which keeps track of the commits done and points to the current checked out commit.<br>
[more info on the three trees](https://code.tutsplus.com/tutorials/what-are-the-three-trees-in-git--cms-28188)

### Basic behavior

We create points in time or snapshots with commits. first we add the files to the index tree which creates a hash of the changes to each file, then we create said snapshot using commit which can be checked on moving the head.<br>
<br>
Usually we want to keep checking which files have been modified. To check which files have been modified since the last commit we use status. Red files show modified files whose changes have not been added.<br>
<br>
commits are states in time of your code. like timestamps, you can navigate them which we will do in more detail below.<br>

## Commands

### Starting a new repo

To start a repository we just have to open a terminal on the folder of our project and enter the init command. this will create all the necessary files to keep track of changes etc. some common commands:

| Command      | Description |
| :---        |    ---:   |
|```git init```| initialize repository |
|```git status```| check modified files |
|```git diff <file>```| check file differences |
|```git add <file>```| add files |
|```git commit -m "<comment>"```| commit to local |
|```git push origin <remote_branch>```| push to remote |
|```git checkout <hash>```| move head |
|```git restore <file> (old versions >git checkout -- <file>)```| discard file changes |
|```git remote update origin --prune```| update remote list |

### Branch Manipulation

git allows us to create and move between branches which are different versions of our code. these branches can be forked from any other branch, creating a copy that can be modified, and even mixing them back together if wanted.

| Command      | Description |
| :---        |    ---:   |
|```git branch -a```| list all branches |
|```git branch -r```| list remote branches |
|```git branch <branch>```| create local branch |
|```git branch -d <branch>```| delete local branch |
|```git push origin --delete <remote_branch>```| delete remote branch |
|```git checkout -b <branch>```| To create a branch and switch to it. (you will checkout this branch and keep modifications on files) |

NOTE: branch creation tip
If you have changes and you are not sure to add them to your current branch. you can use git checkout -b <branch> to create a branch and check it, then you can commit. with this you can create a new branch to save the changes and the previous branch will be left untouched and later you can decide if merging or not.

### Remotes and clonning

A big advantage of git its that allows to work locally and remotely. the remote repository could be on github, bitbucket, gitlab, or even a personal server, git allows us to synchronize our local repositories with the remotes we want.

### Remote branches.

| Command      | Description |
| :---        |    ---:   |
|```git push origin <remote_branch>```| to push commits to remote |
|```git pull origin remote <remote_branch>```| to pull commits into local |
|```git push origin <remote_branch>```| create remote branch |
|```git remote rename <remote_branch> <new_remote_branch>```| rename a branch |

### Remote origins.

| Command      | Description |
| :---        |    ---:   |
|```git remote -v```| check remote origins |
|```git remote add <name> <url>```| add remote |
|```git remote set-url <name> <new_url>```| set new remote |
|```git remote rename <name> <new_name>```| rename remote |
|```git remote rm <name>```| remove remote |
|```git remote update origin --prune```| update remote branch list |
|```git checkout --track -b <branch> origin/<remote_branch>```| bring remote branch, create local and check it |
|```git clone <remote>```| clone remote |
|```git clone --single-branch --branch <branch> <remote>```| Clone single branch |

### File differences

Since git keeps track of all the changes we made to the files, specially code, it can be used to check the changes made to the modified files. lines added, lines removed, changes between files, between the same file in different commits, etc.

| Command      | Description |
| :---        |    ---:   |
|```git diff <file>```| differences from last commit |
|```git diff <commit1> <commit2> -- <file>```| differences on a file from different commits |
|```git diff HEAD~N HEAD -- <file>```| differences from current head to N commits back |
|```git diff <revision_1>:<file_1> <revision_2>:<file_2>```| differences on different files from different commits |
|```git diff <branch1>..<branch2>```| differences on different branches |

### Stashing

Its sometimes useful to save changes temporarily. if we were working on something but we are not sure we should commit it, or maybe we dont want to commit it, but we want to keep these changes around for a bit. this allows to perform git operations without having to commit and without having to discard changes.

| Command      | Description |
| :---        |    ---:   |
|```git stash```| save stash |
|```git stash -p <file>```| save stash specific files |
|```git stash pop```| get stash changes and remove from stash list |
|```git stash pop stash@{n}```| get specific stash changes and remove stash from stash list |
|```git stash apply stash@{n}```| get specific stash changes but keep on stash list |
|```git stash save "<stash name>"```| save stash and name it |
|```git stash list```| list stashes |
|```git stash drop stash@{n}```| remove stash from stash list |

### Joining Changes

#### Merge, Rebase and Cherry-Pick
Merge is a command that allows us to combine the changes of a different branch into the current branch. (the name might cause a confusion. merge is to bring the differences of another branch into the currently checked one). if you want to bring the changes made to branch2 into branch1 you have to checkout branch1 and then use gir merge branch2.<br>
<br>
Another important and similar command is Rebase. It's similar to merge in the sense that is used to join branches, however, rebase alters the git history, (TODO)
#### Push/Pull
The way push and pull work is similar to merge but between a local and a remote branch. When you use pull you are bringing the changes from a different repository to the local (like merge but remotely). Push on the other hand forces the changes from your local repository into the target repository. Push is the only of these 3 commands that works outwards.<br>
<br>
Lastly, pull request means that you are requesting the owner of a different repository to please pull the changes from your repository into their repository. it differs from a push because you are not simply forcing your changes on a remote, you are making a request (and the other party may accept or deny) for a pull command, hence, pull request.

| Command      | Description |
| :---        |    ---:   |
|```git merge <branch>```| merge |
|```git merge squash <branch>```| merge squash (into a single commit) |
|```git merge --abort```| Undoes the merge (aborts the merge. in case there are conflicts) |
|```git cherry-pick <commit>```| cherrypick, to pick a specific commit |
|```git rebase <branch>```| Rebase |
|```git pull <branch> <remote>/<branch>```| pull |
|```git push <branch> <remote>/<branch>```| push |

### Commit Navigation

git allows us to navigate between points in time (commits or hashes) using different forms and perform different options, be it just to take a peak at the code at that point without modifying anything or to go back to restore a previous state. <br>
The way git works there is a tree with nodes which are the commits, and the state of the repository is where the head (basically a pointer) is. so you can navigate old versions moving the head and go back to the last commit etc.<br>

#### checkout vs reset vs revert
An important detail is that git uses a pointer, the Head and a main ref to know how to navigate versions. when you use git checkout, the head moves but the main ref is still on the last commit, so you can change versions of your code. but if you want to undo a commit, you have to use reset which moves both, head and main. however, reset undoes the history so its a bit dangerous to use. if you want to go back some commits but keep the reversion itself on the history use revert which creates a new commit that undoes the changes from a previous commit.<br>

####  Checkout
using the command checkout we can navigate on different branches and commits in a secure way. if you go to an older commit using checkout what happens is you detach the head from the main but you cannot make changes.

| Command      | Description |
| :---        |    ---:   |
|```git log --decorate```| show commit history |
|```git checkout <hash>```| check specific commit |
|```git checkout <branch>```| check latest commit on a branch |

#### Reset
This command is a little complex to understand. we have to keep in mind the three trees system that git uses. this command can move these trees depending on the flag used with it.
soft vs hard vs mixed
```
soft moves only the commit tree. so the files are still modified, and still staged but no commit has been made, so you can un-stage and unmake the changes with git remove and git checkout filemixed moves the commit tree and the stage tree, so files are still modified but changes are not staged, and can be removed using git checkout filehard moves the three trees, so it goes back to how files were a commit before (or the number of commits reseted)
```

| Command      | Description |
| :---        |    ---:   |
|```git reset --soft HEAD~n```| go back n commits keeping changes on disk and added |
|```git reset HEAD~n```| go back n commits keeping changes on disk |
|```git reset --hard HEAD~n```| go back n commits without keeping changes |

another way to move is using reflog. reflog is the complete history of operations on git, adn can be used to move to a certain point after using a git command etc.

| Command      | Description |
| :---        |    ---:   |
|```git reflog```| check head movement history |
|```git reset --soft HEAD@{1}```| move head to a certain point |

#### Revert
revert is similar to reset as it is used to go back to a previous commit but this command creates a new commit that undoes the changes from a previous commit. This command adds new history to the project (it doesn't modify existing history like reset).

| Command      | Description |
| :---        |    ---:   |
|```git revert <commit-hash>```| revert |

NOTE: git revert works very differently to reset. this command reverts individual commits, so when using git reset HEAD~2 undoes everything from hash HEAD and HEAD~1 until it reaches HEAD~2, git revert HEAD~2 only undoes that specific hash. <br>
<br>
If you want to undo multiple commits you have to specify the order to undo them like this: <oldest-to-undo-hash>^..<newest-hash> and git will undo each commit from newest to oldest in order and create a commit for each undo to the history. As an alternative you can add the flag --no-commit for reverting changes but not commenting, stacking multiple reverts and just one commit at the end.<br>

#### Example Reset vs Revert

If we have 5 commits oldest to newest:
```
A -> B -> C -> D -> E (HEAD)
```
And we want to go back 3 commits (undo E,D,C and go back to B) <br>
using reset:
```sh
git reset --soft/hard/mixed HEAD~3
```
using revert
```sh
git revert C^..E
Alternatively using HEAD
git revert HEAD~2^..HEAD
```
Notice that when using reset we are specifying where to move the head HEAD~3 which is B and undoes everything until reaches that point, whereas revert we specify which commits to undo so we say undo HEAD HEAD~1 and HEAD~2

### Worktrees

worktrees are a way to checkout multiple branches at the same time. its really handy if you want to update for example one base branch and quickly update another branch with merge and not having to commit then checkout etc.<br>
It's also very useful if you want to run the application for bot branches at the same time, to compare etc, or if you need to work with multiple branches regularly.<br>
The way it works it creates a new folder where the checkout branch as the branch you used to create the folder. and to checkout said branch just navigate to said folder.<br>
Example, i have a repo with 3 branches, main, main-company1, main-company2. for 3 versions of the same code. i can work with the three at the same time.<br>

| Command      | Description |
| :---        |    ---:   |
|```git worktree add <path-dir> <branch>```| Create worktree |
|```git worktree remove <path-dir>```| To remove a worktree (Obviously doesn't destroy the branch) |

Pull Request

Diagrams

This long-ass command lets you view commit histories against each other
```sh
git log --graph --abbrev-commit --decorate --oneline <branch1> <branch2>
```

For ease, create and alias
```sh
alias gt="git log --graph --abbrev-commit --decorate --oneline"
```

add that line to ~/.bashrc and now we can just use gt
```sh
gt <branch1> <branch2>
```
#### Other Advanced Commands

| Command      | Description |
| :---        |    ---:   |
|```git merge-base <branch1> <branch2>```| Find last commit where two branched diverged |

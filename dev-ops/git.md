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

#### Starting a new repo

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
---
layout: post
title:  "Git - useful commands"
date:   2016-04-07 20:29:53
categories: git 
---

This is a list of git commands that I frequently use and need to stackoverflow them from time to time. So, recording them here so that I dont need to look them up at all times like now.

I work on windows, but use git bash for everything to do with git.

### Clone a remote repository

__*To clone a remote repository*__

```
$ git clone <repository_url> <directory>
```

To clone the libgit2sharp repo from github 

```
$ git clone https://github.com/libgit2/libgit2sharp.git
```

This will - 

- Create a new folder with the name of the repo
- Clone the repository with creating remote tracking branches for each branch in the remote repository
- Checks out the currently active branch in the remote repository.

__*To clone into an existing folder*__

```
$ git clone <repository_url> <directory>
```

To clone libgit2sharp into gitsharp

```
$ git clone https://github.com/libgit2/libgit2sharp.git gitsharp
```

__*To checkout a specific branch after clone*__
All the clone commands above will checkout the active branch in the remote repository. In order to checkout a specific remote branch

```
$ git clone <repository> -b <branch>
```

The following command clones the repo into an existing folder gitsharp and checks out the vNext branch

```
$ git clone -b vNext https://github.com/libgit2/libgit2sharp.git gitsharp
```

### Search for a branch
I often need to search for a branch by name, there may be nice native git command that I do not know of, but I use plain ol' grep

```
$ git branch --all | grep -i vnext
```

### Checkout a remote branch creating a tracking branch
There are a couple of terms here that would be good to clarify.

**Branch** - _A label that we assign to the commit which is at the top of a reachable series of commits._

**Local branch** - _A branch that exists in a local repository._
   
**Remote branch** - _A branch that exists from a remote repository. Note that remote branches are addressed as [remote repo]/[branch name]_ 
   
**Tracking branch** - _If we want to work on a remote branch, we need a local version that 'tracks' the remote branch. This is a tracking branch._

So, lets see what remote branches are available

``` 
$ git branch --remote
```

To create a tracking branch, checkout and switch to the branch
```
$ git checkout --track <remote>\<branch>
```

e.g.
```
$ git checkout --track origin/pointers
```

This command will fail if the branch already exists.

```
$ git checkout --track origin/master
```

    fatal: A branch named 'master' already exists.


to reset or create a new branch use the -B option during checkout

```
$ git checkout -B <new branch> <remote>\<branch>
```

for e.g. 

```
$ git checkout -B master origin/master
```

### Reset local changes
When you want to discard all the local changes

```
$ git reset 
```

this will reset the index i.e. remove all the files from the index, but will leave the working directory untouched. Specify '--hard' to reset the working directory as well

```
$ git reset --hard
```

### Clean working directory
When you want to reset your working area, you might want to remove all untracked files, these are all the files that git does not know about yet, these include local changes not yet staged and all .git ignored files

```
$ git clean --force
```

### Lastly...
In this post I have ignored the more often used commands like ```git add``` and ```git commit``` and also some of the more more esoteric ones like ```git cat-file```. The intention is mostly to record the ones that I need regularly, but not often enough for them to be embedded in my muscle memory. 
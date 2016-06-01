# Git Deleting, Unstaging, and Ignoring Files

## Overview

Continuing on in the Simon Stamp Saga... In this lesson we will learn how to delete files from both local and remote repositories, as wella s how to unstage files, and ignore files wish to automatically prevent from being staged. 

## Objectives

1. Delete files and folders on both local repo and remote.
2. Unstage files and folders.
3. Ignore files and folders.

## Ready, Set, Delete!

Watch the video below if you are unfamiliar with Git. We will be using Git to access course materials and to share and collaborate on project code throughout this course. After watching the video you may use the text below to review all of the topics discussed in the video.

<iframe width="853" height="480" src="https://www.youtube.com/embed/WZ0cMLe8hcQ?list=PLj148bJp5wizT5Eo8uxHBieh--t_4wOtD" frameborder="0" allowfullscreen></iframe>

### Deleting

In order to delete files and folders from the local repository and have that deletion propagated to the remote as well, we need to include some different flags for our staging and commiting steps. Let's start out by creating a new file to use as an example for deletion. Back in our simon-stamp-collection folder in Terminal, type: `touch index.html` to create a new index file. Then head back into your code editor and open the index file and type "Hello World."  and save. Then back in Terminal, lets stage this file `git add index.html` and press return. Then commit `git commit -m "add index file"` and press return. Then push `git push origin master` and press return. Then head to our simon-stamp-collection repository on Github.com in our browser and refresh the page. Here you can see out new index file is now included within our file list. Now let's assume that we changed our mind. We no longer wish to have an index file. Thus we would like to delete it not only from our local repo, but also from our remote as well. Back in Terminal, we will start by removing the file locally using the rm command `rm index.html` and press return. Now if we type `ls` and press return. We can see that the index file has been removed. So far so good. Now if we type `git status` and press return we receive the following response:

```shell
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

We can see git is aware the index file has been deleted as indicated on line 6. Normally when you you stage and commit files it only includes new files and modified files. In our case we need to also make git aware to include this deletion as part ofg the next commit. That way when we push this commit up to our remote, it will be aware of the deletion and thus remove the file from the remote repo. Git gives us a hint on line 8 where it suggests "git commit -a". The `-a` flag stands for "all" meaning all changes including deletions. We can combine this with our previously used commit flag `-m` for leaving a message in the commit. These become combined as `-am`. Ready? Now we type: `git commit -am "remove index file"` and press return. Then push up our commit `git push origin master` and press return.

Back in the browser in our simon-stamp-collection repo on Github.com we can refresh the page and see that indeed the index file has been removed from the remote. So the key to deleting files locally and having that deletion also propigate to the remote we simply included the `-a` flag to our commit.

### Unstaging

You will recall we have been using `git add` to stage files. What if you accidentally stage something you did not wish to be included in an upcoming commit? Let's create a new file for this example. Back in terminal type: `touch private-keys.md` and press return. Next type `git status` and press return. This will display that there is one new untracked file:

```shell
On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    private-keys.md

nothing added to commit but untracked files present (use "git add" to track)
```

This file has sensitive data that we do not want to exist on our public Github repo where everyone can see it. It is easy enough just not to call the add command on it, but what if I acidentally do add it. Perhaps by typing `git add .` meaning add the entire folder staging all changes. Or perhaps it just slipped my mind and we typed in the file name directly. Go ahead and stage the file `git add private-keys.md` and press return. now when I type `git status` it responds:

```shell
On branch master
Your branch is up-to-date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   private-keys.md

```

Oops, we appear to have accidentally staged this file. Not to fear we can easily unstage files using the reset command. We previously used reset to revert to a previous commit by giving the reset command the SHA key for a matching commit. Well, we can also unstage files by calling reset on a particular filename as well. In Terminal type: `git reset private-keys.md` and press return. Now type `git status` and press return. It will display the following response:

```shell
On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    private-keys.md

nothing added to commit but untracked files present (use "git add" to track)
```

Now it is back to normal. We have sucessfully unstaged (untracked) our private-keys.md file. This means it will not be included in our next commit.

### Ignoring

Sometimes there are certain files or file types that we never want to track changes one. To protect ourself from ever allowing these files to become staged we can include them in a special file within our repository called **.gitignore**. Let's permanently protect our private-keys.md file by adding it into our ignore file. To start back in Terminal type `touch .gitignore` and press return. Then let's bring this file up in your code editor. Then paste in this typical git ignore file content and also add our private-keys filename at the very bottom of this ignore file:

```txt
# Compiled source #
###################
*.com
*.class
*.dll
*.exe
*.o
*.so

# Packages #
############
# it's better to unpack these files and commit the raw source
# git has its own built in compression methods
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip

# Logs and databases #
######################
*.log
*.sql
*.sqlite

# OS generated files #
######################
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

private-keys.md
```

So you can see its easy to ignore certain files and file types by simply typing them directly into this ignore file. The lines that start with # are comments and wont be ignored, they are just there to leave ourself notes about the content. We simply type `*.log` to ignore any filename that ends in the extension .log; here the `*` star is a wildcard. To ignore a particular file like `private-keys.md` we simply typed the filename in. To ignore an entire folder you can also just type in the foldername. 

Now let's save and exit and head back to Terminal and let's see if this works. Type `git add .` and press return to stage all changes in this folder. Then type `git status` and press return. It will respond:

```shell
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working directory clean
```

This means that there is nothing that was staged or needs staged. It is now ignoring the private-keys.md file.

### Global Gitignore

It is worth noting that if you can globally across all repos ignore specific files by including them in a .gitignore file in your user directory at: `~/.gitignore`. This file should have been already added to your user folder during the environment setup for this course. I thought I'd mention though just for your knowledge. See the link below in the resources to learn more about Global Gitignore files.

## Summary

- To incldue a deletion in our commit add the "m" flag `git commit -am "deleted something"`.
- To unstage a file type `git reset <filename>`.
- Files and folders can be ignored by typing their filename/foldername into a .gitignore file.
- A global .gitignore file can be used to ignore certain files across all repos automatically.

## Resources

- [Git Docs - Add](https://git-scm.com/docs/git-add)
- [Git Docs - Reset](https://git-scm.com/docs/git-reset)
- [Git Docs - Ignoring Files](https://help.github.com/articles/ignoring-files/)
- [Git Ignore Example File](https://gist.github.com/octocat/9257657)
- [Git Cheatsheet](https://www.git-tower.com/blog/content/posts/54-git-cheat-sheet/git-cheat-sheet-large01.png)
- [Git Best Practices](https://www.git-tower.com/blog/content/posts/54-git-cheat-sheet/git-cheat-sheet-large02.png)

<p class='util--hide'>View <a href='https://learn.co/lessons/html-css-git-delete-unstage-ignore'>HTML CSS Git: Delete, Unstage, Ignore</a> on Learn.co and start learning to code for free.</p>

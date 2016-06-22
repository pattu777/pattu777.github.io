---
layout: post
title: Useful Git commands
comments: true
---

So I have been using Git for a while now. Considering it's the only version control system I know, I use it for all my projects. But I realize most of my knowledge of Git revolves around just a couple of commands.

* add
* commit
* push
* pull
* A little bit of branching.

So in this post I will show some useful but less frequently used Git commands that I have stumbled upon in the past.

# Diff between a remote and a local file.[^1]
Let's take a remote repository `Demo` and a file `test.py` in the master branch of that repository. To get the diff between `test.py` from local and remote repository of the same branch, the following command can be used.

```bash
$ git diff remotename/branchname -- [local_path_of_file]
$ git diff origin/master -- test.py
```

# Viewing commit messages.

To print commit messages from a Git repository in most recent order, use the following command.

```bash
$ git log --oneline
```
Sample output -

```bash
bf5363b Docstring added
30237b8 search bst changed
a9223c1 bst in Python with unit tests
4d7cf03 unittest for LinkedList
98263e4 unittest cases for stack added
5365fbb array representation of a Queue
f9c7f8d size attribute added to Stack class
e81affa linked list representation of a stack
138601c Stack implemented in Python
9ea29fc Initial commit
```

# Undo the last git add.[^2]

Sometimes we add some unnecessary files in a specific commit via `git add`. To remove them before committing changes, `git reset` command is used.  
This will unstage the changes made to those files since the last commit.

```bash
$ git reset -- <file1> <file2> <file3>
```

Or to undo all the files you have added, just use `git reset`.

# Delete a remote branch.

In order to remove a remote branch from a Git repository, the following command is used.

```bash
$ git push origin --delete <remote_branch_name>
```

# Modify the last commit message.
Sometimes I want to change my last commit message. It might be because of a spelling mistake or the message was unusually long. So to modify the changes to commit message, `--amend` is used with `git commit`.

```bash
$ git commit -v --amend
```

# Untrack files without removing them.
Usually we come across some files which we don't want to be part of Git, such as config files, sensitive files or local system files etc. There are many ways to untrack them.

* Adding them to a `.gitignore` file.
* Not adding them in `git add` command.

I might be missing some other simpler methods. But below I will show one more method to untrack those files without deleting them.

```bash
$ git rm --cached <file_path>
$ git rm --cached local_conf.py
```

That's all. These are all the commands that I can think of right now. I will write a different post if I come across some more tricks like these.

[^1]: [http://stackoverflow.com/questions/21101572/git-diff-between-file-in-local-repo-and-origin](http://stackoverflow.com/questions/21101572/git-diff-between-file-in-local-repo-and-origin)
[^2]: [http://stackoverflow.com/questions/348170/undo-git-add-before-commit](http://stackoverflow.com/questions/348170/undo-git-add-before-commit)

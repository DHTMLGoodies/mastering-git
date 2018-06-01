# Mastering GIT

The best way to learn git is to use the command line. Use ui tools
later once you mastered the basics.

## The four areas

1. The working area - file system for your project
2. The index 
3. The repository - the .git folder
4. The stash

### The two questions

1. How does a specific git command move information acrosse the
four areas?
2. How does this command change the repository?

### The repository

Stored in .git

Contains blobs (files), trees (folders) and commits. 

These are 
- immutable, they can be deleted but never changed. 
- linked together

Commits
- is a snapshot
- each commit points to a parent commit(s)
- a branch points to a commit

HEAD
- can only be one HEAD
- HEAD is pointing to a branch


## The index

- unique to Git
- between the working area and the repository
- also called the staging area
- stage to staging area and commit to repository
- data stored in .git/index (binary)

```
git status
nothing to commit, working tree clean
```

means that the file system, index and repository
are the same.

```
git diff
```

compares the working area with the index.

```
git diff --cached
````

compares the index with the repository.


## Moving data to the right

Working Area -> Index -> Repository

1) Make changes to file "menu.txt"
2) git add "menu.txt" to move it to the index
3) git commit -m "Updated menu" to move it from index to repository

## Moving data to the left

Happens in
- git checkout branch (moves HEAD to new branch)

git checkout updates the HEAD to point to a different commit
It also updates the working area and the index

## Compare branches

```
git diff branch1 branch2
````


## git rm

1. Create new file COPYRIGHT.txt
2. git add COPYRIGHT.txt

the file is now in the working area and the index, but not in
the repository.

```
git rm --cached COPYRIGHT.txt
```

will remove the file from the index but keep it in the working area

```
git rm -f COPYRIGHT.txt
```

will remove the file from both the index and the working area.


## Moving files
Given: a clean repository with one file menu.txt

```
|-----------------------------------------------|
| working area |   index     |    repository    |
| menu.txt     |   menu.txt  |    menu.txt      |
|-----------------------------------------------|
```

1. from this clean state, rename menu.txt to menu.md 
2. do "git status"

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    menu.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        menu.md

no changes added to commit (use "git add" and/or "git commit -a")
````

The three areas now look like this:

```
|-----------------------------------------------|
| working area |   index     |    repository    |
| menu.md      |   menu.txt  |    menu.txt      |
|-----------------------------------------------|
```

git has a menu.txt in it's index, but cannot find it in
the working area, so it reports it as deleted.

It sees a new file called menu.md in the working area.
This is not in the index, nor the repository, so it marks
it as a new untracked file.

if you add both files:

```
git add menu.md
git add menu.txt
````
 git status will report:

 ```
 $ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        renamed:    newfile.txt -> menu.md
```

after both files has been added, git noticed that it's the
same file and that is has been renamed.

Git automatically detects when you are removing and moving
files.

git mv is a convenience method which renames a file and
adds it to the index.
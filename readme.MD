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

## git reset

* Confusing to many
* To understand reset, you need to understand branches
and be familiar with the Three main areas.
* reset does different things in different contexts.

### Commands that moves branches

* commit
* merge
* rebase
* pull

reset is a command designed to move branches.

```git reset --hard``` copies data from the repository to
the index and the working area.

```git reset --mixed``` copies data from the repository to the
index, but leaves the working area alone. ```--mixed``` is the
default option.

```git reset --soft``` moves the branch in the repository, but
leaves the index and repository alone.

in conclusion git reset moves the current branch and optionally
copies data from the repository to the other areas.

Example:
```
| Working area   |    index     |     repository   |
| menu.txt(1)    |  menu.txt(1) |     menu.txt(1)  |
```

Update menu.txt

```
| Working area   |    index     |     repository   |
| menu.txt(2)    |  menu.txt(1) |     menu.txt(1)  |
```

```
git add menu.txt
```

```
| Working area   |    index     |     repository   |
| menu.txt(2)    |  menu.txt(2) |     menu.txt(1)  |
```

commit it:
```
git commit -m  "Added menu item"
```

```
| Working area   |    index     |     repository   |
| menu.txt(2)    |  menu.txt(q2) |     menu.txt(2)  |
```

The master branch in the repository has now moved to the new commit.
HEAD is pointing at the master branch is moving as well.


create strawberry.txt, add it: ```git add strawberry.txt``` and commit it ```git commit -m "strawberry recipe"```. 

#### How to rollback the last two commits?

```
git log
```

which logs the last commits with a hash.

to undo the last two commits do

```
git reset --hard 7ad4c2a
````

this will move the branch/HEAD back to that commit and 
copy these files to the index and the working area.

The latest two commites in the repository will eventually
be garbage collected since no branch is pointing at them.

Example 2:

1. Update a file and add it with git add
2. the file is now in the working area and the index

To undo the "add", we can use a ```git reset --mixed HEAD``` or
simply ```git reset HEAD``` which will copy from the repository
to the index, but leave the working area alone.

Since no hash is defined, the branch/HEAD will not move, it will only
copy it's current data from the repository to the index, it will unstage what we staged with ```git add```.

To also undo our changes to the file we can do a

```
git reset --hard HEAD
```

Be careful with ```git reset --hard``` as it's one of the more
destructive commands in git


SUMMARY: Moving files between the areas

1. ```git add <file>``` : working area to index.
2. ```git checkout <file>``` index to working area.
3. ```git checkout HEAD <file>```repository to index and working area (use with caution)
3. ```git commit``` : index to repository
4. ```git reset HEAD <file>``` : repository to index. file is optional
5. ```git reset --hard HEAD```: from repository to index and working area.
6. ```git reset <hash>```:  moves the branch in the repository and copies it from repository to index, but leaves the working area untouched.
7. ```git reset --hard <hash>```: moves the branch/head in the repository to a specific commit and copies it's content to the index and the working area.
8. ```git stash``` : moves files from the working area and index to the stash area and put git in a clean state.
9. ```git stash pop``` : moves files from the stash to the working area.

```git reset --hard``` does not accept paths. It copies from repository to the index and the working area, i.e. put the repository in a clean state. 


## The stash
the only command updating the stash is the

```
git stash
````

command.


1. create new file (guacamole.txt)
2. add it with ```git add```.
3. Update another file "menu.txt"

guacamole.txt is now in the working area and the index. The latest version
of menu.txt is only in the working area.

```
git stash --include-untracked
````

can be used to also stash untracked files. The default is to not stash untracked files.

This is what git stash do

1) Copies data from the working area AND index to the stash
2) Do a checkout from the repository, i.e. copies files from the repository to the index and
the working area, thus putting git in a clean state.

####List stashes
```
git stash list
````

####Move from stash to working area

```
git stash apply
```

Applies the latest stash item.

```
git stash clear
```

Clears the entire stash


```
git stash pop
````

Copies content from the latest stash to the Working area and removes it.


## Merge conflicts

1. create new branch and do a checkout

```
git branch tomato
git checkout tomato
```

2. Update guacamole.txt

3. ```git add guacamole.txt```. 

4. ```git commit -m "Added tomato to the recipe```. 

5. ```git checkout master```

6. Update guacamole.txt again

7. ```git add guacamole.txt````

8. ```git commit -m "Added onion````

9. ```git merge tomato````

will now trigger a merge conflict

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 9 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   guacamole.txt
```

To resolve a conflict manually:

* Edit guacamole.txt and save it
* Add it to the index with ```git add guacamole.txt```

## Explore History

Git history commands

* ```git log --graph --decorate --oneline```
* ```git show 33b1869``` - show changes in a commit
* ```git show <branch>```- show changes in a branch
* ```git show HEAD```- changes in commit at HEAD
* ```git show HEAD^```- ^ = parent commit
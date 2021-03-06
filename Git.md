LearnGit
========

#### A while new different thought

Git is simple, just a key-value database, each file is checksumed by its content. Each commits only contains the snapshot of files in staging areas (the files added by `git add`). It's not "file oriented delta storage". 

#### git add <file> & git reset HEAD <file>
git add to add <file> into stage ( waiting to commit ). git reset HEAD <file> will remove <file> from stage.
  
#### git reset and git revert

* git reset have 4 modes: 
  * --soft: don't change index, don't change working tree
  * --mixed: (this is default), reset the index, don't change working tree
  * --hard : reset both index and working tree. 
* git reset HEAD : reset index (reverse opertaion of git add)
* git reset HEAD^: move current branch back to previous commit, reset index, but don't change working tree. 

#### Show information

* git status: will show whether there are files not in stage or not committed yet.

* git show <branch/commit>: will show the commit's information, usually includes summary and diff information.

* git log --graph --oneline: will show a brief history of all commits in current branch

* git log --graph --oneline --all: will show a brief history of all commits in all branches

* cat .git/HEAD: will show which branch does HEAD refer to.


#### Pull Branch / Merge / Resolving Merge

First you should know, there are 'local' and 'remote' repos.
```
git log
```
This will show the log info for you current local branch.
```
git log origin
```
This will show the log info for origin (Where to store the remote branches). Usually, 'git log' and 'git log origin' will be the same.

* Fetch objects and refs from remote repo.
```
git fetch
```
This will fetch objects and refs from remote repo to 'origin'.
```
git log origin # this would show the info of remote repo as they are just fetched from remote into origin
git log # show info of local repo, is different from what above shows.
```
However, git fetch won't change your code.

* Merge origin and current branch
```
git merge origin/master
```
This will merge the origin onto your current branch. Your code will be changed. If you use 'git log' and 'git log origin', the outputs are the same.

Or you can use git pull to do a git fetch followed by git merge automatically.

* Resolve the conflicts

If there are conflicts, there would be failed when merging. Also, git will change the conflict files. To resolve the conflicts:

```
vim a.txt # suppose a.txt is conflicted, you should first change a.txt
git add a.txt
git commit # only by committing a.txt, you mark a.txt as resolved
```

#### Reset/ Checkout / Revert

* git reset is to set the pointer HEAD:

```
git log --oneline

a09a03f V0.1.5
2805f3f V0.1.4

git reset 2805 # this would set HEAD pointing to V0.1.4, but the codes won't change
git checkout -f HEAD # change the code to V0.1.4
or git checkout -- <file>

# Above two commands are equal to the following:

git reset --hard 2805 # This would change HEAD to V0.1.4 and also change the code
```
* git checkout

```
git log --oneline

a09a03f V0.1.5
2805f3f V0.1.4

git checkout 2805 # this would change the code to V0.1.4, but commit a09a are still there. 
git chekcout -f # == git checkout -f HEAD if there's changes on working tree, with -f, the command will be aborted. 
git checkout -- a.txt # = git chekcout <Index> -- a.txt, will change the a.txt to current index's content. 
git checkout 2805 -- a.txt # will change a.txt to 2805's a.txt, it will also update the index to 2805's content.
git checkout -b new_branch # create a new branch based on 2805;
```
* different vs. git reset
  * git-checkout change HEAD and working tree and index; 
  * git checkout -f, discard current change;
* git reset change HEAD and branch tag and index, but not working tree
  * git reset --hard, also the working tree.
  * git reset HEAD, unstage the index. 

* git revert
git revert 
```
git log --oneline

a09a03f V0.1.5
2805f3f V0.1.4

git revert HEAD # This will create a new commit, and is working tree will be same as 2805; We can do this on published branches. 
```




* clean the untracked files and directories.

```
git clean -d -n\-f
```
option -d means directory. -n will print which file would delete. -f will delete them.

#### Branch

* create a new branch:

```
git branch <new_branch>
or
git checkout -b <new_branch>
>>>>>>> feature_branch
```

* merge with master:

```
git checkout master
git merge <new_branch> 
```

* delete branch:

```
git branch -d <new_branch>
```

* tracking remote branch

```
git checkout --track origin/remote_branch
or
git checkout -b remote_branch --track origin/remote_branch
```

* create a branch remotely

```
# create a new branch locally
git branch name_of_branch
git checkout name_of_branch
# edit/add/remove files    
# ... 
# Commit your changes locally
git add fileName
git commit -m Message
# push changes and new branch to remote repository:
git push origin name_of_branch:name_of_branch # = git push origin name_of_branch
```

* force the branch to a specifio commit
```
git log --oneline

a09a03f V0.1.5
2805f3f V0.1.4
git branch -f master 2805 # now master will point to 2805.
```

### Rebase and Merge

#### Merge
![enter image description here][1]

Merge is three-way merge between C2, C3 and C4. 

![enter image description here][2]

```
git checkout master
git merge experiment
```

Note `git pull` = `git fetch` && `git merge origin/master`

#### Rebase
Rebase is first generate two diff patches between C3 and C2, C4 and C2. Then apply the two patches in turn. 
![enter image description here][3]
```
git checkout experiment
git rebase master
```


### Which kind of commit is counted in contributions?

* If you create a new branch and commit changes to this branch, those commits won't be counted!

* If you merge to master locally, and push master to remote master, all the commits will be counted. But Still not sure will be counted in one day or multiple days.


### About remote `origin`

Origin is just an alias for remote url. It is created by default when you *clone* from remote repository.

* Show origin info

```
git remote show origin # you can also see the remote urls
#or
git romote show https://github.com/shixing/pyPBMT.git
```

If you init your git, and pull from remote repository, there will not be origin, you need to set it.

```
git remote add origin https://github.com/shixing/pyPBMT.git
```


#### Some notation

* HEAD: the current branch
* HEAD^: previous commit of current branch
* HEAD~n: previous n commit of current branch

#### Reference

http://stackoverflow.com/questions/315911/git-for-beginners-the-definitive-practical-guide/1350157#1350157


  [1]: http://git-scm.com/figures/18333fig0327-tn.png
  [2]: http://git-scm.com/figures/18333fig0328-tn.png
  [3]: http://git-scm.com/figures/18333fig0330-tn.png

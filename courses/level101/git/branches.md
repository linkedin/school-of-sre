# Working With Branches

Coming back to our local repo which has two commits. So far, what we have is a single line of history. Commits are chained in a single line. But sometimes you may have a need to work on two different features in parallel in the same repo. Now one option here could be making a new folder/repo with the same code and use that for another feature development. But there's a better way. Use _branches._ Since git follows tree like structure for commits, we can use branches to work on different sets of features. From a commit, two or more branches can be created and branches can also be merged.

Using branches, there can exist multiple lines of histories and we can checkout to any of them and work on it. Checking out, as we discussed earlier, would simply mean replacing contents of the directory (repo) with the snapshot at the checked out version.

Let's create a branch and see how it looks like:

```bash
$ git branch b1
$ git log --oneline --graph
* 7f3b00e (HEAD -> master, b1) adding file 2
* df2fb7a adding file 1
```

We create a branch called `b1`. Git log tells us that b1 also points to the last commit (7f3b00e) but the `HEAD` is still pointing to master. If you remember, HEAD points to the commit/reference wherever you are checkout to. So if we checkout to `b1`, HEAD should point to that. Let's confirm:

```bash
$ git checkout b1
Switched to branch 'b1'
$ git log --oneline --graph
* 7f3b00e (HEAD -> b1, master) adding file 2
* df2fb7a adding file 1
```

`b1` still points to the same commit but HEAD now points to `b1`. Since we create a branch at commit `7f3b00e`, there will be two lines of histories starting this commit. Depending on which branch you are checked out on, the line of history will progress.

At this moment, we are checked out on branch `b1`, so making a new commit will advance branch reference `b1` to that commit and current `b1` commit will become its parent. Let's do that.

```bash
# Creating a file and making a commit
$ echo "I am a file in b1 branch" > b1.txt
$ git add b1.txt
$ git commit -m "adding b1 file"
[b1 872a38f] adding b1 file
1 file changed, 1 insertion(+)
create mode 100644 b1.txt

# The new line of history
$ git log --oneline --graph
* 872a38f (HEAD -> b1) adding b1 file
* 7f3b00e (master) adding file 2
* df2fb7a adding file 1
$
```

Do note that master is still pointing to the old commit it was pointing to. We can now checkout to master branch and make commits there. This will result in another line of history starting from commit 7f3b00e.

```bash
# checkout to master branch
$ git checkout master
Switched to branch 'master'

# Creating a new commit on master branch
$ echo "new file in master branch" > master.txt
$ git add master.txt
$ git commit -m "adding master.txt file"
[master 60dc441] adding master.txt file
1 file changed, 1 insertion(+)
create mode 100644 master.txt

# The history line
$ git log --oneline --graph
* 60dc441 (HEAD -> master) adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

Notice how branch b1 is not visible here since we are on the master. Let's try to visualize both to get the whole picture:

```bash
$ git log --oneline --graph --all
* 60dc441 (HEAD -> master) adding master.txt file
| * 872a38f (b1) adding b1 file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

Above tree structure should make things clear. Notice a clear branch/fork on commit 7f3b00e. This is how we create branches. Now they both are two separate lines of history on which feature development can be done independently.

**To reiterate, internally, git is just a tree of commits. Branch names (human readable) are pointers to those commits in the tree. We use various git commands to work with the tree structure and references. Git accordingly modifies contents of our repo.**

## Merges

Now say the feature you were working on branch `b1` is complete and you need to merge it on master branch, where all the final version of code goes. So first you will checkout to branch master and then you pull the latest code from upstream (eg: GitHub). Then you need to merge your code from `b1` into master. There could be two ways this can be done.

Here is the current history:

```bash
$ git log --oneline --graph --all
* 60dc441 (HEAD -> master) adding master.txt file
| * 872a38f (b1) adding b1 file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

**Option 1: Directly merge the branch.** Merging the branch b1 into master will result in a new merge commit. This will merge changes from two different lines of history and create a new commit of the result.

```bash
$ git merge b1
Merge made by the 'recursive' strategy.
b1.txt | 1 +
1 file changed, 1 insertion(+)
create mode 100644 b1.txt
$ git log --oneline --graph --all
*   8fc28f9 (HEAD -> master) Merge branch 'b1'
|\
| * 872a38f (b1) adding b1 file
* | 60dc441 adding master.txt file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

You can see a new merge commit created (8fc28f9). You will be prompted for the commit message. If there are a lot of branches in the repo, this result will end-up with a lot of merge commits. Which looks ugly compared to a single line of history of development. So let's look at an alternative approach

First let's [reset](https://git-scm.com/docs/git-reset) our last merge and go to the previous state.

```bash
$ git reset --hard 60dc441
HEAD is now at 60dc441 adding master.txt file
$ git log --oneline --graph --all
* 60dc441 (HEAD -> master) adding master.txt file
| * 872a38f (b1) adding b1 file
|/
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

**Option 2: Rebase.** Now, instead of merging two branches which has a similar base (commit: 7f3b00e), let us rebase branch b1 on to current master. **What this means is take branch `b1` (from commit 7f3b00e to commit 872a38f) and rebase (put them on top of) master (60dc441).**

```bash
# Switch to b1
$ git checkout b1
Switched to branch 'b1'

# Rebase (b1 which is current branch) on master
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: adding b1 file

# The result
$ git log --oneline --graph --all
* 5372c8f (HEAD -> b1) adding b1 file
* 60dc441 (master) adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

You can see `b1` which had 1 commit. That commit's parent was `7f3b00e`. But since we rebase it on master (`60dc441`). That becomes the parent now. As a side effect, you also see it has become a single line of history. Now if we were to merge `b1` into `master`, it would simply mean change `master` to point to `5372c8f` which is `b1`. Let's try it:

```bash
# checkout to master since we want to merge code into master
$ git checkout master
Switched to branch 'master'

# the current history, where b1 is based on master
$ git log --oneline --graph --all
* 5372c8f (b1) adding b1 file
* 60dc441 (HEAD -> master) adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1


# Performing the merge, notice the "fast-forward" message
$ git merge b1
Updating 60dc441..5372c8f
Fast-forward
b1.txt | 1 +
1 file changed, 1 insertion(+)
create mode 100644 b1.txt

# The Result
$ git log --oneline --graph --all
* 5372c8f (HEAD -> master, b1) adding b1 file
* 60dc441 adding master.txt file
* 7f3b00e adding file 2
* df2fb7a adding file 1
```

Now you see both `b1` and `master` are pointing to the same commit. Your code has been merged to the master branch and it can be pushed. Also we have clean line of history! :D

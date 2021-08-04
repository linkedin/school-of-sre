# Git

## Prerequisites

1. Have Git installed [https://git-scm.com/downloads](https://git-scm.com/downloads)
2. Have taken any git high level tutorial or following LinkedIn learning courses
      - [https://www.linkedin.com/learning/git-essential-training-the-basics/](https://www.linkedin.com/learning/git-essential-training-the-basics/)
      - [https://www.linkedin.com/learning/git-branches-merges-and-remotes/](https://www.linkedin.com/learning/git-branches-merges-and-remotes/)
      - [The Official Git Docs](https://git-scm.com/doc)

## What to expect from this course

As an engineer in the field of computer science, having knowledge of version control tools becomes almost a requirement. While there are a lot of version control tools that exist today like SVN, Mercurial, etc, Git perhaps is the most used one and this course we will be working with Git. While this course does not start with Git 101 and expects basic knowledge of git as a prerequisite, it will reintroduce the git concepts known by you with details covering what is happening under the hood as you execute various git commands. So that next time you run a git command, you will be able to press enter more confidently!

## What is not covered under this course

Advanced usage and specifics of internal implementation details of Git.

## Course Contents

 1. [Git Basics](https://linkedin.github.io/school-of-sre/level101/git/git-basics/#git-basics)
 2. [Working with Branches](https://linkedin.github.io/school-of-sre/level101/git/branches/)
 3. [Git with Github](https://linkedin.github.io/school-of-sre/level101/git/github-hooks/#git-with-github)
 4. [Hooks](https://linkedin.github.io/school-of-sre/level101/git/github-hooks/#hooks)

## Git Basics

Though you might be aware already, let's revisit why we need a version control system. As the project grows and multiple developers start working on it, an efficient method for collaboration is warranted. Git helps the team collaborate easily and also maintains the history of the changes happening with the codebase.

### Creating a Git Repo

Any folder can be converted into a git repository. After executing the following command, we will see a `.git` folder within the folder, which makes our folder a git repository. **All the magic that git does, `.git` folder is the enabler for the same.**

```bash
# creating an empty folder and changing current dir to it
$ cd /tmp
$ mkdir school-of-sre
$ cd school-of-sre/

# initialize a git repo
$ git init
Initialized empty Git repository in /private/tmp/school-of-sre/.git/
```

As the output says, an empty git repo has been initialized in our folder. Let's take a look at what is there.

```bash
$ ls .git/
HEAD        config      description hooks       info        objects     refs
```

There are a bunch of folders and files in the `.git` folder. As I said, all these enables git to do its magic. We will look into some of these folders and files. But for now, what we have is an empty git repository.

### Tracking a File

Now as you might already know, let us create a new file in our repo (we will refer to the folder as _repo_ now.) And see git status

```bash
$ echo "I am file 1" > file1.txt
$ git status
On branch master

No commits yet

Untracked files:
 (use "git add <file>..." to include in what will be committed)

       file1.txt

nothing added to commit but untracked files present (use "git add" to track)
```

The current git status says `No commits yet` and there is one untracked file. Since we just created the file, git is not tracking that file. We explicitly need to ask git to track files and folders. (also checkout [gitignore](https://git-scm.com/docs/gitignore)) And how we do that is via `git add` command as suggested in the above output. Then we go ahead and create a  commit.

```bash
$ git add file1.txt
$ git status
On branch master

No commits yet

Changes to be committed:
 (use "git rm --cached <file>..." to unstage)

       new file:   file1.txt

$ git commit -m "adding file 1"
[master (root-commit) df2fb7a] adding file 1
1 file changed, 1 insertion(+)
create mode 100644 file1.txt
```

Notice how after adding the file, git status says `Changes to be committed:`. What it means is whatever is listed there, will be included in the next commit. Then we go ahead and create a commit, with an attached messaged via `-m`.

### More About a Commit

Commit is a snapshot of the repo. Whenever a commit is made, a snapshot of the current state of repo (the folder) is taken and saved. Each commit has a unique ID. (`df2fb7a` for the commit we made in the previous step). As we keep adding/changing more and more contents and keep making commits, all those snapshots are stored by git. Again, all this magic happens inside the `.git` folder. This is where all this snapshot or versions are stored _in an efficient manner._

### Adding More Changes

Let us create one more file and commit the change. It would look the same as the previous commit we made.

```bash
$ echo "I am file 2" > file2.txt
$ git add file2.txt
$ git commit -m "adding file 2"
[master 7f3b00e] adding file 2
1 file changed, 1 insertion(+)
create mode 100644 file2.txt
```

A new commit with ID `7f3b00e` has been created. You can issue `git status` at any time to see the state of the repository.

       **IMPORTANT: Note that commit IDs are long string (SHA) but we can refer to a commit by its initial few (8 or more) characters too. We will interchangeably using shorter and longer commit IDs.**

Now that we have two commits, let's visualize them:

```bash
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

`git log`, as the name suggests, prints the log of all the git commits. Here you see two additional arguments, `--oneline` prints the shorter version of the log, ie: the commit message only and not the person who made the commit and when. `--graph` prints it in graph format.

**Now at this moment the commits might look like just one in each line but all commits are stored as a tree like data structure internally by git. That means there can be two or more children commits of a given commit. And not just a single line of commits. We will look more into this part when we get to the Branches section. For now this is our commit history:**

```bash
   df2fb7a ===> 7f3b00e
```

### Are commits really linked?

As I just said, the two commits we just made are linked via tree like data structure and we saw how they are linked. But let's actually verify it. Everything in git is an object. Newly created files are stored as an object. Changes to file are stored as an objects and even commits are objects. To view contents of an object we can use the following command with the object's ID. We will take a look at the contents of the second commit

```bash
$ git cat-file -p 7f3b00e
tree ebf3af44d253e5328340026e45a9fa9ae3ea1982
parent df2fb7a61f5d40c1191e0fdeb0fc5d6e7969685a
author Sanket Patel <spatel1@linkedin.com> 1603273316 -0700
committer Sanket Patel <spatel1@linkedin.com> 1603273316 -0700

adding file 2
```

Take a note of `parent` attribute in the above output. It points to the commit id of the first commit we made. So this proves that they are linked! Additionally you can see the second commit's message in this object. As I said all this magic is enabled by `.git` folder and the object to which we are looking at also is in that folder.

```bash
$ ls .git/objects/7f/3b00eaa957815884198e2fdfec29361108d6a9
.git/objects/7f/3b00eaa957815884198e2fdfec29361108d6a9
```

It is stored in `.git/objects/` folder. All the files and changes to them as well are stored in this folder.

### The Version Control part of Git

We already can see two commits (versions) in our git log. One thing a version control tool gives you is ability to browse back and forth in history. For example: some of your users are running an old version of code and they are reporting an issue. In order to debug the issue, you need access to the old code. The one in your current repo is the latest code. In this example, you are working on the second commit (7f3b00e) and someone reported an issue with the code snapshot at commit (df2fb7a). This is how you would get access to the code at any older commit

```bash
# Current contents, two files present
$ ls
file1.txt file2.txt

# checking out to (an older) commit
$ git checkout df2fb7a
Note: checking out 'df2fb7a'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

 git checkout -b <new-branch-name>

HEAD is now at df2fb7a adding file 1

# checking contents, can verify it has old contents
$ ls
file1.txt
```

So this is how we would get access to old versions/snapshots. All we need is a _reference_ to that snapshot. Upon executing `git checkout ...`, what git does for you is use the `.git` folder, see what was the state of things (files and folders) at that version/reference and replace the contents of current directory with those contents. The then-existing content will no longer be present in the local dir (repo) but we can and will still get access to them because they are tracked via git commit and `.git` folder has them stored/tracked.

### Reference

I mention in the previous section that we need a _reference_ to the version. By default, git repo is made of tree of commits. And each commit has a unique IDs. But the unique ID is not the only thing we can reference commits via. There are multiple ways to reference commits. For example: `HEAD` is a reference to current commit. _Whatever commit your repo is checked out at, `HEAD` will point to that._ `HEAD~1` is reference to previous commit. So while checking out previous version in section above, we could have done `git checkout HEAD~1`.

Similarly, master is also a reference (to a branch). Since git uses tree like structure to store commits, there of course will be branches. And the default branch is called `master`. Master (or any branch reference) will point to the latest commit in the branch. Even though we have checked out to the previous commit in out repo, `master` still points to the latest commit. And we can get back to the latest version by checkout at `master` reference

```bash
$ git checkout master
Previous HEAD position was df2fb7a adding file 1
Switched to branch 'master'

# now we will see latest code, with two files
$ ls
file1.txt file2.txt
```

Note, instead of `master` in above command, we could have used commit's ID as well.

### References and The Magic

Let's look at the state of things. Two commits, `master` and `HEAD` references are pointing to the latest commit

```bash
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

The magic? Let's examine these files:

```bash
$ cat .git/refs/heads/master
7f3b00eaa957815884198e2fdfec29361108d6a9
```

Viola! Where master is pointing to is stored in a file. **Whenever git needs to know where master reference is pointing to, or if git needs to update where master points, it just needs to update the file above.** So when you create a new commit, a new commit is created on top of the current commit and the master file is updated with the new commit's ID.

Similary, for `HEAD` reference:

```bash
$ cat .git/HEAD
ref: refs/heads/master
```

We can see `HEAD` is pointing to a reference called `refs/heads/master`. So `HEAD` will point where ever the `master` points.

### Little Adventure

We discussed how git will update the files as we execute commands. But let's try to do it ourselves, by hand, and see what happens.

```bash
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

Now let's change master to point to the previous/first commit.

```bash
$ echo df2fb7a61f5d40c1191e0fdeb0fc5d6e7969685a > .git/refs/heads/master
$ git log --oneline --graph
* df2fb7a (HEAD -> master) adding file 1

# RESETTING TO ORIGINAL
$ echo 7f3b00eaa957815884198e2fdfec29361108d6a9 > .git/refs/heads/master
$ git log --oneline --graph
* 7f3b00e (HEAD -> master) adding file 2
* df2fb7a adding file 1
```

We just edited the `master` reference file and now we can see only the first commit in git log. Undoing the change to the file brings the state back to original. Not so much of magic, is it?

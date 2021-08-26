# Git with GitHub

Till now all the operations we did were in our local repo while git also helps us in a collaborative environment. GitHub is one place on the internet where you can centrally host your git repos and collaborate with other developers.

Most of the workflow will remain the same as we discussed, with addition of couple of things:

 1. Pull: to pull latest changes from github (the central) repo
 2. Push: to push your changes to github repo so that it's available to all people

GitHub has written nice guides and tutorials about this and you can refer them here:

- [GitHub Hello World](https://guides.github.com/activities/hello-world/)
- [Git Handbook](https://guides.github.com/introduction/git-handbook/)

## Hooks

Git has another nice feature called hooks. Hooks are basically scripts which will be called when a certain event happens. Here is where hooks are located:

```bash
$ ls .git/hooks/
applypatch-msg.sample     fsmonitor-watchman.sample pre-applypatch.sample     pre-push.sample           pre-receive.sample        update.sample
commit-msg.sample         post-update.sample        pre-commit.sample         pre-rebase.sample         prepare-commit-msg.sample
```

Names are self explanatory. These hooks are useful when you want to do certain things when a certain event happens. If you want to run tests before pushing code, you would want to setup `pre-push` hooks. Let's try to create a pre commit hook.

```bash
$ echo "echo this is from pre commit hook" > .git/hooks/pre-commit
$ chmod +x .git/hooks/pre-commit
```

We basically create a file called `pre-commit` in hooks folder and make it executable. Now if we make a commit, we should see the message getting printed.

```bash
$ echo "sample file" > sample.txt
$ git add sample.txt
$ git commit -m "adding sample file"
this is from pre commit hook     # <===== THE MESSAGE FROM HOOK EXECUTION
[master 9894e05] adding sample file
1 file changed, 1 insertion(+)
create mode 100644 sample.txt
```

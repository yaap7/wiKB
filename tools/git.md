# git

## Clone a repository

From github through HTTPS :

``` bash
git clone https://github.com/yaap7/wiKB
```

From github through SSH:

``` bash
git clone git@github.com:yaap7/wiKB.git
```

From a custom SSH server:

``` bash
git clone TODO
```

## How-to - host your own git server

See [this HowTo](../howto/git_host_our_own_server.md).

## Create a local branch tracking a remote branch

If my current branch is on heroku/master, and I want to create a new local branch `github-deployment` which will push/pull from origin/deployment, I can use this command:

``` bash
git branch github-deployment origin/deployment
```

## Rename remotes

To rename a remote (e.g. to rename origin to github), use `git remote rename <old_branch> <new_branch>`. Example:

``` bash
git remote rename origin github
```

## Delete remote branches

To delete a remote branch, use `git push -d <remote> <branch>`. Example:

``` bash
git push -d github dev_branch
```

After the deletion is completed, other developers have to update their remotes using:

``` bash
git fetch --all --prune
```

(`git remote update` is not enough!)

## Push on another branch than the current one

If the branch you are working on is tracking a remote branch, e.g. your local branch `dev` is tracking the `remotes/github/dev`, you still can push to `remotes/heroku/master` by using `git push <remote> <local_branch>:<remote_branch>`. Example:

``` bash
git push heroku dev:master
```

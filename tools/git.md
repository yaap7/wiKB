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

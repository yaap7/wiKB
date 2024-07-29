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

## Sign your commits and tags

Reference: <https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key>

Enable signing globally:

``` bash
git config --global tag.gpgsign true
git config --global commit.gpgsign true
```

Add signing key per repo if you have multiple keys for multiple identities:

``` bash
$ gpg --list-secret-keys --keyid-format=long
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot <hubot@example.com>
ssb   4096R/4BB6D45482678BE3 2016-03-10

git config user.signingkey 3AA5C34371567BD2
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

## Create aliases

To simply type `git lgo` to launch `git log --oneline`:

``` bash
git config --global alias.lgo "log --oneline"
```

## Push on another branch than the current one

If the branch you are working on is tracking a remote branch, e.g. your local branch `dev` is tracking the `remotes/github/dev`, you still can push to `remotes/heroku/master` by using `git push <remote> <local_branch>:<remote_branch>`. Example:

``` bash
git push heroku dev:master
```

## Ressources

* <https://kubesimplify.com/get-good-at-git>

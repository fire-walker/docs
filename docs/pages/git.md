## Local SSH

To generate ssh keys, change `key-name` and `/absolute/path`

``` bash
ssh-keygen -t rsa -b 4096 -C "key-name" -f "/absolute/path"
```

Check if ssh-agent is running

``` bash
eval "$(ssh-agent -s)"
```

Add generated ssh key to ssh-agent

``` bash
ssh-add "/key/location"
```

Then just copy the `.pub` and paste it into `github.com`

## Rollback Changes

This cleans up the repo of any untracked changes as per the last commit then pulls in the latest version.[^1].

``` bash
# clean up
git reset
git checkout .
git clean -fdx

# sync to latest
git pull
```

## Automated Deployment

There are multiple ways to automate deployment. See the pros and cons of each of these methods [here](https://docs.github.com/en/developers/overview/managing-deploy-keys). For more ways to do this check out this [cheatsheet](https://coolaj86.com/articles/vanilla-devops-git-credentials-cheatsheet/)

### Deploy Keys

This is very similar to local development with the usual ssh keys. The only difference is in how much this key gets to do. It's access is limited only to the repo it's added to.

Create a key pair and add it here `Repo > Settings > Deploy Keys`

``` bash
ssh-keygen -t rsa -b 4096 -C "key-name" -f "/abs/path"
```

Using deploy keys in scripts. The `.ask-pass` should `echo` the ssh key password.

``` bash
# start ssh-agent
eval "$(ssh-agent -s)"

# add the ssh key
DISPLAY=:0 SSH_ASKPASS="/abs/path/.ask-pass" ssh-add ~/.ssh/<key-name>

# do stuff

# kill ssh-agent
trap "ssh-agent -k" exit
```

!!! info ""
    **Always remember to kill what you start.**

[^1]: https://stackoverflow.com/questions/14075581/git-undo-all-uncommitted-or-unsaved-changes

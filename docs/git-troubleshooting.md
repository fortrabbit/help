---

template:      article
reviewed:      2020-10-29
title:         Git troubleshooting
naviTitle:     Git troubleshooting
excerpt:       What you may want to know to debug Git issues
lead:          'This article aims to help you finding common issues when deploying with Git on fortrabbit.'
group:         troubleshooting
stack:         all
dontList:      false

---


## Read first please

1. **If you are new to Git**: Check out [Git intro article](/git) first, learn the basic concepts and how to install Git for your local development
2. **If you are setting up Git here for the first time**: Read the [Git deployment guide](/git-deployment) first, see how Git is integrated as a deployment method at fortrabbit 

## Common issues and solutions


### Permission denied error

If you see this after issuing the commands `git push` or `git pull`,

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

then you the key you are using is not associated with your Dashboard account. Verify that you have imported your key into your fortrabbit account and that [Git](git) is correctly installed on your machine. It may take a up to two minutes for a new key to get activated, after it is imported. Usually it works within 30 seconds.


### Deployment lock error

```
!! Could not get lock on repository; probably deployment in progress.
```

Wait around 20 minutes if you see the above message when deploying. That can happen when somebody else is deploying at the same time, a Git deployment was cancelled or there was a connection error during the deployment. It should not happen often, but can happen from time to time. As the error suggests, the fortrabbit deploy service makes sure that only one deployment is ongoing at a time. That will be cancelled after some time if not finished successfully. Contact support, if the repository lock is not resolving or if this happens often.

### Git client max connections

Some graphical clients for Git, like SourceTree and GitKraken are fetching often and are opening many parallel connections. So you might get blacklisted here for using one of those. You can change the settings of your GUI client: In GitKraken, you can turn off auto-fetching under your preferences. In SourceTree you can turn off checks for default remotes.


### Composer errors

Sometimes you might see a Composer error in the git deploy log. This is rare. This can stop your code to be deployed. Usually this is caused by configuration issues on your side. Check your composer.json for dependency issues.


## Reporting issues with Git in support

To help us helping you please provide a verbose output of the push (or pull) operation:

```
# See remotes
$ git remote -v

# Generate verbose output for push
$ GIT_SSH_COMMAND="ssh -vvv" git push fortrabbit master

# Generate verbose output for pull
$ GIT_SSH_COMMAND="ssh -vvv" git pull fortrabbit master
```

**Note**: The remote name `fortrabbit` might be `origin` or any other custom name you have chosen, or it might not be needed, so without `fortrabbit master`

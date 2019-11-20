---

template:      article
reviewed:      2019-08-01
naviTitle:     Multi-staging
title:         Multi stage App life cycles
lead:          Learn about development/production environments and how to run them on fortrabbit.
group:         platform
stack:         all

---

## Goal

**Move fast, break nothing.** Experimenting without downtimes. Multi staging is quite the opposite of open heart surgery — it's an advanced strategy to run the same application in separate but similar environments. It's especially useful in teams but can also bring benefits for the one-man-army.

### Use cases

* **Feature development**: Build a new version while still being able to fix bugs in the running App without uploading all the new feature code.
* **Purpose separation**: The backend team can break things while the frontend team can still work uninterrupted.
* **Continuous integration**: Code needs to be tested and monitored before being deployed to the live App.


### Common set ups

* **staging + production**: "Staging" is where you do your development. "Production" runs the live App. From time to time you migrate all the (stable) changes from staging into production.
* **temporary + production**: Same as above, it's more of a one-time-developed project. Maybe once a year there is an upgrade, a re-brush or alike. For this you utilize an additional, temporary environment, in which you do the upgrades.
* **testing + staging + production**: Code changes are made to the testing environment. Once they seem stable, they get published to the staging where they are monitored and further tested. Finally they get published to production.

### Differences to local development

Most likely you are developing with a local PHP environment on your machine. So your laptop is already a of kind of **staging** while the actual App at fortrabbit is **production**. Please also see our [local development article](local-development).


## Multi-staging on fortrabbit

The short of it: all you need to utilize multi-staging on fortrabbit is multiple Apps. We further assume you are using our [Git deployment](git-deployment) — because then your local setup is easier to manage.

Git supports multiple, named branches of your code. Per default, it comes with a branch called `master`. When pushing to fortrabbit our deployment will look for the `master` branch and deploy it. 

To make things easier for multi-staging scenarios, there is another branch, which is preferred over the `master` branch by the fortrabbit deployment: A branch named like your App. Say the name of your App is `your-app`, then you can create a branch called `your-app` which will be deployed instead of the `master` branch.

### Sample setup

Assuming you use a 3-stage layout: testing, staging and production. You start by creating three Apps. Let's name them: `your-app-test`, `your-app-stage` and `your-app-prod`. Now you can map those App names with local branch names. Here is how:

#### Clone the first App

```bash
# Clone the testing App:
$ git clone {{app-name}}@deploy.eu2.frbit.com:your-app-test.git {{your-app-test}} {{app-name}}

# Go into the folder
$ cd {{app-name}}
```

This first cloning will create the remote `origin` and clone the `master` branch to your local disk.

#### Add the other Apps

First you should rename the cloned `origin` remote to `testing`. Then you can add the two additional staging and production Apps:

```bash
$ git remote rename origin testing
$ git remote add staging {{your-app-stage}}@deploy.{{region}}.frbit.com:{{your-app-stage}}.git
$ git remote add production {{your-app-prod}}@deploy.{{region}}.frbit.com:{{your-app-prod}}.git
```

#### Mapping branches

Again, start out with the testing App by creating a new local branch named as the testing App. Then you can push it to the remote using the `-u` flag, which will create a "link" between the local and the remote branch:

```bash
# Map branches to Apps
$ git checkout -b {{your-app-test}}
$ git push -u testing {{your-app-test}}
$ git checkout -b {{your-app-stage}}
$ git push -u staging {{your-app-stage}}
$ git checkout -b {{your-app-prod}}
$ git push -u production {{your-app-prod}}


# Verify settings
$ nano .git/config
# contents should look something like this:

# [core]
#     repositoryformatversion = 0
#     filemode = true
#     bare = false
#     logallrefupdates = true
# [remote "testing"]
#     url = git@deploy.eu2.frbit.com:your-app-test.git
#     fetch = +refs/heads/*:refs/remotes/testing/*
# [remote "staging"]
#     url = git@deploy.eu2.frbit.com:your-app-stage.git
#     fetch = +refs/heads/*:refs/remotes/staging/*
# [remote "production"]
#     url = git@deploy.eu2.frbit.com:your-app-prod.git
#     fetch = +refs/heads/*:refs/remotes/production/*
# [branch "your-app-test"]
#     remote = testing
#     merge = refs/heads/your-app-test
# [branch "your-app-stage"]
#     remote = staging
#     merge = refs/heads/your-app-stage
# [branch "your-app-prod"]
#     remote = production
#     merge = refs/heads/your-app-prod
```

#### Commit to testing

Let's play do a commit cycle. Code away, build something great.

```bash
# go to testing branch
$ git checkout {{your-app-test}}
# make changes..
$ git commit -am 'My changeset'
# this will automatically push to the testing App, since the branch is linked
$ git push
```

#### Merge upwards to staging

When everything works out as planned and your codes reaches a sufficient level of majority you can merge your testing code into staging:

```bash
# go to staging branch
$ git checkout {{your-app-stage}}
# merge changes from testing
$ git merge {{your-app-test}}
# this will automatically push to the staging App, since the branch is linked
$ git push
```

#### Merge upwards to production

Now everything should have been thoroughly tested and is ready for production.

```bash
# go to production branch
$ git checkout {{your-app-prod}}
# merge changes from staging
$ git merge {{your-app-stage}}
# this will automatically push to the production App, since the branch is linked
git push
```

## Caveat runtime data

The biggest problem introduced with multi stage environments is runtime data: databases, user uploads and anything else that is being generated by the App. Sure you need to separate those as well. If all environments require the latest data to run properly or for testing, you need to figure out a way to synchronize the production data downwards, to the other environments. These problems are highly individual and cannot be solved in a general manner. We are experienced with these kind of things and [we love to help you](http://www.fortrabbit.com/contact).

## Further reading

We have also written a [blog post](http://blog.fortrabbit.com/multi-stage-deployment-for-website-development) (some while ago!) about this.

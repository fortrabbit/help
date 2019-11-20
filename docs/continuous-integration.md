---

template:         article
reviewed:         2017-12-20
title:            Integrating CI with fortrabbit
naviTitle:        Continuous Integration
lead:             Automated testing, builds, special deploy scripts and other actions on fortrabbit.
group:            deployment
stack:            all
dontList:         true
deprecated:       true

keywords:
    - ci
    - continuous-integration
    - wrecker
    - travis
    - jenkins
    - hudson

---

So, you want to use continuous integration? Deploy only if your tests succeed? This article gives you the big picture about how to do just that on fortrabbit.

## Problem

Your App is a complex monster: dependencies and lot's of things that can go wrong. You change something here which breaks something over there. You maybe have some tests in place, but there is no workflow to run all of your tests frequently.

## Solution

Continuous Integration to the rescue — first making things even more complicated but then better. Apply an incremental and iterative software development style:

```nohighlight
 .------.     .------.     .--------.
 | code |---->| test |---->| deploy |
 '------'     '------'     '--------'
    ^                          |
    '--------- repeat ---------'
```

You can **code** on your local machine and **deploy** to fortrabbit. For the **testing** part you must either: run tests locally before each `git push` or integrate an external CI provider in your deployment and automate the whole process.




This article helps you to integrate an external CI provider in your workflow.

## Usage

```nohighlight
 #####
# + + #     +———————————+  +—————————+  +————+  +————————————+
# ––– #———> | Git local |->| GitHub… |->| CI |->| fortrabbit |
 #####      +———————————+  +—————————+  +————+  +————————————+
  you
```

Most, if not all, of the CI providers assume that you host your code on a public available version control (Git) repository. This does not mean your repository publicly accessible without authentication, just that it's not on your local host. You can use [GitHub](https://github.com) or [Bitbucket](http://bitbucket.org/) and any other VCS provider to host a (private) public repositories.



### Workflow

You need to setup a hook, which notifies your CI provider whenever you push something (the CI providers will tell you how). Once that's done:

1. You push your code changes to your remote (GH, BB, ...)
3. The CI providers pulls the changeset and runs your tests
4. If the tests succeed, the CI provider will deploy your App to fortrabbit

### CI & multi staging

The general concepts of our [multi staging](multi-staging) apply, just the `local branch` -> `remote branch` mappings are different.

For example, if you have three environments: `testing`, `staging` and `production` and if you use Bitbucket as your repository, you would create three local branches: `test`, `stage` and `prod`. All three local branches would map directly to branches of the same name on Bitbucket. On Bitbucket, you would now set up your deployment hook, which tells your CI provider.

```bash
# 1. clone the repo from BitBucket (or Github) example
$ git clone git@bitbucket.org:your-username/your-repo-name.git

# 2. Go to into that folder
$ cd your-repo-name

# 3. rename master branch to test branch, if you haven't already
$ git branch -m master test

# 4. create stage and prod branch, if you haven't already
$ git branch stage
$ git branch prod

# 5. do some stuff in the test branch
git checkout test
# .. code

# 6. Commit changes
$ git commit -am 'My changes'

# 7. on first push to your bitbucket remote, make sure use the "-u" switch
$ git push -u origin test

# 8. Merge test into stage
$ git checkout stage
$ git merge test
$ git push -u origin stage

# 9. merge stage into prod
$ git merge stage
$ git checkout prod
$ git push -u origin prod
```

---

template:      article
reviewed:      2019-09-30
naviTitle:     Getting started with Git
title:         Getting started with Git on fortrabbit
lead:          This is a quick intro to Git, how to set it up and how to use it on fortrabbit.
group:         deployment
stack:         all

keywords:
    - ssh key
    - public key
    - git
    - ssh
    - Putty
    - msysGit
    - Cygwin
    - Windows
    - Git Bash

---

## About Git

Git is a distributed version control system. It's quite popular to work with text files — hence in software development. It's fast, assures data integrity and supports non-linear workflows (branching). Git was developed by Linux kernel developers (including Linus Torvalds). It's free and can be used either from the command line or via graphical user interfaces.

Git can help you to collaborate on code projects, keep track of your code changes and rollback to points in time, if needed. It comes with:

* A history of all files included, so you can review or undo changes
* Powerful file merging which makes collaboration easy

To deploy code to fortrabbit you use Git from your local machine and push to the remote on fortrabbit. So it should be part of your [local development setup](local-development).


## Learn Git

To successfully use fortrabbit you should be familiar with Git standard operations and concepts — these are: `commit`, `push` and `pull`. If you don't know Git, yet, we recommend to go ahead and learn the basics. You will profit from it either way — whether you keep using fortrabbit or not. It's a cornerstone of todays software development. For developers not used to the shell it might not be very intuitive to start with, but once you got started it soon becomes very, very handy for all kinds of things besides deploying to fortrabbit. There are many good tutorials out there in the interwebs to get started. For example:

* [Official docs from Git SCM](https://git-scm.com/doc)
* [Try Git in your browser for free](https://try.github.io/levels/1/challenges/1)
* [Guides & ebook from Git Tower](https://www.git-tower.com/learn/)
* [Get Git right from Atlassian](https://www.atlassian.com/git/)
* and [many more](http://lmgtfy.com/?q=learn+git)



## Install Git

To use Git, you need to have it installed on your local machine. You might already have Git: open a terminal window and type `git --version`. Good news for macOS and Linux users: you most likely already have it installed.

### Git on Windows

You can work it out. The primary problem you have to solve is: which Git distribution are you going to use. There are multiple of those floating around, we recommend to download and install Git from the **[official Git website](https://git-scm.com/downloads)**. This one comes with the "Git Bash".

* [Stack Overflow: Difference between Gits for Windows](http://stackoverflow.com/questions/22310007/differences-between-git-scm-msysgit-git-for-windows)
* [Beanstalk Git Windows install guides with SSH key setup](http://guides.beanstalkapp.com/version-control/git-on-windows.html#installing-Git)



## Using Git with fortrabbit

Once you have everything setup and working you can have a look at how to use Git with fortrabbit:

### Authentication

You can either authenticate with your Dashboard password or with your [public SSH keys](/ssh-keys). The code examples in these help pages will show you examples that you can copy/paste. Please see our [authentication Article](/access-methods) to learn about those methods. 


### Using the remote Git repo as a version control system

Each [App](app) comes with a dedicated Git remote repository. In Git lingua this is only called "remote". This server-side repository can act as a backup for your code and as the base for collaboration. All fortrabbit Git remotes are private for you and your team.


### Using Git for deployment

On top of storing your code in a different location (local machine + fortrabbit remote) Git is also the way to deploy code changes to your App. When issuing `git push` your code will be transferred to the fortrabbit remote and a chain of deployment processes is triggered. Those run [Composer](https://getcomposer.org/) and execute custom scripts you can define yourself, if needed. The resulting code base and files will be packaged and distributed to the various components of your [Apps](app): the PHP component (public web) and the optional Worker component (background). Please see our [deployment article](deployment) for detailed informations and advanced usage.


## Git desktop GUIs

Most people probably use Git from the command line (aka bash, terminal, shell). But there are also GUIs (desktop Apps) to manage Git. Those help to get started and to see visually what's going on:

* [Git Desktop GUIs list](https://git-scm.com/downloads/guis)


### Git is not GitHub

Sometimes people confuse Git with GitHub. [GitHub](https://github.com) is a popular provider to host Git repositories publicly or privately. GitHub has extended Git workflows with neat communication tools around the basic Git usage. Most notable is the "pull request" workflow. The fortrabbit [Dashboard](dashboard) does not offer such project communication tools — it's barebone Git only. You can however build your own workflow which includes a hosted Git repo on GitHub ([integration guide](/github)) or Bitbucket ([integration guide](/bitbucket)) or any another Git repo provider.

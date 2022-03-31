---

template:         article
reviewed:         2022-03-18
title:            Install Kirby on fortrabbit
naviTitle:        Kirby
lead:             Kirby is a popular, file based CMS originated from Germany (YEAH!). Learn here how to install and tune Kirby on fortrabbit.
group:            Install_guides
stack:            uni
deprecated:       true
dontList:         true
workInProgress:   false

websiteLink:      https://getkirby.com/
websiteLinkText:  getkirby.com
category:         CMS
image:            kirby-mark.png
version:          2.5.10

otherVersions:
    3 : install-kirby-3


keywords:
    - getkirby
    - starterkit

---

## Get ready

Make sure you to have completed all steps in the [get ready guide](/get-ready) and have the Kirby "starterkit" downloaded, so that you have something to start with. Better you have it already running.

### Choose your deployment work-flow

There are two main ways to deploy code here on fortrabbit: [Git](/git-deployment) and [SFTP](/sftp-uni). The general rule of thumb is, that content driven legacy applications, like [WordPress](/install-wordpress), are better uploaded in classical manner with SFTP. Modern PHP web frameworks that are based on [Composer](/composer) are mostly deployed with Git. 

Now, Kirby is a bit in between. Kirby is, like [Grav](/install-grav), file based. So there is no database, the actual contents are text files written on the file system. Ask yourself: Where will the contents of your Kirby website will get generated? 

1. Always locally before deployment  > Use Git
2. Online via the "panel" (Kirby Dashboard) > Probably use SFTP

#### When to deploy Kirby with SFTP

* You or your client/editor is editing files directly on the live App in the "panel"
* You are used to work with SFTP

#### When to deploy Kirby with Git

* You are not using the "panel" (on remote)
* You are using Kirby alone, or your co-editors are also using Git

[Our architecture graphic here](/deployment-methods-uni#toc-understanding-the-architecture) shows you that the files, you can access via SFTP or SSH (Universal App) are not the ones, that are in Git. The Git repo is a separated thing. So, deploying with Git is a one way street that only goes up, not down. In other words: You can NOT `git pull` any changes you have made on the Apps file system. In an ideal world, code and content are maybe separated. With a file based CMS this is all together.

## Deploy with SFTP

There is not much to say on that topic. Just make sure to upload all contents of your local Kirby folder, including the hidden .htaccess file into the `htdocs` folder within your App. 


## Deploy with Git

Here is a basic rundown to initialize Git, add fortrabbit as a remote and push changes to your App:

```bash
# 1. Initialize Git (when not already)
$ git init .

# 2. Add your Apps Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'My first commit'

# 6. Initial push and upstream
$ git push -u fortrabbit main

# From there on only
$ git push
```

### An advanced way to use Git

Advanced users might separate the content parts from the code parts and use Git to deploy the code parts and SSH/SFTP or maybe best `rsync` to manage the actual contents.



## Customization

So far we have covered the vanilla parts. Let's get deeper:


### Sending mails

fortrabbit does not support `sendmail`, so sending mails out of the box might not work. Kirby provides config for transactional mail providers, or you can use a plugin to send e-mail over your own SMTP account.


### Setting the panel option

When initially setting up the `panel`, you might gonna see an error about the server name. You can add this configuration line to the file: `site/config/config.php`:

```
c::set('panel.install', true);
```

### Get a license

Remember that Kirby is not a free software. Get a license, support the authors!

## Disclaimer

Kirby is a mighty system with lot's of options. I might have missed some good stuff. Don't hesitate to contribute here. This install guide is [public on GitHub](https://github.com/fortrabbit/help/blob/master/docs/install-kirby-2.md)
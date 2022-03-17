---

template:         article
reviewed:         2022-03-18
title:            Install Slim Framework 3
naviTitle:        Slim Framework
lead:             Slim is a PHP micro framework that helps you write simple web applications and APIs quickly. Learn how to install and tune Slim 3 on fortrabbit.

group:            Install_guides
stack:            all
deprecated:       true
dontList:         true
dontIndex:        true

websiteLink:      http://www.slimframework.com/?utm_source=fortrabbit
websiteLinkText:  slimframework.com
category:         micro framework
image:            slim-logo.png
version:          3.10

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready).

## Root path

Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

## Install

Execute locally in your terminal:

```bash
# 1. Use Composer to create a local Symfony project named like your App
$ composer create-project slim/slim-skeleton {{app-name}}

# 2. Change into the folder
$ cd {{app-name}}

# 3. Initialize a local Git repo
$ git init .

# 4. Add all files
$ git add -A

# 5. Commit files for the first time
$ git commit -m 'Initial'

# 6. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 7. Initial push and upstream
$ git push -u fortrabbit main
# That's it.

# 8. From there on only
$ git push
```

You can see the welcome page in the browser:
[https://{{app-name}}.frb.io/](https://{{app-name}}.frb.io/)
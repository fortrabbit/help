---

template:         article
reviewed:         2018-04-29
title:            Install Grav
naviTitle:        Grav
lead:             Grav is a popular free file based CMS based on Twig & Markdown. Learn here how to install and tune Grav on fortrabbit.
group:            Install_guides

websiteLink:      https://getgrav.org?utm_source=fortrabbit
websiteLinkText:  getgrav.org
category:         CMS
image:            grav-symbol.png
version:          1.4.3
stack:            pro
uniLink:          install-grav-1-uni
supportLevel:     b
deprecated:       1

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready).


## Install

[Grav](http://getgrav.org) runs pretty much out of the box, as long as you use the [zip package installation](http://learn.getgrav.org/basics/installation#option-1-install-from-zip-package) installation method.

### Download

First **[download](http://getgrav.org/downloads)** and unpack the latest Grav locally (Grav core, not with Admin). It unpacks in the subfolder `grav`.

### Configure for Composer

Exclude the `vendor/` and the `cache/` folder from Git. To do so, create a  new file (in your text-editor) with the following contents and save it on top level of the grav folder as the `.gitignore` file:

```
vendor
cache/*
!cache/.gitkeep
```

### Git deployment

```bash

# 1. Change into the folder
$ cd grav

# 2. Initialize Git
$ git init .

# 3. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'Initial'

# 6. Initial push and upstream
$ git push -u fortrabbit master

# 7. From there on only
$ git push
```

Done. Your Grav site is now online and you can visit it in your browser!
[https://{{app-name}}.frb.io](https://{{app-name}}.frb.io)

## Tuning

### Install theme

Grav supports themes and plugins, which both can be installed with the `bin/gpm` tool. Installing plugins or themes is easy: Find the [theme you like](http://getgrav.org/downloads/themes). Run locally:

```bash
$ bin/gpm install {{theme}}
```

Open `user/config/system.yaml` and set `pages/theme` to `{{theme}}`. Now commit and push:

```bash
$ git add -A
$ git push -m 'New theme'
$ git push
```

Done.

### Install plugin

Find the [plugin you need](http://getgrav.org/downloads/plugins) then install it as usual (we use download here). Following an an example. Run locally:

```bash
$ php bin/gpm install [[simplesearch]]
```

Then configure the plugin, if needed (usually in `user/plugin/<pluginname>/<pluginname>.yaml`), commit and push:

```bash
$ git add -A
$ git push -m 'New plugin'
$ git push
```

Done.

### Grav Admin VS ephemeral storage

The popular [Grav Admin plugin](https://learn.getgrav.org/admin-panel/introduction) provides a way to manage a site in a guided web interface similar to the "wp-admin". This is great, when you want your client to manage her/his Grav site her/himself. Unfortunately it does not play well with ephemeral storage. Like with other PaaS — the local file system storage of the App will be wiped on each deploy — think 12-factor-app. So all local changes made in the Grav Admin panel will be gone.

Currently you should: use Grav on fortrabbit for your own private projects where you: write locally in your text editor, handle the pages in Git.

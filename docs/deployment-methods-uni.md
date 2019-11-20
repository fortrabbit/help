---

template:      article
naviTitle:     "Deployment methods"
title:         "Deployment methods"
reviewed:      2019-09-30
excerpt:       "How Git, SSH & SFTP work side by side."
group:         deployment
stack:         uni

---

[Universal Apps](app-uni) offer three methods to deploy and access code:

3. [SFTP](/sftp-uni) — legacy upload style
1. [Git](/git-deployment) — advanced: push to deploy
2. [SSH](/ssh-uni) — advanced tasks and deployment

The different deployment methods work side by side. This articles explains how they interact, what you need to know when mixing methods and give tips for best practices.

## Understanding the architecture

```nohighlight
# Universal App deployment architecture
                                                         ┌───────────────┐
                                                         │ website users │
                                                         └──────┬────────┘
                                                                │
                                                          http requests
                                                                │
                            ┌───────────────────────────────────┼─────────┐
                            │fortrabbit App                     │         │
                            │                                   │         │
┌────────────────┐          │┌─────────────────┐         ┌──────▼────────┐│
│ local Git repo ├─Git push─┼▶ remote Git repo ├──rsync──▶  web storage  ││
└────────────────┘          │└─────────────────┘         └──────▲────────┘│
                            │                                   │         │
                            │                                   │         │
                            └───────────────────────────────────┼─────────┘
                                                                │
                                                                │
                                                            SFTP/SSH
                                                                │
                                                          ┌─────┴─────┐
                                                          │ developer │
                                                          └───────────┘
```

### What the web storage contains

Please keep in mind, that the Git repo is not the web storage. After you Git push, first the Git repo will be updated, then changes will be synchronized (overwrite but not delete) to the web storage. So the web storage contains:

1. The latest Git changes synced in
2. Changes from SFTP & SSH
3. Uploads from website users

### Git works only one way

**Git is a one way street here and the only way is up.** You can not `pull` the changes, which are made via SSH or SFTP, from the web storage back into your Git repo. So you can upload something via SFTP and clone it down later via Git. While this might looks odd at first: this design keeps your Git repo clean of temporary, binary and other blob data. The diagram above visualizes this. Use Git only for code deployment, not to manage all of your Apps runtime data. Separate code - managed in Git - from content - managed via SSH/SFTP.

### Git push overwrite but not deletes

The [Professional Stack](app-pro) has [atomic deployments](app-pro#toc-atomic-deployment). With the Universal Stack it's different, the contents from Git will be synced into the web space:

When you push new changes via Git to your App Composer will be executed. In addition, user defined scripts (either specified via [deployment file](/deployment-file-v2) or [Composer scripts](https://getcomposer.org/doc/articles/scripts.md)) might also be executed. Resulting of the push and the executions, a temporary file set is generated and subsequently synchronized to your App's web storage. So, when accessing your App via SSH/SFTP, the web storage can contain files which are not in Git. 

The strategy applied in the synchronization is:

* Files existing in the temporary file set and existing on the web storage will be overwritten
* Files existing in the temporary file set but not existing on the web storage will be created
* Files not existing in the temporary file set but existing on the web storage will not be touched

### Not all applications work well with Git

Git deployment is great when your App skeleton has clean folder structure with exclude patterns and [Composer](/composer) support. [Laravel](/install-laravel) and [Symfony](/install-symfony) are poster-child-level for good Git support. [WordPress](/install-wordpress) and other CMS are not Git compatible, out-of-the-box (while good [hacks](install-wordpress-pro) are available). [Craft 3](/craft-3-deploy-git) works well with Git and Composer.

## Choosing a workflow

You can mix deployment methods, but not interchange the different deployment methods. Choose your main deployment method upfront.

Use the workflow that matches your skills and the requirements of the software you are using. In general we advice to work with Git, as it is: faster, more secure, it also includes an additional code backup with rollback and — with fortrabbit — Composer is also already integrated.

## Mixing deployment methods

It's not black and white either. When working with Git, you often will use some SSH/SFTP in addition — to deploy generated assets and manage user uploads for example. 
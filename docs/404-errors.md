---

template:      article
reviewed:      2020-10-30
title:         Troubleshoot 404 errors
naviTitle:     404 errors
excerpt:       Some tips on troubleshooting 404 errors.
lead:          'The 404 HTTP status code means "File Not Found". The server can be reached and is answering but there is nothing to show under this address. This article aims to help developers troubleshooting 404 errors.'
group:         troubleshooting
stack:         all
dontList:      false

---


## 404 error behavior

* 404 errors will usually be shown on screen immediately
* 404 errors often occur during setup or after code or configuration changes
* 404 errors here are often rendered using a fortrabbit error page template


## Common reasons and solutions

**In most cases this is not a server issue, but a problem with your code and configuration.** Please check the following common issues first:


### No code deployed

A common reason for the 404 page is that no code has been deployed yet. 

This can be a misunderstanding: You might have thought that when you choose a [Software Preset](/app#toc-software-preset) software will be installed. Sorry, no one-click-installs here, you need to install the software yourself. So please go ahead and deploy some code first.

Or maybe the deployment is not yet finished (SFTP is still uploading?) or your initial Git push returned an error. Please check that all code is deployed completely. With a Universal App you can use SSH/SFTP to login and see if the files are there.


### Wrong root path

Maybe your software is using a different root path than the one that is currently set? Check the [root path](/app#toc-root-path) settings.

`htdocs` is the default [root path](/app#toc-root-path) if no specific software has been chosen in the [Software Preset](#toc-software-preset). Now, if you decide to install [Laravel](install-laravel) or any other software later on, you might have to set the root path accordingly.

We have also seen cases where people have uploaded a whole `Craft` or `WordPress` folder into the App's `htdocs` folder. Better upload all files into `htdocs` directly, not into an extra folder that contains the files.


### .htaccess is missing

Another common cause for 404 errors is a missing `.htaccess` file. This file is hidden from your Operating System by default (as it starts with a period) but contains important rules for your application to function properly.
So if you are uploading with SFTP and have dragged the files from your Desktop (Finder) into your SFTP application (Cyberduck, Transmit, FileZilla), this file will likely be missing. 

You might be able to use the file explorer from your SFTP application or temporarily show hidden files in your OS to make the `.htaccess` file visible to you. Just make sure that when an `.htaccess` file is present (most likely it is), that it gets uploaded as well.


### Wrong address

You might have an error with URL. Check for typos in the address bar (URL) of your browser.


### App is not yet ready

Creating an App can sometimes take a few minutes. If you visit the App URL during that time, you'll get a 404 error. It's possible that this DNS response gets cached locally. 


## It could also be something on our side

It is also possible — although less likely - that this error is caused by a network, hardware or configuration issue on the side of your hosting provider — us. Please check our status page under [status.fortrabbit.com](https://status.fortrabbit.com) if there are any ongoing maintenance windows or service issues known.


## Contacting support

Whether your investigation (see above) leads to possible service issues on our side or you are stuck debugging the issue on your own, don't hesitate to contact us.

Please include logs and additional information with your support request. 

* <a href="#asd" onclick="Intercom('showNewMessage', 'I see 404 for my App ______ for around ___. I have made the following changes recently: ____. I have checked for the root path already.')">Contact support</a>
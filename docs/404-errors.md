---

template:      article
reviewed:      2020-10-19
title:         404 errors
naviTitle:     404 errors
excerpt:       What you may want to know about our 404 errors.
lead:          Are you seeing a 404 error page? 
group:         troubleshooting
stack:         all
dontList:      false

---

When your App is just sending a "404 file not found" page, please check this before contacting support:

#### No code deployed

A common case for the 404 page is that no code has been deployed yet. 

This can be a misunderstanding: You might have thought that choosing a [Software Preset](#toc-software-prest) software will be installed. Sorry, no one-click-installs here, you need to install the software yourself. So please go ahead and deploy some code first.

Maybe also, the deployment is not yet finished (SFTP is still uploading?) or your initial Git push returned an error? Please check if all code is deployed completely. With a Universal App you can use SSH/SFTP to login and see if the files are there.


#### Wrong root path

Maybe your software is using a different root path than the one that is currently set? Check the [root path](#toc-root-path) settings.

`htdocs` is the default [root path](#toc-root-path), when no specific software has been chosen in the [Software Preset](#toc-software-preset). Now, when you decide to install [Laravel](install-laravel) or any software later on, you might have to set the root path accordingly.

We have also seen cases where people have uploaded a whole `Craft` folder into the App's `htdocs` folder. You should probably better upload all files into `htdocs` directly, not into an extra folder that contains the files.


#### .htaccess is missing

Another common cause for 404 errors is a missing `.htaccess` file. This file is hidden from your Operating System by default (as it starts with a period) but contains important rules for your application to function properly.
So when you are uploading with SFTP and have maybe dragged the files from your Desktop (Finder) into your SFTP application (Cyberduck, Transmit, FileZilla), this file is likely missing. 

You might be able to use the file explorer from your SFTP application or temporarily show hidden files in your OS to make the `.htaccess` file visible to you. Just make sure that when an `.htaccess` file is present (most likely it is), that it gets uploaded as well.


#### App is not yet ready

Creating an App can sometimes take a few minutes. If you visit the App URL during that time, you'll get also get a 404 error. It's possible that this DNS response gets cached locally. 
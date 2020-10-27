---

template:      article
reviewed:      2020-10-20
title:         Troubleshoot 403 errors
naviTitle:     403 errors
excerpt:       What you may want to know about our 403 errors.
lead:          'The 403 HTTP status code is "Forbidden". This means that access is denied, or in other words: the resource is not allowed for some reason. This article aims to help developers troubleshooting such errors.'
group:         troubleshooting
stack:         all
dontList:      false

---

## 403 error behavior

* 403 errors will usually be shown on screen immediately
* 403 errors often occur after code or configuration changes
* 403 errors here are often rendered using a fortrabbit error page template


## Common reasons and solutions

**In most cases this is not a server issue, but something with your code and configuration.** Please check the following common issues first:



### No code deployed

The App is new. You have not yet deployed any code. Please upload some code from your local development environment.


### Wrong root path 

With certain CMS and frameworks we pre-populate the root path. So depending in your choice when creating the App, something might not be configured correctly. Check the [root path](/app#toc-root-path) of your App.


### .htaccess problems

Sometimes 403 errors are caused by issues with `.htaccess`. Check your `.htaccess` file. Developes using SFTP often miss to upload that hidden file. See our [.htaccess help](/htaccess).


### File permissions

 Sometimes 403 errors are happening due to wrong file permissions. This can happen when you deploy with SFTP or SSH. Make sure that PHP can write the files `composer.json`, `composer.lock`, `storage/*` and `vendor/*` and other files. Set the file permissions for those files to `744` with your SFTP client or by SSH.  The `*.php` files should be executable.

<!-- Duplicate with Craft CMS: Upload SFTP, maybe remove the other one -->


### Missing http-auth credentials

When you have used HTTP auth to protect your website from unwanted visitors and someone is trying access that without entering the correct username and password a 403 access denied error will occur as well.


### DNS issues

Sometimes it takes a while until DNS is propagated. This can be when your App is new, you have upgraded the scaling or you have just routed a domain. Wait a little and try again.


### Wrong address

You might have an error with URL. Check for typos in the address bar (URL) of your browser.


## It could also be something on our side

It is also possible — although less likely - that this error is caused by a network, hardware or configuration issue on our side, your hosting provider. Please check our status page under [status.fortrabbit.com](https://status.fortrabit.com) if there are any ongoing maintenance windows or service issues known.


## Contacting support

Whether your investigation (see above) leads to possible service issues at our side or you are stuck in debugging the issue on your own, don't hesitate to contact us.

Please include logs and additional information with your support request. 

* <a href="#asd" onclick="Intercom('showNewMessage', 'I see 403 for my App ______ for around ___. I have made the following changes recently: ____.')">Contact support</a>
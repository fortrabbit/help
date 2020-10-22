---

template:      article
reviewed:      2020-10-20
title:         403 errors
naviTitle:     403 errors
excerpt:       What you may want to know about our 403 errors.
lead:          'The 403 HTTP status code is "Forbidden". This means that access is denied. The resource is not allowed for some reason.'
group:         troubleshooting
stack:         all
dontList:      false

---


## Common reasons and solutions

In most cases this is not a server issue, but something with your code and configuration.


### No code deployed

The App is new. You have not yet deployed any code. Please upload some code from your local development environment.


### Wrong root path 

With certain CMS and frameworks we pre-populate the root path. So depending in your choice when creating the App, something might not be configured correctly. Check the [root path](/app#toc-root-path) of your App.


### .htaccess problems

Sometimes 403 errors are caused by issues with `.htaccess`. Check your `.htaccess` file, or maybe the file missing. See our [.htaccess help](/htaccess).


### File permissions

 Wrong permissions - Check your file permissions


### DNS issues

* DNS not propagated -  Wait a little and try again


### Wrong address

Check for typos in the address bar (URL)
* Something else - Please check the [fortrabbit status](https://status.forttabbit.com)

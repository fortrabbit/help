---

template:      article
reviewed:      2020-10-20
title:         403 errors
naviTitle:     403 errors
excerpt:       What you may want to know about our 403 errors.
lead:          'The 403 HTTP status code is "Forbidden". This means that access is denied. The resource is not allowed for you for some reason.'
group:         troubleshooting
stack:         all
dontList:      false

---


## Common reasons and solutions

In most cases this is not a server issue, but something with your code and configuration.

### No code deployed

Please push some code

### Wrong root path 

Check if the [root path](/app#toc-root-path) (doc root) is configured correctly

* Issues with `.htaccess`- Check your [`.htaccess` file](/htaccess), maybe it is missing
* Wrong permissions - Check your file permissions
* DNS not propagated -  Wait a little and try again
* Wrong address - Check for typos in the address bar (URL)
* Something else - Please check the [fortrabbit status](https://status.forttabbit.com)

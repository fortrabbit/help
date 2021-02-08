---

template:      article
reviewed:      2020-10-29
title:         Troubleshoot 503 errors
naviTitle:     503 errors
excerpt:       Some tips on troubleshooting 503 errors.
lead:          'The 503 HTTP status code means "Service Unavailable". This article aims to help developers troubleshooting 503 errors.'
group:         troubleshooting
stack:         all
dontList:      false

---


## 503 error behavior

* 503 errors will usually be shown on screen immediately
* 503 errors often occur after code or configuration changes
* 503 errors are sometimes just printed as a "service unavailable" message, check the browser’s developer tools for the response's HTTP status code to be sure this is indeed a 503 error
* 503 errors here are often rendered using a fortrabbit error page template


## Common reasons and solutions

**In most cases this is not a server issue, but something with your code and configuration.** Good news is that they are mostly easy to debug for you.


### Check the logs

Examine the logs of your App. See [here for Uni Apps](logging-uni) and [here for Pro Apps](logging-pro). There you'll likely find where the application exited with which error. Compare this with your local development environment and see if it fails the same way. Review your latest code changes.


### Review your latest changes

Since 500 errors often appear during installation, setup or code changes: Review your code and configuration changes. Compare this with your [local development environment](/local-development) and see if it fails the same way. Have you ran an update recently?


## It could also be something on our side

It is also possible — although less likely - that this error is caused by a network, hardware or configuration issue on our side, your hosting provider. Please check our status page under [status.fortrabbit.com](https://status.fortrabbit.com) if there are any ongoing maintenance windows or service issues known.


## Contacting support

Whether your investigation (see above) leads to possible service issues on our side or you are stuck debugging the issue on your own, don't hesitate to contact us.

Please include logs and additional information with your support request. 

* <a href="#asd" onclick="Intercom('showNewMessage', 'I see 502 for my App ______ for around ___. I have made the following changes recently: ____. Find attached the php_error log in question.')">Contact support</a>
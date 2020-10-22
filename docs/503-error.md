---

template:      article
reviewed:      2020-10-19
title:         503 errors
naviTitle:     503 errors
excerpt:       What you may want to know about our 503 errors.
lead:          'The 503 HTTP status code is "Service unavailable". This article aims to help developers troubleshooting such errors.'
group:         troubleshooting
stack:         all
dontList:      false

---


## 503 error behavior

* 503 errors will usually be shown on screen immediately
* 503 errors often occur after code or configuration changes
* 503 errors are sometimes just printed as a "service unavailable" message, check the HTTP in the browser to be sure this is indeed a 503 error
* 503 errors here are often rendered using a fortrabbit error page template


## Common reasons and solutions

In most cases 503 server errors are not a server issue, but something with your code and configuration. There are many different reasons for 5xx errors. Good news is that they are mostly easy to debug for you.


### Check the logs

Become a detective. Start the investigation by examining the logs of your App - see [here for Uni Apps](logging-uni) and [here for Pro Apps](logging-pro). Within the logs you will find where application exited with which error: This is the hot lead you are after. Compare this with your local development environment and see if it fails the same way. Review your latest code changes.


## It could also be something on our side

It is also possible — although less likely - that this error is caused by a network, hardware or configuration issue on our side, your hosting provider. Please check our status page under [status.fortrabbit.com](https://status.fortrabit.com) if there are any ongoing maintenance windows or service issues known.


## Contacting support

Please help us helping you. Have a look in the logs of your App to find out where the error exactly is. If that does not help you already, please include logs and additional information with your support request. 

* <a href="#asd" onclick="Intercom('showNewMessage', 'I see 404 for my App ______ for around ___. I have made the following changes recently: ____')">Contact support</a>
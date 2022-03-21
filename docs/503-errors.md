---

template:      article
reviewed:      2022-03-10
title:         Troubleshoot 503 errors
naviTitle:     503 errors
excerpt:       Some tips on troubleshooting 503 errors.
lead:          'The 503 HTTP status code means "Service Unavailable". This article aims to help developers troubleshooting 503 errors.'
group:         troubleshooting
stack:         all
dontList:      false
rank:          10
order:         15

---


## Typical symptoms of 503 errors

+ 503 errors will usually be shown on screen immediately
+ 503 errors often occur after code or configuration changes
+ 503 errors are sometimes just printed as a "service unavailable" message, check the browser's developer tools for the response's HTTP status code to be sure this is indeed a 503 error
+ 503 errors here are often rendered using a fortrabbit error page template

503 and [504 errors](/504-errors) are two sides of the same coin. PHP-FPM (the PHP process manager here) has too much to do. 503 and 504 errors are often happening simultaneously.

503 and 504 are two sides of the same coin. PHP-FPM has too much to do. Happening at the same time.

## Common reasons and solutions

**In most cases this is not a server issue, but something with your code and configuration.** Good news is that they are mostly easy to debug for you.

### Check the logs

Examine the logs of your App. See [here for Uni Apps](logging-uni) and [here for Pro Apps](logging-pro). There you'll likely find where the application exited with which error. Compare this with your local development environment and see if it fails the same way. Review your latest code changes.

### 'AH1067: Failed to read FastCGI header' Apache error

If you see the error "Failed to read FastCGI header" in the App's error log stream, there are these likely causes:

+ A bug in the App's PHP code, such as a buggy WordPress plugin
+ The App is using too much memory
+ A bug in a PHP extension you've enabled
+ When this error occurs, the response sent to the browser will be "503 Service Unavailable"

#### What the error means

The error message "Failed to read FastCGI header" means that while Apache was waiting for PHP-FPM, the PHP process did not respond properly. This error does not mean there is a problem with Apache.

### Restarting PHP (workaround)

In some cases, to the the App working again, it may be enough to restart the App from the Dashboard.

+ [Restarting an App](/app#toc-restarting-an-app)

If the issue is caused by buggy code, or slow queries, or 3rd party APIs, the problem is likely to happen again.

### Review your latest changes

Review your code and configuration changes. Compare this with your [local development environment](/local-development) and see if it fails the same way. Have you ran an update recently?

### Look for errors and slow endpoints

The most common cause of these errors is buggy PHP code. You can try disabling Craft CMS or WordPress plugins, custom themes, and other custom code until you identify which code is causing the problem.

If some or several scripts or endpoints of you App are slow, new connections will get backed up, ultimately resulting in PHP-FPM not being able to respond any more. If this is the case, then the solution is to optimize the slow PHP code.

### Buggy PHP extensions

In some cases, the problems may be caused by an extension that you have enabled, such as New Relic, Mongodb, or any other PECL extension. If you suspect this is the case, let us know in the support-chat.

### Running out of memory

If your App uses more memory than the Plan allows for, the App will crash every time the limit is exceeded. We configure our Apps with swap to avoid crashes, but if you see more than 10-30 MB of swap usage in the dashboard, then this is a strong indicator that your app is using too much memory.

#### Upgrade the plan

One solution is to upgrade your Plan, if possible. Otherwise, you wil have to profile the App to figure out why it's using so much memory.

## It could also be something on our side

It is also possible — although less likely - that this error is caused by a network, hardware or configuration issue on our side, your hosting provider. Please check our status page under [status.fortrabbit.com](https://status.fortrabbit.com) if there are any ongoing maintenance windows or service issues known.

## Contacting support

Whether your investigation (see above) leads to possible service issues on our side or you are stuck debugging the issue on your own, don't hesitate to contact us.

Please include logs and additional information with your support request.

+ <a href="#asd" onclick="Intercom('showNewMessage', 'I see 502 for my App ______ for around ___. I have made the following changes recently: ____. Find attached the php_error log in question.')">Contact support</a>

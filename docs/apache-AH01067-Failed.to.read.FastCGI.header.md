---

template:      article
reviewed:      2022-03-03
title:         Troubleshoot 503 errors
naviTitle:     Dealing with AH1067: Failed to read FastCGI header
excerpt:       Some tips on troubleshooting 503 / 504 errors.
lead:          'Troubleshooting apache errors (http 503 / 504)'
group:         troubleshooting
stack:         all
dontList:      true
rank:          0

---

<!--

add also:
AH01075: Error dispatching request to

-->
## How to Fix 'AH1067: Failed to read FastCGI header' Apache Error

If you see the error "Failed to read FastCGI header" in the App's error log stream, there are three most likely causes:

- A bug in the app's PHP code, such as a buggy WordPress plugin
- A bug in a PHP extension you've enabled
- The App is using too much memory
- When this error occurs, the response sent to the browser will be "503 Service Unavailable".

### What the Error Means

The error message "Failed to read FastCGI header" means that while Apache was waiting for PHP-FPM, the PHP process did not respond properly. This error does not mean there is a problem with Apache.

## Restarting PHP (workaround)

In some cases, to the the App working again, it may be enough to retart the App from the Dashboard.

- [Restarting an App](/app#toc-restarting-an-app)

If the issue is caused by buggy code, or slow queries, or 3rd party APIs, the problem is likely to happen again.


## General tips for dealing with 503 / 504

### Look for errors, slow endpoints and optimize the PHP code

The most common cause of these errors is buggy PHP code.

You can try disabling CraftCMS or WordPress plugins, custom themes, and other custom code until you identify which code is causing the problem.

<!-- should mention php-fpm slow-log too -->
You may find more information in the App's PHP error log.

If there are errors, it means that there is abug in the App's PHP code.

If some or several scripts or endpoints of you App are slow, new connections will get backed up, ultimately resulting in PHP-FPM not being able to respond any more. If this is the case, then the solution is to optimize the slow PHP code.

### Buggy PHP Extensions

In some cases, the problems may be caused by an extension that you have enabled, such as New Relic, Mongodb, or any other PECL extension. If you suspect this is the case, let us know in the support-chat.

### Running Out of memory

If your App uses more memory than the Plan allows for, the App will crash everytime the limit is exceeded. We configure our Apps with swap to avoid crashes, but if you see more than 10-30 MB of swap usage in the dashboard, then this is a strong indicator that your app is using too much memory.

One solution is to upgrade your Plan, if possible. Otherwise, you wil have to profile the App to figure out why it's using so much memory.


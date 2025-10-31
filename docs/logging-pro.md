---

template:   article
reviewed:   2025-10-31 08:31:59
title:      About logs on fortrabbit
naviTitle:  Logging
lead:       Accessing live logs of your App is essential for developing. Here is how you can do it on fortrabbit.
group:      development
stack:      pro

uniLink:    logging-uni

keywords:
    - Logging
    - Logs

rank: 20

---

You are developing your App and see the "white screen of death". You are getting a 5xx error and don't know why. You write debug logs and need them to trace a problem.


## Log access

Use the SSH logging [SSH remote command](remote-ssh-execution) in the terminal to get a live streams of all the logs for your Apps:

```bash
# All sources tailed together:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail

# Only Apache access logs:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_access
# incoming requests with response status, time-stamp, additional headers & first line of request

# Only Apache error log:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_error
# helpful to debug `.htaccess` files and alike

# Only Object Storage access log:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:objects_access
# See requests when and from whom

# Only PHP error logs:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_php_error
# contains whatever your App writes on `error_log()`

# Only web standard error output:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_stderr
# contains everything written by your App to `STDERR`


# Only Cron Job or Nonstop Job output:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:worker
# Worker must be enabled

# --------------- PRO TIPS ---------------

# Use the mono flag to force monochrome output, if your console displays colors incorrectly:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail {{app-name}} mono

# Use multiple source:name parameters at once:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_stderr source:web_php_error

# Filter by HTTP status code, e.g. 4xx OR 3xx
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_access 2>&1 | grep -E ' 40.?| 30.?'
```

## Quirks

Unlike with [Universal Apps](/logging-uni), there are no historical oder logs for Pro Apps available. You might use a third party service like Papertrail to achieve that.
---

template:      article
reviewed:      2023-11-15 15:55:06
title:         MySQL troubleshooting
naviTitle:     MySQL troubleshooting
excerpt:       Some tips on troubleshooting MySQL connections.
lead:          'This article aims to help you finding common issues with your MySQL database on fortrabbit.'
group:         mysql
stack:         all
dontList:      false
order:         10

---

## Can't connect from local

The most common misunderstanding when trying to connect from a local machine, is that people overlook to first open up the SSH tunnel and then connect to the database. Graphical MySQL clients support this connection method out of the box. 

You'll need to **enter both: SSH access and MySQL access details**. Within the fortrabbit Dashboard under your App > Access, there is a small link labeled: "Show SSH tunnel info" which will reveal everything you'll need to enter in a MySQL client to connect to the remote database.

## max_user_connections error

You'll see a `max_user_connections` error when you've reached the max connection limit. 

```
SQLSTATE[HY000] [1226] User 'xxxxxx' has exceeded the 'max_user_connections' resource (current value: 10)
```

The limit is defined by the App plan. We give a few more MySQL concurrent connections than allowed PHP-FPM concurrent requests.

### Typical causes

- too many concurrent connections from the web
- every requests starts multiple connections to MySQL
- a background cron job or worker keeps connections open for a long time
- lingering queries that run for a long time
- MySQL GUI client opens too many connections (see below)

### What to do about it

Figure out what’s causing connections to linger → eliminate those causes.

### GUI clients eating MySQL connections

You are trying to connect to the database with a MySQL GUI, like Navicat, Workbench or Sequel Ace? Some those clients are "eating" MySQL connections like popcorn. With fortrabbit, the MySQL connections and the PHP processes are limited. If you use up all the allotted connections, it can take a little until the App recovers. If this turns out to be a problem for you, look into limiting the number of concurrent connections from your MySQL gui or the App itself.

## Access denied; missing SUPER privileges

When importing a database dump you've created with a recent version of `mysqldump`, you may experience errors like this:

```
#1227 - Access denied; you need (at least one of) the SUPER privilege(s) for this operation
```

To prevent this error, create the dump again using the `--set-gtid-purged=OFF` option. If you don't use the `mysqldump` command line tool directly, but a GUI that relies on it, the chance is very high there is a checkbox to disable "GTID PURGED".

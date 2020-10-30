---

template:      article
reviewed:      2020-10-29
title:         MySQL Troubleshooting
naviTitle:     MySQL Troubleshooting
excerpt:       Some tips on troubleshooting MySQL connections.
lead:          'This article aims to help you finding common issues when connecting to your MySQL database on fortrabbit.'
group:         troubleshooting
stack:         all
dontList:      false

---

## Can't connect from local

The most common misunderstanding when trying to connect from a local machine, is that people overlook to first open up the SSH tunnel and then connect to the database. Graphical MySQL clients support this connection method out of the box. 

You'll need to **enter both: SSH access and MySQL access details**. Within the fortrabbit Dashboard under your App > Access, there is a small link labeled: "Show SSH tunnel info" which will reveal everything you'll need to enter in a MySQL client to connect to the remote database.


## max_user_connections

You'll see a `max_user_connections` error when you've reached the max connection limit of your current MySQL plan. Most likely you are trying to connect to the database with a MySQL GUI, like Navicat, Workbench or Sequel Pro. Some those clients are "eating" MySQL connections like popcorn. With fortrabbit, the MySQL connections and the PHP processes are limited. If you use up all the allotted connections, it can take a little until the App recovers. If this turns out to be a problem for you, look into limiting the number of concurrent connections from your MySQL gui or the App iteslf.

If you see that error on other occasions or it's not going away after a while, contact support.

## Access denied; missing SUPER privileges

When importing a database dump you've created with a recent version of `mysqldump`, you may experience errors like this:

```
#1227 - Access denied; you need (at least one of) the SUPER privilege(s) for this operation
```

To prevent this error, create the dump again using the `--set-gtid-purged=OFF` option. If you don't use the `mysqldump` command line tool directly, but a GUI that relies on it, the chance is very high there is a checkbox to disable "GTID PURGED".   

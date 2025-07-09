---

template:      article
reviewed:      2025-07-09 14:42:02
title:         Advanced MySQL topics
naviTitle:     Advanced MySQL topics
lead:          How to access & configure the common database on fortrabbit.
group:         mysql
proGroup:      Components
stack:         all
rank:          1
order:         5

keywords:
     - localhost
     - mysqldump
     - dump
     - mysql
     - database
     - innodb
     - myisam
     - phpmyadmin
     - heidisql
     - workbench
     - sequel-pro
     - sequel
     - tunnel
     - nosql

---

## Different time zone

MySQL has [time zone support](http://dev.mysql.com/doc/refman/5.5/en/time-zone-support.html). Our Nodes are defaulting to the standard time zone UTC+00 (aka GMT). If you want to change this time zone, you can do so on a "per connection" basis.

There are two approaches to tackle this issue: handle the time zone on application level or handle the time zone on database level. Each has its merits and which one is better strongly depends on the use case.

### Setting time zone in plain (My)SQL

The syntax to change the time zone is:

```sql
-- set time zone to +3 hours
SET time_zone = '+03:00';

-- set time zone to -7 hours
SET time_zone = '-07:00';
```

You can query the current time zone like so:

```sql
SELECT @@session.time_zone;
```

### Setting time zone with PDO

`PDO` offers configurable options when setting up the connection. One of them allows you to issue commands right after initialization (connection).

```php
$pdo = new PDO(
    'mysql:host='. getenv('MYSQL_HOST'). ';dbname='. getenv('MYSQL_DATABASE'),
    getenv('MYSQL_USER'),
    getenv('MYSQL_PASSWORD'),
    array(
        PDO::MYSQL_ATTR_INIT_COMMAND => 'SET time_zone = \'+02:00\''
    )
);
```

## Resetting the MySQL password

Instead of [looking up the existing MySQL password](#toc-obtain-the-mysql-password), you can also reset it. Do so in the Dashboard > Your App > Access > MySQL. Please mind that this comes with consequences:

* Unless your are using [env vars](env-vars): You'll need to change the password in your App's configuration files
* Your coworkers need to change their password in their locally configured remote access tools (see below)

## Accessing MySQL from a different App

It is possible to access a MySQL from another App within the same region (EU, US). The database from `App-A` can be accessed from `App-B`. This works since both Apps are within the same network. You will only need to update the MySQL access credentials to do so. There are not many good use cases for this, but it might be good to know.

## Accessing the remote MySQL from your local App

It is also possible to access the fortrabbit database from the local installation of your App. You will need to open a tunnel, as described above to do so. While possible we do not recommend that approach. You local App will feel slow, unless you have a very good internet connection. Further, you will run the risk of messing up the database if several apps are writing to it at the same time. The best practice here is separating the local development environment from production.

## Differences between Professional and Universal

All [Universal Apps](/app-uni) automatically come with a MySQL database. For [Professional Apps](app-pro), MySQL is an optional Component. There you can [scale](scaling#toc-mysql) it up and down individually.

## MySQL limits

Each App has one database named like the App. The mysql-user you have received is not granted the privileges to `CREATE DATABASE`. Please mind that `CREATE SCHEMA` requires the same permission.

## Using MySQL functions, procedures and triggers

You don't have permissions to create MySQL functions, procedures and triggers as it requires SUPER privileges.

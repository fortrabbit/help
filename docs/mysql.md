---

template:      article
reviewed:      2021-10-23
title:         All about MySQL
naviTitle:     MySQL
lead:          How to access & configure the common database on fortrabbit.
group:         platform
proGroup:      Components

stack:         all

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



PHP + MySQL is a classic.

## Obtain the MySQL password

You can look up the MySQL password in the Dashboard > Your App > Access.

<div markdown="1" data-user="known">
[Look up the MySQL password for **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}#tab-mysql)
</div>


## Access MySQL from your App

Usually there is a configuration file which is used from the App to connect to the fortrabbit database. This is what you need to fill in there:

* **Database Name**: `{{app-name}}`
* **Database Username**: `{{app-name}}`
* **Database Password**: `{{app-database-password}}` _[< how to recover](#toc-obtain-the-mysql-password)_
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com` _< not localhost_
* **Database Port**: `3306` (usually not required)

If possible, we recommend to use [MySQL env vars](env-vars) instead of hard coding them into your configuration files. The automatically available environment variables are:

* `MYSQL_DATABASE`: Database name
* `MYSQL_USER`: Database username
* `MYSQL_PASSWORD`: Database password
* `MYSQL_HOST`: Database host
* `MYSQL_PORT`: Database port

To give you an example on how to use the environment variables:

```php
$pdo = new \PDO(
    'mysql:host='. getenv('MYSQL_HOST'). ';dbname='. getenv('MYSQL_DATABASE'),
    getenv('MYSQL_USER'),
    getenv('MYSQL_PASSWORD'),
);
$pdo->query("SELECT * FROM ...")
```

That is only a generic example. See our specific guides for: [Laravel](install-laravel-uni#toc-mysql), [Symfony](install-symfony-6#toc-mysql), [WordPress](install-wordpress-uni#toc-mysql), [Craft CMS](craft-3-tune) which are often using zero-config style environment variables.

## Access the MySQL database from local

Whether you want to run a query on your live database or you want to dump your whole database: you need to access the MySQL database on fortrabbit remotely. For security reasons you cannot connect to the MySQL database from "outside" directly, but you can open a [SSH tunnel](http://en.wikipedia.org/wiki/Tunneling_protocol) and then connect to the MySQL database through this tunnel.

If you haven't: you need to [obtain your MySQL password](/#toc-obtain-the-mysql-password). Next you can decide upon using a graphical user interface or the terminal:


### MySQL via terminal

To set up a tunnel from port 13306 on localhost to the mysql-database on 3306, use the following command;

```bash
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@tunnel.{{region}}.frbit.com
```

Port local 13306 is just an example, any port in the range 1025-65535 can be used. The remote port 3306 must be 3306.
This command is not supposed to print a confirmation message. If nothing shows up: you did it right!

Once the tunnel is up, you can connect to the remote MySQL database with the `mysql` console client. Open a **new window terminal window** and issue this on your **local machine**:

```bash
# connect to the database < use 127.0.0.1, not localhost
$ mysql -u{{app-name}} -h127.0.0.1 -P13306 -p -D {{app-name}}
```

In the next step you will be asked for your [MySQL password](#toc-obtain-the-mysql-password).


### MySQL via GUI

We recommend the free [MySQL Workbench](http://www.mysql.com/products/workbench/) (Mac/Linux/Windows). There is also [Navicat](http://www.navicat.com/products/navicat-for-mysql) (also multi-platform), [HeidiSQL](http://www.heidisql.com/) for Windows and [Sequel Ace](https://github.com/Sequel-Ace/Sequel-Ace) for Mac, along with [at least a few others](https://www.google.com/search?q=mysql%20gui).

The clients above can be configured to help you to connect via an SSH tunnel to the MySQL database in one convenient step. In the connection dialog or tab of the client you pick, you will need to insert the correct parameters for SSH and MySQL.

To give you an idea of how the access details should be configured, here an example using MySQL Workbench:

* Create a new connection with *Connection Method* set to `Standard TCP/IP over SSH`, then:
* **SSH Hostname**: `tunnel.{{region}}.frbit.com`
* **SSH Username**: `{{ssh-user}}`
* **SSH Password**: `{{ssh-password}}`
* **SSH Keyfile**: <code data-with-password>No need</code><code data-without-password>Your local SSH private key</code>
* **MySQL Hostname**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **MySQL Server Port**: `3306`
* **Username**: `{{app-name}}`
* **Password**: [Look it up in the Dashboard](#toc-obtain-the-mysql-password)
* **Default Schema**: `{{app-name}}`

**Note**: The MySQL hostname will not be `127.0.0.1` or `localhost` — it's the remote server:
`{{app-name}}.mysql.{{region}}.frbit.com`.


### phpMyAdmin

For security and practical reasons we consider it bad practice to install phpMyAdmin on your fortrabbit App. However, you can also manage the remote MySQL with a **local phpMyAdmin installation**. Add an additional server configuration to your local phpMyAdmin `config.inc.php` file like so:

```
$cfg['Servers'][$i]['verbose']       = '{{app-name}}';
$cfg['Servers'][$i]['host']          = '127.0.0.1';
$cfg['Servers'][$i]['port']          = '13306'; // like specified in the tunnel command (see below)
$cfg['Servers'][$i]['connect_type']  = 'tcp';
$cfg['Servers'][$i]['extension']     = 'mysqli';
$cfg['Servers'][$i]['compress']      = FALSE;
$cfg['Servers'][$i]['auth_type']     = 'cookie';
$i++;
```

Then open a [terminal tunnel](#toc-mysql-via-terminal), then visit your local phpMyAdmin in the browser. You now can select your fortrabbit App. You will be asked for the MySQL user "**{{app-name}}**" and [password](#toc-obtain-the-mysql-password). Using a local phpMyAdmin with your remote database requires you to always open a tunnel first - a [MySQL GUI](#toc-mysql-via-gui) might be the better choice.


## Export & import

To set up an application environment, you may need to migrate existing data. The following examples deal with exporting and importing data from one environment (local machine) to another (fortrabbit). The same steps apply in reverse, but the to and from login details need to be swapped.


### Using the terminal

Using `mysqldump` and `mysql` is the standard approach to migrate a database between two MySQL servers from the shell. First of start by exporting your data from your local database. Do this on your local machine:

```bash
# on your local machine or on the old server
$ mysqldump --set-gtid-purged=OFF -u{{local-db-user}} -p{{local-password}} {{local-db-name}} > dump.sql
``` 

The `--set-gtid-purged=OFF` option is required to prevent permission errors like `#1227 - Access denied; you need (...) SUPER privilege(s)` when importing the database. If you use `mariaDB` locally the GTID option is needed.

Next, open a tunnel and import the newly created dump file into your database. This is easier with two terminal windows: One for the tunnel and the other to execute the import. These instructions must be run on your **local machine**, the import will not work if you log in to fortrabbit before running these commands.

open the tunnel in the first terminal

```bash
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@tunnel.{{region}}.frbit.com
```

open a new terminal and run the mysql import

```
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < dump.sql
```

### Using MySQL Workbench (GUI)

**Export from local**:

1. Open Workbench
2. Setup your local database connection
3. Open your local database connection
4. Choose: Server > Data Export from the menu
5. Select your local database name
6. Make sure to "Dump Structure and Data" (select below the database name listing)
7. Choose a local destination file
8. Start the export

**Import to fortrabbit**:

1. Open Workbench
2. Create a new connection as [shown above](#toc-mysql-via-gui)
2. Open the newly created remote database connection
3. Choose: Server > Data Import from the menu
4. Choose your previously generated dump file
5. Make sure to select your App name in the *Default Target Schema*
6. Start the import


### MySQL LOAD DATA

You can export and import a large, single table with the following example:

```bash
# on your local machine or on the old server
$ echo 'SELECT * FROM tablename;' | mysql database-name > tablename.sql

# import everything via a tunnel to yourfortrabbit MySQL database
$ mysql --local-infile=1 -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}}

# on the mysql shell
$ mysql> LOAD DATA LOCAL INFILE '/path/to/tablename.sql' INTO TABLE tablename;
```

**Note**: You will be asked to enter your App's database password. [Look it up in the Dashboard](https://dashboard.fortrabbit.com/apps/{{app-name}}#mysql).


### Foreign keys

In some cases — like when importing our [MySQL backups](/backups) — you might need to temporarily disable foreign key constraints first, before importing the database. Just run this MySQL query, after connecting in to the database before actually doing the import:

```mysql
SET FOREIGN_KEY_CHECKS=0;
```

In a GUI like Sequel Ace there is a query window to run it. This value will then be reset to the default value when you close the connection.


## Local MySQL

This article describes how to deal with the fortrabbit remote MySQL database. You might want to have a local one as well. Please see our [local development article](/local-development).


## Advanced usage


### Different time zone

MySQL has [time zone support](http://dev.mysql.com/doc/refman/5.5/en/time-zone-support.html). Our Nodes are defaulting to the standard time zone UTC+00 (aka GMT). If you want to change this time zone, you can do so on a "per connection" basis.

There are two approaches to tackle this issue: handle the time zone on application level or handle the time zone on database level. Each has its merits and which one is better strongly depends on the use case.


#### Setting time zone in plain (My)SQL

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


#### Setting time zone with PDO

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


### Resetting the MySQL password

Instead of [looking up the existing MySQL password](#toc-obtain-the-mysql-password), you can also reset it. Do so in the Dashboard > Your App > Access > MySQL. Please mind that this comes with consequences:

* Unless your are using [env vars](env-vars): You'll need to change the password in your App's configuration files
* Your coworkers need to change their password in their locally configured remote access tools (see below)


### Accessing MySQL from a different App

It is possible to access a MySQL from another App within the same region (EU, US). The database from `App-A` can be accessed from `App-B`. This works since both Apps are within the same network. You will only need to update the MySQL access credentials to do so. There are not many good use cases for this, but it might be good to know.


### Accessing the remote MySQL from your local App

It is also possible to access the fortrabbit database from the local installation of your App. You will need to open a tunnel, as described above to do so. While possible we do not recommend that approach. You local App will feel slow, unless you have a very good internet connection. Further, you will run the risk of messing up the database if several apps are wrinting to it at the same time. The best practice here is separating the local development environment from production.


### Differences between Professional and Universal

All [Universal Apps](/app-uni) automatically come with a MySQL database. For [Professional Apps](app-pro), MySQL is an optional Component. There you can [scale](scaling#toc-mysql) it up and down individually.




## Limits

Each App has one database named like the App. The mysql-user you have received is not granted the privileges to `CREATE DATABASE`. Please mind that `CREATE SCHEMA` requires the same permission.


### Using MySQL functions, procedures and triggers

By default you don't have permissions to create MySQL functions, procedures and triggers as it requires SUPER privileges.
We enable it on request for dedicated MySQL plans (Pro Stack only).


## Troubleshooting MySQL

Please see [here](/mysql-troubleshooting)
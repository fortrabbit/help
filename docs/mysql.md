---

template:      article
reviewed:      2019-09-28
title:         All about MySQL
naviTitle:     MySQL
lead:          PHP + MySQL is a classic. Access & configure the common database on fortrabbit.
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

That is only a generic example. See our specific guides for: [Laravel](install-laravel-uni#toc-mysql), [Symfony](install-symfony-uni#toc-mysql), [WordPress](install-wordpress-uni#toc-mysql), [Craft CMS](craft-3-tuning) which are often using zero-config style environment variables.


## Access the MySQL database from local

Whether you want to run a query on your live database or you want to dump your whole database: you need to access the MySQL database on fortrabbit remotely. For security reasons you cannot connect to the MySQL database from "outside" directly, but you can open a [SSH tunnel](http://en.wikipedia.org/wiki/Tunneling_protocol) and then connect to the MySQL database through this tunnel.

If you haven't: you need to [obtain your MySQL password](/#toc-obtain-the-mysql-password). Next you can decide upon using a graphical user interface or the terminal:


### MySQL via terminal

If you are familiar with the shell then this is no biggie. Issue this in your **local** terminal:

```bash
# open a tunnel on local port 13306 < arbitrary, choose between 10000-65000
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@tunnel.{{region}}.frbit.com
```

**This command will not reply with any message on success! If nothing shows up: you did right!** This behavior is how SSH clients are implemented and sadly we cannot issue any positive response message.

Once the tunnel is up, you can connect to the remote MySQL database with the `mysql` console client. Open a **new window terminal window** and issue this on your **local machine**:

```bash
# connect to the database < use 127.0.0.1, not localhost
$ mysql -u{{app-name}} -h127.0.0.1 -P13306 -p {{app-name}}
```

In the next step you will be asked for your [MySQL password](#toc-obtain-the-mysql-password).

### MySQL via GUI

We recommend the free [MySQL Workbench](http://www.mysql.com/products/workbench/) (Mac/Linux/Windows). There is also [Navicat](http://www.navicat.com/products/navicat-for-mysql) (also multi-platform), [HeidiSQL](http://www.heidisql.com/) for Windows and [Sequel Pro](http://www.sequelpro.com/) for Mac. And a [host of others](https://www.google.com/search?q=mysql%20gui).

All clients have connection presets that help you to establish the SSH tunnel and the MySQL connection in one convenient step. In the connection info you will insert all the SSH access information and the MySQL connection information.

To give you an idea of how the access details should be inserted, here an example using MySQL Workbench:

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

A common task is to move your MySQL data around, like when you are migrating to fortrabbit or you are about to set up a staging environment. All following examples show you how to export data from your local machine and import it into your App's database on fortrabbit. It works the same, with swapped login details, for the other way around.

### Using the terminal

Using `mysqldump` and `mysql` is the standard approach to migrate a database between two MySQL servers from the shell. First of start by exporting your data from your local database. Do this on your local machine:

```bash
# on your local machine or on the old server
$ mysqldump -u{{your-local-db-user}} -p{{your-local-db-password}} {{your-local-db-name}} > dump.sql
```

Replace the placeholders with your local credentials. 

Next, open a tunnel and import the just created dump file into your database. This requires two terminal windows: One containing the open tunnel, the other to execute the import. Do this on your **local machine**, please don't login via SSH before, run it locally:

```bash
# open the tunnel
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@tunnel.{{region}}.frbit.com

# !!! in a new terminal window !!!
# import the dump
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < dump.sql
```

If you see a connection error with the first command, please troubleshoot your connection. 

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

### LOAD DATA

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

In a GUI like Sequel Pro there is a query window to run it. This value will then be reset to the default value when you close the connection.


## Local MySQL

This article describes how to deal with the fortrabbit remote MySQL database. You might want to have a local one as well. Please see our [local development article](/local-development).

- - -

## Advanced usage

### Different time zone

MySQL has [time zone support](http://dev.mysql.com/doc/refman/5.5/en/time-zone-support.html) Our nodes default to the standard time zone "UTC". If you want to change this time zone, you can do so on a "per connection" basis.

There are two approaches to tackle this issue: handle the time zone on application level or handle the time zone on database level. Each has its merits and which one is better strongly depends on the use case. This article shows you how to set the time zone in the database.

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



### Differences between Professional and Universal

All [Universal Apps](/app-uni) automatically come with a MySQL database. For [Professional Apps](app-pro), MySQL is an optional Component. There you can [scale](scaling#toc-mysql) it up and down individually.

### Limits

Each App has one database named like the App. There are no privileges to `CREATE DATABASE`. Please mind that `CREATE SCHEMA` requires the same permission.

## Troubleshooting MySQL

### Can't connect from local

The most common misunderstanding when trying to connect from a local machine, is that people overlook to first open up the SSH tunnel and then connect to the database. Graphical MySQL clients support this connection method out of the box. You'll need to enter both: SSH access and MySQL access details. Within the fortrabbit Dashboard under your App > Access, there is a small link labeled: "Show SSH tunnel info" which will reveal everything you'll need to enter in a MySQL client to connect to the remote database.

### max_user_connections

You'll see a `max_user_connections` error when you've reached the max connection limit of your current MySQL plan. Most likely you are trying to connect to the database with a MySQL GUI, like Navicat, Workbench or Sequel Pro. Some those clients are "eating" MySQL connections like popcorn. With fortrabbit, the MySQL connections and the PHP processes are balanced and therefore kept on a low level, to force best practices and improve security. Once the connection are eaten up, it can take a little until the App recovers, auto-heals itself. There might be a setting with the client to limit the connections, or you'll try the command line tools as an alternative.

If you see that error on other occasions or it's not going away after a while, contact support.


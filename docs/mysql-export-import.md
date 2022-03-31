---

template:      article
reviewed:      2022-03-21 17:27:46
title:         How to export and import a MySQL database
naviTitle:     MySQL export & import
lead:          To set up an application environment, you may need to migrate existing data. The following examples deal with exporting and importing MySQL data from one environment (local machine) to another (fortrabbit). The same steps apply in reverse, but the to and from login details need to be swapped.
group:         mysql
proGroup:      mysql
stack:         all
rank:          1
order:         2

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


## Get ready

Make sure to be able to access the fortrabbit MySQL database, see the [MySQL access article](/mysql). Craft CMS users may want to look into using [Craft Copy](/craft-3-deploy-craft-copy) that helps with that.

## MySQL export import on the command line

Using `mysqldump` and `mysql` is the standard approach to migrate a database between two MySQL servers from the shell. First of start by exporting your data from your local database. Do this on your local machine:

```bash
# on your local machine or on the old server
$ mysqldump --set-gtid-purged=OFF -u{{local-db-user}} -p{{local-password}} {{local-db-name}} > dump.sql
```

The `--set-gtid-purged=OFF` option is required to prevent permission errors like `#1227 - Access denied; you need (...) SUPER privilege(s)` when importing the database. If you use `mariaDB` locally the GTID option is needed.

Next, open a tunnel and import the newly created dump file into your database. This is easier with two terminal windows: One for the tunnel and the other to execute the import. These instructions must be run on your **local machine**, the import will not work if you log in to fortrabbit before running these commands.

open the tunnel in the first terminal

```bash
ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@tunnel.{{region}}.frbit.com
```

open a new terminal and run the mysql import

```bash
mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < dump.sql
```

## MySQL export import using MySQL Workbench (GUI)

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
3. Open the newly created remote database connection
4. Choose: Server > Data Import from the menu
5. Choose your previously generated dump file
6. Make sure to select your App name in the *Default Target Schema*
7. Start the import

## MySQL LOAD DATA

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

## Foreign keys

In some cases — like when importing our [MySQL backups](/backups) — you might need to temporarily disable foreign key constraints first, before importing the database. Just run this MySQL query, after connecting in to the database before actually doing the import:

```mysql
SET FOREIGN_KEY_CHECKS=0;
```

In a GUI like Sequel Ace there is a query window to run it. This value will then be reset to the default value when you close the connection.
---

template:      article
reviewed:      2022-03-21 17:27:46
title:         All about MySQL
naviTitle:     MySQL access
lead:          How to access & configure the common database on fortrabbit.
group:         mysql
proGroup:      Components
stack:         all
rank:          1
order:         1

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

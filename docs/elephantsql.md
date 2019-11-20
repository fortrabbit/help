---

template:         article
naviTitle:        ElephantSQL
reviewed:         2019-09-27
title:            Using ElephantSQL with fortrabbit
stack:            all

group:            Databases
section:          Extending_fortrabbit

websiteLink:      http://www.elephantsql.com?utm_source=fortrabbit
websiteLinkText:  elephantsql.com
category:         databases
dataCenters:      US, EU
company:          84codes AB
image:            elephantsql-mark.png

keywords:
     - PostgreSQL-as-a-Service
     - RDBMS
     - relational database

---


## About PostgreSQL

PostgreSQL is an extensible open-source object-relational database management system (RDBMS). It's not new and sexy, but tried and tested as the initial release was in 1996. It's sometimes a better choice than [MySQL](/mysql). PostgreSQL boasts many characteristics designed to support high-transaction, mission-critical applications. Example scenarios are: in-database custom procedures, complex or edge case queries (like geo data handling).


## About ElephantSQL

ElephantSQL is a hosted PostgreSQL service provided from "84codes AB" from Sweden. They have been into this for a long time. Service features are: followers, point in time recovery, forks, backups.


## Pricing

It starts with a free plan. You can scale by the amount of data storage, concurrent connections, CPU power and dedicated memory. See the [pricing & plans page](http://www.elephantsql.com/plans.html?utm_source=fortrabbit).


## Signing Up

You can sign up with just your e-mail (double-opt in) or your GitHub/Google account.


## Booking

Once you're logged click the "+ Create" button on the right side, which will lead you to the new instance dialog. We recommend to use `frbit-your-app` as the Name. Depending on where your fortrabbit App runs choose the right Data Center:

* Europe: Choose `Amazon Web Service` > `EU-West-1 (Ireland)`
* USA: Choose `Amazon Web Service` > `US-East-1 (Northern Virginia)`

Since you can scale later on at any point, we recommend to choose a plan fitting your current needs. The plans page is linked from there if you are unsure how much you need.

## Connecting

In the instances list of the ElephantSQL console click on the "Details" button of your just created database. Now open the fortrabbit Dashboard in another tab, navigate to Your App > Settings > App secrets and insert the ElephantSQL credentials as your App's [secrets](secrets):

```plain
# The "Hostname" from the ElephantSQL details
ELEPHANT_SQL_HOST=something-01.db.elephantsql.com
ELEPHANT_SQL_PORT=5432

# The "Database name" from the ElephantSQL details
ELEPHANT_SQL_DATABASE=acbd123

# The "Username" from the ElephantSQL details
ELEPHANT_SQL_USER=acbd123

# The "Password" from the ElephantSQL details
ELEPHANT_SQL_PASSWORD=acbd123
```

To use ElephantSQL from your fortrabbit App you need to do one more thing:

## Enabling the PHP extension

Head over to the fortrabbit Dashboard, navigate to your App > Settings > PHP, scroll down to Drivers and enable `pgsql` extension. Save.

## Using ElephantSQL

Once the extension (above) is activated, PHP using [PDO](http://php.net/manual/en/ref.pdo-pgsql.php) supports PostgreSQL natively. So you can just:

```php
$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
$dbh = new \PDO(
    "pgsql:dbname={$secrets['CUSTOM']['ELEPHANT_SQL_DATABASE']};host={$secrets['CUSTOM']['ELEPHANT_SQL_HOST']}",
    $secrets['CUSTOM']['ELEPHANT_SQL_USER'],
    $secrets['CUSTOM']['ELEPHANT_SQL_PASSWORD']
);
$stmt = $dbh->query("SELECT * FROM table");
print_r($stmt->fetchAll(\PDO::FETCH_ASSOC));
```

Also check out the [install guides](/#install-guides) and find out how to integrate ElephantSQL with your favorite framework or CMS.

## Further reading

* [PostgreSQL on Wikipedia](https://en.wikipedia.org/wiki/PostgreSQL)
* [Official PostgreSQL website](http://www.postgresql.org/)
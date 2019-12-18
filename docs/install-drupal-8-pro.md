---

template:         article
reviewed:         2017-01-20
title:            Install Drupal 8
naviTitle:        Drupal
lead:             Drupal 8 is one of the best known open source PHP CMS. Learn here how to use it with fortrabbit.
group:            Install_guides
stack:            pro
uniLink:          install-drupal-8-uni

websiteLink:      https://www.drupal.org/?utm_source=fortrabbit
websiteLinkText:  drupal.org
category:         CMS
image:            drupal8-mark.png
version:          8.1
deprecated:       yes
dontList:         true
dontIndex:        true
supportLevel:     c


keywords:
    - drupal
    - install

---

## Get ready

We assume you've already created an App with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


### Set the Apps root path

If you haven't already (the Software Preset does that for you) — in the Dashboard: [Set the root path](/app#toc-root-path) of your App's domains to **web**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


## Quick start

Execute the following in your local terminal to initialize a new [composer-based Drupal](https://github.com/drupal-composer/drupal-project).

```bash
# 1. Create a new Drupal project locally with Composer:
$ composer create-project drupal-composer/drupal-project:8.x-dev --stability dev --no-interaction {{app-name}}

# 2. Go into the project folder
$ cd {{app-name}}

# 3. Finish the local installation
$ php vendor/bin/drupal site:install --root=web
# Alternatively visit your localhost in the browser
# to use the configuration wizard
```

### MySQL

Create a new file `web/sites/default/settings_prod.php` with the following contents:

```php
// Detect environment: Only triggered on fortrabbit
if (!getenv('APP_NAME')) {
    return;
}

// Configure database
$databases['default']['default'] = [
    'database'  => getenv('MYSQL_DATABASE'),
    'username'  => getenv('MYSQL_USER'),
    'password'  => getenv('MYSQL_PASSWORD'),
    'prefix'    => '',
    'host'      => getenv('MYSQL_HOST'),
    'port'      => getenv('MYSQL_PORT'),
    'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
    'driver'    => 'mysql',
];
```

Now open up `web/sites/default/settings.php`. At the bottom of the file include the just created production file:

```php
// other code …
include __DIR__ . '/settings_prod.php';
```

See [below](#toc-mysql-import) on how to export/import your MySQL database.


### Configure deployment on fortrabbit

Next, create the [deployment file](/deployment-file-v2) `fortrabbit.yml` with the following contents and place it top level of your local project.

```yaml
sustained:
    - vendor
    - web/core
    - web/modules/contrib
    - web/themes/contrib
    - web/profiles/contrib
```


### Object Storage

fortrabbit Apps have an [ephemeral storage](/quirks#toc-ephemeral-storage). If you require a persistent storage, for user uploads or any other runtime data your App creates, you can use our [Object Storage Component](/object-storage). Once you have booked the Component in the Dashboard the credentials will become available via the [App secrets](/secrets).

Open up `web/sites/default/settings_prod.php` again and add at the bottom:

``` php
// enable object storage
$schemes = [
    'object-storage' => [
        'driver' => 's3',
        'config' => [
            'key'      => getenv('OBJECT_STORAGE_KEY'),
            'secret'   => getenv('OBJECT_STORAGE_SECRET'),
            'region'   => getenv('OBJECT_STORAGE_REGION'),
            'bucket'   => getenv('OBJECT_STORAGE_BUCKET'),
            'options'  => [],
            'protocol' => 'https',
            'prefix'   => '',
            'cname'    => getenv('OBJECT_STORAGE_HOST'),
            'endpoint' => 'https://' .getenv('OBJECT_STORAGE_SERVER'),
        ],
        'cache'     => true
        'serve_js'  => true,
        'serve_css' => true
    ],
];
$settings['flysystem'] = $schemes;

// use /tmp folder
$config['system.file']['path.temporary'] = '/tmp';
```

#### Flysystem

To use the Object Storage, you need file abstraction. Install and enable the [Flysystem modules](https://www.drupal.org/project/flysystem) by executing the following in your local terminal:

```bash
# Install Flysystem
$ composer require drupal/flysystem dev-8.x-1.x
$ composer require drupal/flysystem_s3 dev-8.x-1.x

# Enable (drupal console must be called from the `web/` sub folder)
$ php vendor/bin/drupal module:install flysystem --root=web
$ php vendor/bin/drupal module:install flysystem_s3 --root=web
```


### MySQL import

Now is a good time to export your local database and import it into your App. First you'll need to open up a tunnel to your App's MySQL database — [read here](/mysql#toc-mysql-via-terminal). Then you can execute the following in your local terminal:

```bash
# 1. Fetch your fortrabbit database credentials
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets MYSQL

# 2. Export local database
$ mysqldump -u{{your-local-user}} -p {{your-local-db}} > drupal.sql

# 3. Import into fortrabbit database
$ mysql -u{{app-name}} -h127.0.0.1 -P13306 -p {{app-name}} < drupal.sql
# You will be prompted for your fortrabbit database password
```

Read the [MySQL article](mysql#toc-access-mysql-from-local) for other ways to access the database remotely from your computer.


### Configure Git

You probably don't need to touch the `.gitignore` file on top level. This is how it should look like:

```nohighlight
# Ignore directories generated by Composer
drush/contrib
vendor
web/core
web/modules/contrib
web/themes/contrib
web/profiles/contrib

# Ignore Drupal's file directory
web/sites/*/files

# Ignore files generated by PhpStorm
.idea
```

#### Initialize Git and deploy

Back in your local terminal:

```bash
# 1. Initialize Git (if you haven't already)
$ git init .

# 2. Add fortrabbit as a Git remote repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 3. Add all files
$ git add -A

# 4. Commit files
$ git commit -m 'Initial'

# 5. Push to deploy
$ git push -u fortrabbit master
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool! Once the deployment has finished (takes much longer on the first time as all Composer packages need to be installed) you can visit the App URL in your browser:

* [https://{{app-name}}.frb.io](https://{{app-name}}.frb.io)


### Modify upload destinations

Now after everything is pushed and online: login to the Drupal admin [{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) and modify the "Field Settings" of all structure elements which require Upload. For example::

1. Go to "Structure"
2. Click on "Content Types"
3. In the "Article" row click on "Manage fields"
4. In the "Image" row click on "Edit"
5. Open the "Field settings" tab
6. Set "Upload destination" to "Flysystem: object-storage"

Rinse and repeat for all structure elements, for which you want to upload files.

This is it, you are now running Drupal 8 on fortrabbit.

---

template:    article
reviewed:    2023-11-15 15:55:29
title:       Using secure App secrets
naviTitle:   App secrets
lead:        App secrets provide a secure storage and access method for all the credentials your App needs to run.
group:       development
stack:       all

keywords:
    - Secrets
    - Credentials
    - ENV vars
    - Environment variables

---

## Problem

Your App needs confidential access details to connect to other services (user-names, passwords, API keys or alike). Those can be on fortrabbit like the MySQL database access, or externally like an API key for a queue or something. You are as paranoid [as we are](//blog.fortrabbit.com/how-to-keep-a-secret), regarding storing those details in [ENV vars](/env-vars).

## Solution

Use fortrabbit App secrets to store your credentials safely. App secrets are stored in a JSON file called `secrets.json` which is only accessible by you and your App. The location of this JSON file is stored in a predefined environment variable called `APP_SECRETS`.

## App secrets vs ENV vars

App secrets are closely related to ENV vars insofar that they are both available to your App at runtime. The big difference between them is that App secrets are stored highly secured and they are not automatically dumped out by debug tools - such as `phpinfo()` or your favorite debug toolbar.

Since App secrets are a rather unique concept, they are completely optional to use. Per default, new Apps make all App secrets available as [dynamic ENV vars](env-vars#toc-dynamic-env-vars). If you prefer to use only App secrets, then you can disable this behavior.

## App secrets in your App

Access App secrets from inside your App via PHP like so:

```php
// read all App secrets from the JSON file, get the location via ENV var
$secrets = json_decode(file_get_contents($_SERVER["APP_SECRETS"]), true);

// use a specific secret
$meaning_of_life = $secrets['CUSTOM']['MEANING_OF_LIFE'];
```

```php
// App secrets are ordered in a tree structure:
$secrets == [
    'MYSQL' => [
        'PASSWORD' => "{{mysql-password}}",
        'USER'     => "{{mysql-user}}",
        'DATABASE' => "{{mysql-database}}",
        'HOST'     => "{{mysql-host}}",
        'PORT'     => "{{mysql-port}}",
    ],
    'CUSTOM' => [
        'YOUR_CUSTOM_SECRET'    => "{{YOUR_CUSTOM_SECRET}}",
        'yourOtherCustomSecret' => "{{yourOtherCustomSecret}}"
    ]
];
```

See an example to use the App secrets to connect to MySQL for [Laravel](install-laravel-pro#toc-mysql).


## App secrets from local

Read App secrets from your local machine by using an [SSH remote exec](/remote-ssh-execution-pro) command in your terminal:

```bash
# show all App secrets
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets
# {
#     "MYSQL": {
#         "PASSWORD": "{{mysql-password}}",
#         "USER": "{{app-name}}",
#         "DATABASE": "{{app-name}}",
#         "HOST": "{{app-name}}.mysql.{{region}}.frbit.com",
#         "PORT": "3306",
#     },
#     "CUSTOM": {
#         "YOUR_CUSTOM_SECRET": "The custom content",
#         "yourOtherCustomSecret": "The custom content"
#     }
# }

# show only MySQL secrets
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets MYSQL
# {
#     "PASSWORD": "{{mysql-password}}",
#     "USER": "{{app-name}}",
#     "DATABASE": "{{app-name}}",
#     "HOST": "{{app-name}}.mysql.{{region}}.frbit.com",
#     "PORT": "3306",
# }

# show only MySQL password
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets MYSQL.PASSWORD
# {{mysql-password}}
```


## Adding custom App secrets

You can add or remove custom App secrets in the [Dashboard](dashboard). You'll do so in the settings of your [App](app). The contents of the App secrets cannot be viewed in the Dashboard due to the underlying encryption, which we consider a feature, not a bug.

You can overwrite old values by adding new values to the same keys or by first deleting the App Secrets with the "delete" button.

<div markdown="1" data-user="known">

[See App secrets of your App **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/secrets)
[Add new App secrets for your App **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/secrets/new)

</div>


## App secrets vs local environment

Since access to your App secrets should be done using `$_SERVER['APP_SECRETS']`, you can easily set this environment variable locally with a path to a local JSON file containing your local (dummy) secrets.

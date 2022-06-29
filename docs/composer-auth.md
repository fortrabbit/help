---

template:    article
reviewed:    2022-06-29
title:       How to generate auth.json for Composer private repos 
naviTitle:   Private Composer repos
lead:        Generate an auth.json file during deployment to access private Composer repos.
group:       development
stack:       all

keywords:
    - Composer
    - Git
    - ssh
    - GitHub
    - Bitbucket
    - private
---

Modern PHP App development utilizes [Composer](composer) as a dependency manager. There are many great open source packages [out there](http://packagist.org). But your company code is probably not intended to be released to public. That's when you use private Composer repositories.

In the script below we generate global auth.json file that contains a credentials to access Github repo using oAuth and another private repo which is protected Basic HTTP auth, in our example Laravel Nova.
This is just for the sake of demonstration, you probably need or another - adjust it to your needs.

Since you don't want keep secrets in your git history, you store them in [Secrets](/secrets) or [Env vars](/env-vars), in the script we show both ways.

```php
<?php # add-auth.php

$secrets = json_decode(file_get_contents($_SERVER["APP_SECRETS"]), true);
$nova_username = $secrets['CUSTOM']['NOVA_USERNAME'];
$nova_password = $secrets['CUSTOM']['NOVA_PASSWORD'];

$github_oauth =  getenv('GITHUB_OAUTH_TOKEN');

$data = [
    'github-oauth' => [
        ["github.com" =>  "8f4f2e8c4454651dae52fab1b43eefdda06f9cd5xxx"]
    ],
    'http-basic' => [
        'nova.laravel.com' => [
            'username' => $username,
            'password' => $password,
        ]
    ]
];
$json = json_encode($data);

$path = '~/.composer/';
$authFile = $path . 'auth.json';
mkdir($path, 0777, true);
file_put_contents($path, $json);
```

The script you created needs to be executed before composer tries to install packages. Create a fortrabbit.yaml file with the following structure:

```yaml
version: 2
pre: add-auth.php
```

After deploying the two files you are set to access your private repos.

### Link your private repo

Now you can add your private repositories into your `composer.json` file like usual:

```json
    "repositories": [
        {
            "type": "vcs",
            "url":  "git@github.com:my-company/my-package.git"
        }
    ],
    "require": {
        "my-company/my-package": "^1.2.3"
    }
}
```

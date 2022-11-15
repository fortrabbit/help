---

template:    article
reviewed:    2022-11-15
title:       How to use private Composer repos 
naviTitle:   Private Composer repos 
lead:        Learn two different ways how to access private composer repositories during git deployment.
group:       development
stack:       all
order:       11

keywords:
    - Composer
    - Git
    - ssh
    - GitHub
    - Bitbucket
    - auth.json
    - oAuth


---

Modern PHP app development utilizes [Composer](composer) as a dependency manager. There are many great open source packages [out there](http://packagist.org). But your company code is probably not intended to be released to the public or you rely on a third party package which is not open source. That's when you use private Composer repositories.

## Using oAuth or HTTP Basic Auth 

In the script below we generate a global auth.json file that contains credentials to access a Github repo using oAuth, and another private repo which is protected with Basic HTTP auth, in our example Laravel Nova.

This is just for the sake of demonstration, you will probably need to adjust it to your needs.

Since you don't want to keep secrets in your git history, you can store them in [Secrets](/secrets) or [Env vars](/env-vars). In the script we show both ways.

```php
<?php

# add-auth.php

$secrets = json_decode(file_get_contents($_SERVER["APP_SECRETS"]), true);
$nova_username = $secrets['CUSTOM']['NOVA_USERNAME'];
$nova_password = $secrets['CUSTOM']['NOVA_PASSWORD'];

$github_oauth =  getenv('GITHUB_OAUTH_TOKEN');

$data = [
    'github-oauth' => [
        ["github.com" =>  $github_oauth]
    ],
    'http-basic' => [
        'nova.laravel.com' => [
            'username' => $nova_username,
            'password' => $nova_password,
        ]
    ]
];
$json = json_encode($data);

$path = '~/.composer/';
$authFile = $path . 'auth.json';
mkdir($path, 0777, true);
file_put_contents($path, $json);
```

The script you created needs to be executed before Composer tries to install packages. Create a `fortrabbit.yml` file with the following structure:

```yaml
version: 2
pre: add-auth.php
```

After deploying the two files you are set to access your private repos.

### Link your private repo

Now you can add your private repositories to your `composer.json` file as usual:

```json
{
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


## Using SSH Keys

Alternatively you can limit access to a specific SSH keys.

To use your private Composer repo in [Git deployment](git-deployment) you need to set up authentication so your fortrabbit App can access your external repo (probably hosted on Bitbucket, GitHub etc). For this you need a public and private SSH key-pair. Here is how you generate it for your App:

```bash
ssh {{ssh-user}}@deploy.{{region}}.frbit.com keygen
# Generating new SSH key pair
#   Done 321ms
#
# Your SSH public key:
# ssh-rsa AAAAB3NzaC1yc2EAA...ixx47pDIa1xtMV4odTimp
```

The private key will be stored in the deployment environment of your App that Composer can use it but nobody else. The command shows the public key, in the example is starting with `ssh-rsa AAA...` and ending with `..odTimp`.

You can now install the key in your private git repository - something like BitBucket, GitHub or the one from your company. You can re-run this command at any time to view or change the current key of your App.

---

template:      article
reviewed:      2021-03-22
title:         SSH access
naviTitle:     SSH access
lead:          Learn what you can do on the command line with fortrabbit Apps.

stack:         uni
proLink:       remote-ssh-execution-pro
group:         ssh
order:         3
rank:          1

keywords:
    - beginner
    - deployment

---

Using our [Git deployment](git-deployment) is great, but sometimes need a little more control. That's where the Secure SHell comes in. SSH is the big brother of [SFTP](sftp-uni) and allows you to directly interact with your App's code. You may want to set up a [SSH Key](ssh-keys) if you don't have one already.


## Accessing SSH

Execute the following in your terminal to login to your App.

```bash
# Connect to your App via SSH
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
```

If you are asked for a password, it is NOT your Account password. It's likely then that the key has not been imported correctly or she client is sending the wrong key from the default location, and not the one you imported.

If the login went well, you will see small a welcome message:

```
–––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––
```

You are now logged in to your App by SSH.

## Things to do with SSH

Following a few simple examples, just to give you some ideas:

```bash
# Download & unpack wordpress
$ curl https://wordpress.org/latest.tar.gz | tar zx

# Stream your Apache access logs
$ tail -f ../logs/apache_access.log

# Search for 50x responses in apache access
$ grep -E ' 50[0-9] ' ../logs/apache_access.log
```

Also checkout: [WordPress install from SSH](install-wordpress-4-uni#toc-installing-wordpress-with-ssh), [Execute Laravel's artisan](install-laravel-5-uni#toc-migrate-amp-other-artisan-commands).


### Executing PHP scripts

If you want to execute PHP scripts, including `artisan` and it's like, make sure to specify the PHP interpreter explicitly:

```bash
# will work
$ php artisan some:command
$ php some-script.php

# will _not_ work:
$ ./artisan some:command
$ ./some-script.php
```

### Syncing code with rsync

rsync is a super cool deployment tool that adds on top of SSH. Please read the dedicated [rsync article](/rsync).


### Using Composer

Using [Git deployment](git-deployment) will trigger [Composer](composer) automatically. So usually, you don't need to run Composer on the App itself. Really. When you find yourself running Composer on the App itself, chances are that you are doing something wrong there. Think twice, ask us in doubt. If you still need to run Composer manually on the App, you can, please don't use `composer update`, that will cause memory problems in most cases. Use `composer install` instead. Again, running Composer directly on the App is for edge cases.


## Limitations

* This is not a root shell, so you can't install or remove software packages
* Mind that you are using the same runtime as your web application: resource intensive operations will drain memory and CPU from the web execution
* [Crons](/cron-job-uni) are managed via the Dashboard, not the shell
* See the [specs page](https://fortrabbit.com/specs) for more limits and numbers


## Troubleshooting SSH access

Got an error when trying to login by SSH? Please [proceed here](/ssh-troubleshooting).

## Creating an SSH Key

Please follow the instructions on the [SSH Keys](ssh-keys) page.



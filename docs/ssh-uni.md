---

template:      article
reviewed:      2019-09-30
title:         SSH
naviTitle:     SSH
lead:          Learn what you can do on the command line with fortrabbit Apps.

stack:         uni
proLink:       remote-ssh-execution-pro
group:         deployment

keywords:
    - beginner
    - deployment

---

Using our [Git deployment](git-deployment) is great, but sometimes need a little more control. That's where the Secure SHell comes in. SSH is the big brother of [SFTP](sftp-uni) and allows you to directly interact with your App's code.


## Accessing SSH

Execute the following in your terminal:

```bash
# Login to your App by SSH like so:
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
# Unless you are using public key: You will be asked for your Account password
```

When it worked, you will see small a welcome screen:

```
–––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––
```

You are now logged in to your App by SSH. See [below](#toc-troubleshooting-authentication) if you got an error.

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

If you want to execute PHP scripts, including `artisan` and it's like, make sure to specify the PHP interpreter explicity:

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


## Troubleshooting authentication

Got an error when trying to login? fortrabbit supports username + password and public key authentication. The latter is recommend, but, especially when using Windows, sometimes password is the only feasible option. Please go on here:

* [See the access methods article](access-methods)


### Blacklisting

When nothing works any more, see if you are blacklisted, more [here](/troubleshooting#toc-blacklisting).

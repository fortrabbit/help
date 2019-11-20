---

template:      article
reviewed:      2019-09-30
naviTitle:     Remote SSH execution
title:         Using remote SSH commands
excerpt:       "How to run Artisan migrate and other useful commands."
lead:          "There is no full SSH access with Professional Apps, but you can still run SSH commands, here is how."
stack:         pro
group:         deployment

---


## Problem

With fortrabbit [Pro Stack Apps](app-pro) you [deploy with Git](/git-deployment). But sometimes SSH access to your App can be helpful as well. fortrabbit Apps have [ephemeral storage](app-pro#toc-ephemeral-storage) and a horizontal scalable infrastructure. This allows great performance and grants high availability. One trade off is that full SSH access is not feasible.


## Solution

Remote SSH execution: run single commands remotely using the SSH remote exec protocol.


### Application scenarios

* Use CLI tools for tasks like database migration (see below)
* Use remote task runners like [Envoy](https://laravel.com/docs/master/envoy) (or [Gulp/SSH](https://www.npmjs.com/package/gulp-ssh))
* List files on remote: see what actually has been deployed
* Debug while developing your [Worker](/worker-pro) and cron jobs
* Debug while developing your [pre/post deploy scripts](/deployment-file-v2#toc-full-schema)
* Execute arbitrary PHP scripts within your Apps
* …


## Usage

The main difference of the SSH remote exec to a "full SSH environment" is that you can only execute one command at once and that you specify the command to be executed with the SSH login command line. All you need to do is: prefix the command you want to execute remotely with the SSH login command.

### Example

```
# Get a directory listing of your Apps htdocs folder:
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com ls -lha
# Try it out! This works with your (selected) App.
```

### Schema

```
$ ssh [[your-app]]@deploy.[[region]].frbit.com ls -lha
# └─────────────────────┬────────────────────┘ └──┬───┘
#                       ▼                         ▼
#                ssh login command          remote command
```

**Note I**: Unless otherwise specified by you all commands are executed from within `/srv/app/{{app-name}}/htdocs`, which is also the location to which your files are deployed. So if the script, you want to execute, is locally under `{{vendor/bin/foo}}` then you'd need to execute `ssh …frbit.com php {{vendor/bin/foo}}`. Check out the [directory structure](/directories) for more information.

**Note II**: You must write the interpreter `php` before all PHP scripts, including CLIs like `artisan` or `app/console`, you want to execute. So `ssh …frbit.com php {{script.php}}` works and `ssh …frbit.com {{script.php}}` does not.

### Authentication

For remote SSH execution you can identify yourself using your public SSH keys or with your Account password. The examples above show you correct values, when you are logged in to the Dashboard. Please check the [access methods article](/access-methods) for more information.


### Using CLI tools

Many modern web development frameworks and CMS come with a programmable command line interfaces (CLI), Drupal Console and Artisan for example. These CLIs allow you to automate often occurring tasks, help you to develop your App via code generators and provide useful helpers to set up new installations. Executing commands, like database migration, can hugely simplify development.


### Limitations

**No persistent code manipulation**: Code changes made via remote SSH execution don't have any effect in your running App! This means: You can run a database migrate command, which changes the contents of your database and thereby has effect on your App. But: If your remote command actually modifies, creates or removes files, it will only be modified, created or removed from the "deployment Container" in which the SSH command was executed, not in the "Web Container", which is used to serve your App to the web. Also those changes will be undone with your next deploy. Hence: All code changes need to be made locally and then [committed and pushed via Git](/git-deployment).

**No uploads**: SFTP is not available. All code changes need to be made via [Git deployment](/git-deployment) due to [ephemeral storage](app-pro#toc-ephemeral-storage).

**No concurrent usage**: To guarantee code consistency no deployments can be made while a remote SSH command is executing. For the same reasons parallel deployments and parallel SSH executions are not allowed.

**Limited execution time**: The maximal execution time for remote SSH commands is [limited](https://www.fortrabbit.com/specs#limits).

**No background execution**: Commands cannot be detached to the background. This means: When the command execution is finished and you are back on your local shell, then the remote command execution has terminated as well - whether there are still running detached processes or not.


### Examples

```bash
# Arbitrary PHP script
# Call a PHP script, passing some arguments
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php {{my-script.php}} {{arg1}} {{arg2}}

# Laravel: artisan
# Execute `some:command` using Laravel `artisan` CLI:
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan {{some:command}}

# Symfony: console
# Execute `some:command` using Symfony `app/console` CLI:
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php app/console {{some:command}}
```

Check out the [Laravel article](/install-laravel-5#toc-migrate-amp-other-database-commands) for more examples.


#### Gulp/SSH

A simple example which executes `my-script.php` remotely and write it's output into local `logs/output.log` file. This is the `gulpfile.js`:

```js
'use strict'

var fs = require('fs'),
  gulp = require('gulp'),
  SSH  = require('gulp-ssh');

var config = {
  host:       'deploy.{{region}}.frbit.com',
  port:       22,
  username:   '{{ssh-user}}',
  privateKey: fs.readFileSync(process.env.HOME + '/.ssh/id_rsa')
};
var ssh = new SSH({ignoreErrors: false, sshConfig: config});

gulp.task('run-script', function() {
  return ssh
    .exec('php my-script.php some-arg', {filePath: 'output.log'})
    .pipe(gulp.dest('logs'));
});

```

Then run in your local terminal:

```bash
$ gulp run-script
```


## Private repositories

With the Professional Apps you can create a new SSH keypair for your App, using the `keygen` command:

```bash
ssh {{ssh-user}}@deploy.{{region}}.frbit.com keygen
```

This will print out a public key, which you can then install with your private repository (on Github, Bitbucket or wherever).


## Advanced usage

Still reading, go ahead, dive deeper!

### Chaining

You can execute multiple commands at once by quoting everything and separating the commands with a semicolon:

```
# execute three scripts
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com "php {{foo.php}} ; php {{bar.php}} ; php {{baz.php}}"
```

### Aliases

On Mac and Linux: You can define local "aliases", which reduce the amount typing when executing a remote SSH command by a lot. For example:

```
# create the alias
$ alias artisan-{{app-name}}="ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan"

# use the alias
$ artisan-{{app-name}} migrate
# this actually calls "ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan migrate"
```

To persist those aliases between shell sessions you need to add them to your shell profile file. Usually that is either `~/.profile`, `~/.zshrc` or `~/.bashrc`.

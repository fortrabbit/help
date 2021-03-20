---

template:      article
reviewed:      2021-03-19
naviTitle:     Access methods
title:         How to access fortrabbit services
lead:          Learn about the two authentication methods available for fortrabbit services.
group:         deployment
stack:         all

keywords:
     - ssh key
     - git
     - git-deployment
     - ssh
     - username
     - password
     - access
     - authentication

---

After creating an Account on fortrabbit (e-mail + password) and having checked out the [Dashboard](/dashboard), there is more to explore. Read on to learn how to interact with services provided on fortrabbit.

Services or actions which require authentication include:

* Deploying code with [Git](/git-deployment#toc-usage)
* Using [SSH](/ssh-uni)
* Using [SFTP](/sftp-uni)
* Accessing the [MySQL database](/mysql#toc-remote-mysql-access)
* Viewing [log files](/logging-uni)
* Execution [SSH Remote commands](/remote-ssh-execution-pro#toc-usage)
* Accessing the [Object Storage](/object-storage#toc-accessing-the-object-storage)

## Available authentication methods

* **Password** - username + password mode - the classical and default alternative - [see below](#toc-password-authentication)
* **Public key** - ssh-key mode - the recommended approach (slightly more advanced) - [see below](#toc-ssh-key-authentication)

## Identify your current access method

To figure out what is currently active, go to the [fortrabbit Dashboard](/dashboard), then [Your Account (upper right)](https://dashboard.fortrabbit.com/account). On the right side of the page, below the section Security, you will see one of two things:

* **Password**: a heading called Code access with the subtext: You can access code with your Account password
* **Public key** a heading called SSH keys above a list of imported public keys


## Select one of the methods

You must opt in to use ssh-keys by importing a public key. Only one of these methods can be active at any given time.

### Changing from password to SSH key authentication

First, you must create a key-pair and import the public part into the fortrabbit Account.
After a public key is imported, the username+password access mode is automatically disabled.
If you remove all ssh keys from your fortrabbit Account, the username + password access mode is automatically re-enabled.

Please note that the code access part (aka username) is different for these two access modes. The one which is currently correct will be shown in the Dashboard.

To import a public ssh key, go to: Dashboard > Your Account > Code access


<div markdown="1" data-user="known">
[Add a new SSH key for your Account](https://dashboard.fortrabbit.com/account/keys/new)
</div>


### Changing from SSH key to password authentication

If ssh keys do not work for you, you can disable the ssh-key access mode by removing all public keys from your account. Go to: Dashboard > Your Account > SSH keys

From this view, you will be able to select individual keys. Selecting a specific key brings up another view where the key can be deleted. After the last public key is removed, the username + password access mode will be automatically re-enabled.


## SSH key authentication

See our [SSH key guide](ssh-keys) on how to setup keys and how to add them to your fortrabbit Account.


## Password authentication

This is the default method before any public SSH keys are imported. Use this, if you just want to check out fortrabbit or when you have trouble setting SSH keys.

### When you change your Account password

When you change your Dashboard password, for instance when you [regain access to the Dashboard](/dashboard#toc-regaining-access) in case of a forgotten password, all access to the services will change as well - the new password will be used.


## Authentication in teams

You manage your code access with your user Account on fortrabbit. This way you always have up-to-date code access on each App you own and collaborate on. It also makes managing the team easy — add/remove collaborators and code access is handled "automagically". Please mind that your team members might have a different access method and that your settings might not work for them. Also see our [team work article](/collaboration).


## Practical examples

URLs and terminal commands depend on the currently active access method.

```shell
# Git clone with SSH keys
$ git clone {{app-name}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# Git clone with username + password
$ git clone {{app-name}}.{{randomletters}}@deploy.{{region}}.frbit.com:{{app-name}}.git


# Example Git repository URL with SSH keys
tripple-w-app@deploy.eu2.frbit.com:tripple-w-app.git

# Example Git repository URL with username + password
tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com:tripple-w-app.git
```


### The code example helper

When you are currently logged in to the Dashboard (a cookie is stored), you will see a yellow box on the right side here - with a select to choose one of your Apps. This helper knows which authentication method your Account uses. It also changes all code examples on the current page according to the selected App. So you can literary copy/paste all code examples here.


#### Try it out yourself

```shell
# change the values on the right and see the below change
SSH user: {{ssh-user}}
Region:   {{region}}
Your app: {{app-name}}
```

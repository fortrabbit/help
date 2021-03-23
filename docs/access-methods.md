---

template:      article
reviewed:      2021-03-21
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

## Services which require authentication

* Deploying code with [Git](/git-deployment#toc-usage)
* Using [SSH](/ssh-uni)
* Using [SFTP](/sftp-uni)
* Accessing the [MySQL database](/mysql#toc-remote-mysql-access)
* Viewing [log files](/logging-uni)
* Execution [SSH Remote commands](/remote-ssh-execution-pro#toc-usage)
* Accessing the [Object Storage](/object-storage#toc-accessing-the-object-storage)

## Available authentication methods

* **Password**  - [see below](#toc-password-authentication)  
  Classical username + password authentication mode
* **SSH public key authentication** - [see below](#toc-ssh-key-authentication)  
  Slightly more advanced and more convenient mode using private and public SSH keys

## Identify your current access method

To figure out what is currently active, go to: 

+ [fortrabbit Dashboard](/dashboard) > [Your Account (upper right)](https://dashboard.fortrabbit.com/account)

On the right side of the page, below the section Security, you will see one of two things:

* A heading called Code access with a subtext shows that you are using username + password authentication
* A heading called SSH keys above a list of imported public keys shows that you are using SSH public key authentication


## Selecting one of the methods

Only one of these methods can be active at any given time. Username + password is the default. You must opt in to use SSH public key authentication by importing a public key.


### Changing from password to SSH key authentication

<div markdown="1" data-user="known">
[Add a new SSH key for your Account](https://dashboard.fortrabbit.com/account/keys/new)
</div>

Add your public SSH keys to your fortrabbit Account to change the access method from from username + password authentication to SSH public key authentication. Please see the [SSH key help page](/ssh-keys) for all details.

After a public SSH key is imported, the username + password access mode is automatically disabled (takes a couple of minutes).


### Changing from SSH key to password authentication

You can disable SSH public key authentication by removing all public SSH keys from your fortrabbit Account. Go to: 

* Dashboard > Your Account > SSH keys

From this view, you will be able to select individual keys. Selecting a specific key brings up another view where the key can be deleted. After the last public key is removed, the username + password access mode will be automatically re-enabled.


### After changing the access method

Please note that the code access part (aka username) is different for these two access modes. The one which is currently correct will be shown in the Dashboard. Pick up the new access credentials from Access tab with your Apps.


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

```bash
# Git clone with SSH keys
$ git clone {{app-name}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# Git clone with username + password
$ git clone {{app-name}}.{{randomletters}}@deploy.{{region}}.frbit.com:{{app-name}}.git


# Example Git repository URL with SSH keys
appname@deploy.eu2.frbit.com:appname.git

# Example Git repository URL with username + password
appname.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com:appname.git
```

If you need more specific examples, then go to [Verify access methods](/verify-access-methods)

## The code example helper

When you are currently logged in to the Dashboard (a cookie is stored), you will see a yellow box on the right side here - with a select to choose one of your Apps. This helper knows which authentication method your Account uses. It also changes all code examples on the current page according to the selected App. So you can literary copy/paste all code examples here.


#### Try it out yourself

```bash
# change the values on the right and see the below change
SSH user: {{ssh-user}}
Region:   {{region}}
Your app: {{app-name}}
```

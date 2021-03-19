---

template:      article
reviewed:      2021-02-09
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

* **Password** - username+password mode - the classical and default alternative - [see below](#toc-password-authentication)
* **Public-key** - ssh-key mode - the recommended approach (slightly more advanced) - [see below](#toc-ssh-key-authentication)

To figure out what is currently active, go to the [Dashboard](/dashboard), and then [Your Account (upper right)](https://dashboard.fortrabbit.com/account).  
On the right side of the page, below the section Security, you will see one of two things.

- a heading called Code access with the subtext: You can access code with your Account password
- a heading called SSH keys above a list of imported public keys


## Select one of the methods

You must opt in to use ssh-keys by importing a public key. Only one of these methods can be active at any given time.

### How to change from password to SSH key authentication

First, you must create a key-pair and import the public part into the fortrabbit Acccount.
After a public key is imported, the username+password access mode is automatically disabled.
If you remove all ssh keys from your fortrabbit Account, the username+password access mode is automatically re-enabled.

**Note**: the login part (aka username) is different for these two access modes. The one which is currently correct will be shown in the Dashboard.

To import a public ssh key, go to:

* dashboard.fortrabbit.com > Your Account > Code access


<div markdown="1" data-user="known">
[Add a new SSH key for your Account](https://dashboard.fortrabbit.com/account/keys/new)
</div>


### How to change from SSH key to password authentication

If ssh keys do not work for you, you can disable the ssh-key access mode by removing all public keys from your account.

Go to:

- dashboard.fortrabbit.com > Your Account > SSH keys

From this view, you will be able to select individual keys. Selecting a specific key brings up another view where the key can be deleted. After the last public key is removed, the useraname+password access mode will be automatically re-enabled.



## SSH key authentication

We recommend to use public SSH key authentication to identify yourself with fortrabbit services. It's more secure than password authentication and also more convenient, once set it up. SSH key authentication is very common and is used by service providers including BitBucket, GitHub, AWS, Google, Microsoft and virtually everyone else.



### How to add public SSH keys

The public-halves of your SSH key-pair(s) are stored in your fortrabbit Account.
To add and remove keys, go to:

- dashboard.fortrabbit.com > Your Account > Code access / SSH keys

<div markdown="1" data-user="known">

[Add a new SSH key for your Account](https://dashboard.fortrabbit.com/account/keys/new)

</div>



### GitHub SSH key import

**Automatic import**: When signing up to fortrabbit, we'll check at GitHub if there are any public SSH keys associated with your e-mail. If we find any, we'll import them and install them with your account. This is a one time setup, your SSH keys will not be synced. You can manage the SSH keys like any other keys then.

**Manual import**: You can also tell the import helper your GitHub account, when using different e-mail addresses here and on GitHub. There is a small link which will take you

The [direct Dashboard link](https://dashboard.fortrabbit.com/boarding/keys/github) (login and re-authentication maybe required) will take you there directly.


### App-only SSH keys

In certain cases you might want to add code access to an App without the need to register a new Account with fortrabbit. One case is a hectic ad-hoc hotfix scenario (good luck!), another case is that you have some advanced deployment with a third party continuous integration service bot going on. So you can install additional App-only custom public SSH keys with each App. You manage those App-only SSH keys in the Dashboard with your App.


### How to create a SSH key-pair locally

We have a [dedicated page](ssh-keys) with instruction on how to generate SSH keys.

### Troubleshooting SSH key access

Please see the dedicated [SSH troubleshooting page](/ssh-key-troubleshooting).

## Password authentication

This is the default method before any public SSH keys are imported. Use this, if you just want to check out fortrabbit or when you have trouble setting SSH keyss.

### When you change your Account password

When you change your Dashboard password, for instance when you [regain access to the Dashboard](/dashboard#toc-regaining-access) in case of a forgotten password, all access to the services will change as well – the new password will be used.


## Authentication in teams

You manage your code access with your user Account on fortrabbit. This way you always have up-to-date code access on each App you own and collaborate on. It also makes managing the team easy — add/remove collaborators and code access is handled "automagically". Please mind that your team members might have a different access method and that your settings might not work for them. Also see our [team work article](/collaboration).

## Practical examples

URLs and terminal commands depend on the currently active access method (ssh-key or username+password).

To clone the git repository you will use a URL based on these patterns:

    $ git clone                 «your-app»@deploy.«region».frbit.com:«your-app».git
    $ git clone «your-app».«randomletters»@deploy.«region».frbit.com:«your-app».git

Here **&#x7B;{your-app}}** or («your-app») represnts a variable value, which is the name of an App. This is available in the Dashboard if you don't remember.
Equally, **&#x7B;{region}}** is a variable, and will contains the name of the region where the app was launched such as eu2 or us1.

An example Git repository URL can look like the following.

With the ssh-key access mode enabled.

    tripple-w-app@deploy.eu2.frbit.com:tripple-w-app.git

With the useraname+password access mode.

    tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com:tripple-w-app.git


### The code example helper

When you are currently logged in to the Dashboard (a cookie is stored), you will see a yellow box on the right side here - with a select to choose one of your Apps. This helper knows which authentication method your Account uses. It also changes all code examples on the current page according to the selected App. So you can literary copy/paste all code examples here.


#### Try it out yourself

```
# change the values on the right and see the below change
SSH user: {{ssh-user}}
Region:   {{region}}
Your app: {{app-name}}
```


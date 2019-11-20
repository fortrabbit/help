---

template:      article
reviewed:      2019-03-20
naviTitle:     Access methods
title:         How to access fortrabbit services
lead:          Learn about the different authentication methods with fortrabbit.
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

So far so good: You are using your Account e-mail address and your Account password to login to the fortrabbit [Dashboard](/dashboard). Beside that, there are these interaction channels with fortrabbit services:

## Interactions requiring authentication

* Deploying code with [Git](/git-deployment#toc-usage)
* Using [SSH](/ssh-uni)
* Using [SFTP](/sftp-uni)
* Accessing the [MySQL database](/mysql#toc-remote-mysql-access)
* Viewing [log files](/logging-uni)
* Execution [SSH Remote commands](/remote-ssh-execution-pro#toc-usage)
* Accessing the [Object Storage](/object-storage#toc-accessing-the-object-storage)

Of course these operations need to be protected. We need to make sure that only you and your team have access. You can choose between two authentication methods: 

## Available authentication methods

* **Password** - the easy default - [see below](#toc-password-authentication)
* **Public SSH key** - for advanced users - [see below](#toc-ssh-key-authentication)


## How to edit your access method

In the "[Dashboard](/dashboard)", go to "Your Account" (upper right). Under "Code access" you can find your current settings. If you have already added an SSH key this section will be called "SSH keys".


## SSH key authentication

We recommend to use public SSH key authentication to identify yourself with fortrabbit services. It's more secure than password authentication and also more convenient, once you have set it up. SSH key authentication is commonly used with services like BitBucket and GitHub.



### How to add public SSH keys

SSH keys are stored with your Account. In the "Dashboard" > "Your Account" > "Code access"/"SSH keys" you can add and remove SSH keys.

<div markdown="1" data-user="known">

[Add a new SSH key for your Account](https://dashboard.fortrabbit.com/account/keys/new)

</div>

### How to create a public SSH key locally

We have a [dedicated article](ssh-keys) on setting up and troubleshooting your local SSH keys.



### GitHub SSH key import

**Automatic import**: When signing up to fortrabbit, we'll check at GitHub if there are any public SSH keys associated with your e-mail. If we find any, we'll import them and install them with your account. This is a one time setup, your SSH keys will not be synced. You can manage the SSH keys like any other keys then.

**Manual import**: You can also tell the import helper your GitHub account, when using different e-mail addresses here and on GitHub. There is a small link which will take you 

The [direct Dashboard link](https://dashboard.fortrabbit.com/boarding/keys/github) (login and re-authentication maybe required) will take you there directly.


### App-only SSH keys

In certain cases you might want to add code access to an App without the need to register a new Account with fortrabbit. One case is a hectic ad-hoc hotfix scenario (good luck!), another case is that you have some advanced deployment with a third party continuous integration service bot going on. So you can install additional App-only custom public SSH keys with each App. You manage those App-only SSH keys in the Dashboard with your App.


## Password authentication

This is the default method when no public SSH keys are installed. Use this, when you just want to check out fortrabbit or when you have trouble setting up your SSH keys locally. **Hobbyists are using Password authentication. Professionals are using SSH key authentication.**

### How to change from password to SSH key authentication

In the "Dashboard" > "Your Account" > "Code access" you can add an SSH key. Once you have added your first public SSH key, password authentication will be disabled and SSH key authentication will be enabled.


<div markdown="1" data-user="known">
[Add a new SSH key for your Account](https://dashboard.fortrabbit.com/account/keys/new)
</div>


### How to change from SSH key to password authentication

When for some reason SSH key authentication does not work for you, you can downgrade to password like so: In the "Dashboard" > "Your Account" > "SSH keys" you can click on your public SSH keys, this will bring up a view where you can delete the key. When deleting the last key, password authentication will be re-enabled.


### When you change your Account password

When you change your Dashboard password, for instance when you [regain access to the Dashboard](/dashboard#toc-regaining-access) in case of a forgotten password, all access to the services will change as well – the new password will be used.

- - -


## How it works

### Access schema

URLs and terminal commands depend on your chosen access method. 

```
# Git clone example
$ git clone [[ssh-user]]@deploy.[[region]].frbit.com:[[your-app]].git
```

* [[your-app]] is the name of your App (see Dashboard)
* [[region]] is `eu2` or `us1`, depending on the location of your App


#### With SSH key authentication

* No need to enter passwords, your public key will be used
* [[ssh-user]] will be: [[your-app]]

#### With password authentication

* You will need to enter your fortrabbit Account password (each time)
* [[ssh-user]] will be: [[your-app]].[[long-user-string]]


#### The code example helper

When you are currently logged in to the Dashboard (a cookie is stored), you will see a yellow box on the right side here - with a select to choose one of your Apps. This helper knows which authentication method your Account uses. It also changes all code examples on the current page according to the selected App. So you can literary copy/paste all code examples here.

##### Try it out yourself

```
# change the values on the right and see the below change
SSH user: {{ssh-user}}
Region:   {{region}}
Your app: {{app-name}}
```





### Authentication in teams

You manage your code access with your user Account on fortrabbit. This way you always have up-to-date code access on each App you own and collaborate on. It also makes managing the team easy — add/remove collaborators and code access is handled "automagically". Please mind that your team members might have a different access method and that your settings might not work for them. Also see our [team work article](/collaboration).

- - -

## Troubleshooting

### Authenticity error

The first time you are connecting to fortrabbit service via SSH, you might get an error and a prompt like this:

```
The authenticity of host '…' can't be established
RSA key fingerprint is … 
Are you sure you want to continue connecting (yes/no)?
```

Please answer with `yes`. This will add our servers to the known hosts. That should be a one time setup. If you are being asked this every time you deploy, something with your `known_hosts` file is probably wrong. See [this Stack Overflow question](http://stackoverflow.com/questions/9299651/git-says-warning-permanently-added-to-the-list-of-known-hosts).


### General connection errors

If you can't establish a connection at all, a corporate or personal firewall might be the cause. This can a protection software on your computer or something in your company. You can fix this by allowing communication on the SSH standard port: `22`.


### Authentication errors

Your local machine is maybe trying to use a different authentication method. Please check and correct your SSH configuration like so:

```
# terminal command to edit your ssh config file
nano ~/.ssh/config
```

Within this file change the preferred authentications accordingly:

```
# when using password authentication use:
PreferredAuthentications    password,publickey

# when using SSH key authentication use:
PreferredAuthentications    publickey,password
```


### Problems with SSH keys

Please see our [SSH key trouble shooting guides](/ssh-keys#troubleshooting) to resolve common problems with SSH key authentication.

### Multiple Accounts with different access methods issues

When you have multiple Accounts here at fortrabbit, or you have had an Account here before and you have used one access method to identify, your SSH config might saves our host to use that access method for our host. Now when using a different method with a different Account, your computer might still need the old saved one. <!--  TODO: How to fix? -->
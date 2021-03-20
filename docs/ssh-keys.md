---

template:      article
reviewed:      2021-03-20
naviTitle:     SSH key setup
title:         SSH keys setup
lead:          Learn how to how to create an SSH key pair on your local machine and how to add it your fortrabbit Account.
group:         deployment
stack:         all


keywords:
    - ssh key
    - public key
    - git
    - ssh
    - Putty
    - PuttyGen
    - msysGit
    - CygWin
    - Windows
    - Git Bash
    - RSA keys
    - DSA
    - RSA2
    - RSA1
    - SSH-2 RSA
    - OpenSSH
    - secure shell

---

Besides [password authentication](/access-methods#toc-password-authentication) you can use SSH keys to authenticate with fortrabbit services, including **[deploying via Git](git)**, [accessing live logs](logging-pro) and [remote MySQL access](mysql#toc-remote-mysql-access). SSH key authentication is more secure and arguably more convenient. Additionally, it allows several users-accounts to collaborate on one or more Apps without sharing a password.

The goals here are:

1. Create an SSH key pair consisting of a public and private key.
2. Store the keys in the right location, so that your Operating System can make use of them.
3. Save the public key with your fortrabbit Account

And hopefully explain the main concepts behind ssh-keys in simple terms.


## About SSH key authentication

Public-key authentication is more secure than plain old passwords. The main concept is that instead of a short password, one uses a key file which is virtually impossible to guess. You give us the public part of your key and when logging in it will be used, together with the private key and username, to verify your identity.

It's more secure than password authentication and also more convenient, once set it up. SSH key authentication is very common and is used by service providers including BitBucket, GitHub, AWS, Google, Microsoft and virtually everyone else.

The public part of the key, ending in `.pub` is safe to give out, while the private part should not be shared.

After you import a public key into your fortrabbit account, it can then be used to authenticate you. When an SSH client connects, the server will encrypt some secret using the public key. If the client is able to decrypt the secret with the private key and send it back to the server, then the server knows that the client has the private key and can be trusted.


## Do you already have a key?

To see any existing keys, open a terminal or use Git Bash and use find to list the dot-ssh folder.

```bash
$ find ~/.ssh
#/home/handle/.ssh/config
#/home/handle/.ssh/id_rsa
#/home/handle/.ssh/id_rsa.pub
#/home/handle/.ssh/known_hosts
```

If you don't have any key pairs or get an error about a missing folder, then proceed to the next section.

On the other hand, if you do see key pairs like above (public - pub extension, private - no extension), then you can opt to use the existing key pair with fortrabbit. It is not strictly required to create a new key pair, but the one you end up using must be in the RSA format.

To import the public part into your fortrabbit Account via Dashboard, read about [importing keys](ssh-keys#toc-import-a-public-keys-into-your-fortrabbit-account) further down.


## Generate a SSH key pair (aka ssh key)

Currently, we support only RSA keys. Please use 4096 bit keys, or longer.
You can skip the `-f ~/.ssh/id_rsa_fortrabbit` part to use the default location which is `~/.ssh/id_rsa`

```bash
$ ssh-keygen -t rsa -b 4096 -C user@fortrabbit -f ~/.ssh/id_rsa_fortrabbit
# Generating public/private rsa key pair.
# Enter passphrase (empty for no passphrase):
# Enter same passphrase again:
# ...

↑ Press Enter twice to skip the passphrase

# Your identification has been saved in /home/user/.ssh/id_rsa_fortrabbit
# Your public key has been saved in /home/user/.ssh/id_rsa_fortrabbit.pub
# The key fingerprint is:
# SHA256:QuLDaljPIExVTlsCZkyKiByU7p9RlKp8ABmJcyC5F54 user@fortrabbit
# The key's randomart image is:
# +---[RSA 4096]----+
# |*O+*o+..         |
# |&.O.oo+          |
# |*O o+o.          |
# |+oE+ +           |
# |+o= = . S        |
# | B B . .         |
# |. = =            |
# | . o             |
# |                 |
# +----[SHA256]-----+
```


<!-- TODO: Discuss this please! Why do we send the location? The name is already unique! -->

If you want to use the default location (because you don't have any other keys), then type this instead, followed by the Enter key three times.

```bash
$ ssh-keygen -t rsa -b 4096 -C user@fortrabbit
# Generating public/private rsa key pair.
# Enter file in which to save the key (/home/user/.ssh/id_rsa):
# Enter passphrase (empty for no passphrase):
# Enter same passphrase again:
```


### SSH keys generation on Windows

The procedure to create SSH keys is slightly different on Mac OS and Linux compared to Windows.
There are different ways to set up and use Git with Windows and also different ways set up and store the keys.
We recommend using the official installer from the Git website, together with Git Bash.

Check out this concise tutorial from GitHub (covering all three of the most popular platforms):

* [Generate a new SSH key & add it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-windows)

You can also have a look at this very detailed article which is more windows-oriented.

* [Git and SSH key setup on Windows from Beanstalk](http://guides.beanstalkapp.com/version-control/git-on-windows.html)

## Importing a public keys into your fortrabbit Account

After you have set up Git and created a key pair, the public part of the key must be imported in the fortrabbit Dashboard. The navigation to the import form is not very complex: 

* Dashboard > Your Account > Access methods > Add a new SSH key

<div markdown="1" data-user="known">
[Add a new SSH key for your Account](https://dashboard.fortrabbit.com/account/keys/new)
</div>

You might need to re-enter your fortrabbit Account password. Then you will see a form with a textarea to paste the public key in to. If you followed our guide, the file you want to import wil be located in:

```bash
~/.ssh/id_rsa_fortrabbit.pub 
# or
~/.ssh/id_rsa.pub
```

This is what a valid SSH key looks like (don't paste this):

```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbez9IDLYECMpQUQgNTWPG5aPMwJFNNP3a0
gAHVz8+N4HgiwFwBll2iUX0YPHIpbfeXN4Kab30qsevw59cjQ1XC7yjkrXy03OyOv/Z9X+KpB
vnf/cRXwz2zxfQqwvmXIQl3jlxyuA+Y4VjvELIvCrnnsfJDETmF8HZG4zA5XFfS95y5xx3TF9
S/eTlx2qWrmhsf20H+P/FK8otXKa+EW4UY6mew/lVxboEYDfCTju8cS5raJBmTehBaYyWI2dy
9oEWvD+qySvrEf1gXRRAMmt0/bOR4jw8G18i5siMtse2s/qMomG08VMeVIAEK9Tp64Mx4mmQv
IvP1bffus+WdY75 you@localhost
```

**Make sure you paste the public part of the key and not the private.**

The above key is split across multiple lines for the sake of readability. When adding a key in the Dashboard, it must be all-on-one line, without any newline characters in the middle.  The key must start with `ssh-rsa`, other key types are not supported with our services.

The value to paste into the text field can be read into the clipboard from a terminal. On Windows, just use notepad to open the `id_rsa.pub` file and select all, then copy. You can also [import your GitHub keys](/access-methods#toc-github-ssh-key-import).

```bash
$ pbcopy < ~/.ssh/id_rsa.pub    # Mac OS
$ xclip -i < ~/.ssh/id_rsa.pub  # linux
```

### GitHub SSH key import

**Automatic import**: When signing up to fortrabbit, we'll check at GitHub if there are any public SSH keys associated with your e-mail. If we find any, we'll import them and install them with your account. This is a one time setup, your SSH keys will not be synced. You can manage the SSH keys like any other keys then.

**Manual import**: You can also tell the import helper your GitHub account, when using different e-mail addresses here and on GitHub. There is a small link which will take you

The [direct Dashboard link](https://dashboard.fortrabbit.com/boarding/keys/github) (login and re-authentication maybe required) will take you there directly.



## fortrabbit specific concepts

### Account SSH keys

Remember, on fortrabbit an Account represents a person (see [Account help page](/account) ). Your Account on fortrabbit may store the public parts of several SSH keys. We recommend you use this method for authentication. Doing so allows you to push code, view logs, and connect to the MySQL database for all of your Apps. In addition, other users who have an Account on fortrabbit may collaborate on your Apps without the need to share passwords. Their SSH keys will then be allowed to deal with your App.


### App-only SSH keys

In certain scenarios, you may want to grant someone or something access to an App without creating a new Account. For these purposes, you can use App-only keys that are separate from any Account on fortrabbit. The App-only keys are managed via the Dashboard with the Apps. Adding and removing such keys follows the same concepts as described above.


## Troubleshooting SSH key access

Please see the dedicated [SSH troubleshoot guide](/ssh-key-troubleshooting).

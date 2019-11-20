---

template:      article
reviewed:      2019-09-30
naviTitle:     SSH keys setup
title:         Troubleshooting SSH keys setup
lead:          This article helps solving common issues setting up your SSH keys.
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

---

Beside [password authentication](/access-methods#toc-password-authentication) you can use your public SSH keys to authenticate yourself with a variety of fortrabbit services — such as **[deploying via Git](git)**, [accessing live logs](logging-pro) and [remote MySQL access](mysql#toc-remote-mysql-access). SSH key authentication is more secure and more convenient, once setup. This article will help setting up your SSH keys.

The goals here are:

1. Create an SSH key pair consisting of public and private key.
2. Store the keys on the right location, so that your Operating System can make use of them.
3. Save the public key with your fortrabbit Account


## Do you have any SSH keys already?

To check if you have any existing SSH keys installed: Mac Os & Linux: Open a terminal (Mac OS & Linux) or a Git Bash (Windows) and type:

```
ls -hal ~/.ssh
```

If you see an existing key pair listed (for example `id_rsa.pub` and `id_rsa`) that you would like to use to connect to fortrabbit, you can skip the generation and add the public SSH key (contents of `id_rsa.pub`) to your fortrabbit Account in the Dashboard, see below.

If you don't have any key pairs or you receive an error that ~/.ssh doesn't exist, you probably have a different/incorrect setting or haven't set up anything yet.

On Windows: If you don't know what the Git Bash is, or you have a different setup, check our recommendation on the [Git article](git) (again).


## Generate SSH keys

The procedure to create SSH keys is a bit different on each Operating System.

### SSH keys generation on Windows

There are different ways to setup and use Git with Windows, thus there are also different ways setup and store the keys. We recommend to use the official installer form the Git website, together with the Git Bash tool. This tutorial from GitHub is lazer-sharp:

* **[Generate a new SSH key & add it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-windows)**

Have a look at the very detailed [Git and SSH key setup on Windows from Beanstalk](http://guides.beanstalkapp.com/version-control/git-on-windows.html) to learn about alternative ways.


### SSH keys generation on Mac OS & Linux

This tutorial from GitHub is lazer-sharp:

* [Generate a new SSH key & add it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-mac)



## Save your public SSH keys with your fortrabbit Account

After you have set up Git and created your local SSH key and stored it on the correct place, you'll need to tell fortrabbit about that. So that we can securely identify you later on.

* "Dashboard" > "Your Account" > "Access methods" > "Add a new SSH key"

You might be asked to re-enter your fortrabbit Account password to do this. Then you will see a form with a textarea to copy/paste the actual key in. Most likely this should be the contents of the file `id_rsa.pub` which is stored in `~/.ssh`.

This is what a valid SSH key looks like (don't paste this):

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbez9IDLYECMpQUQgNTWPG5aPMwJFNNP3a0
gAHVz8+N4HgiwFwBll2iUX0YPHIpbfeXN4Kab30qsevw59cjQ1XC7yjkrXy03OyOv/Z9X+KpB
vnf/cRXwz2zxfQqwvmXIQl3jlxyuA+Y4VjvELIvCrnnsfJDETmF8HZG4zA5XFfS95y5xx3TF9
S/eTlx2qWrmhsf20H+P/FK8otXKa+EW4UY6mew/lVxboEYDfCTju8cS5raJBmTehBaYyWI2dy
9oEWvD+qySvrEf1gXRRAMmt0/bOR4jw8G18i5siMtse2s/qMomG08VMeVIAEK9Tp64Mx4mmQv
IvP1bffus+WdY75 you@localhost
```

MIND THE DIFFERENCE BETWEEN PUBLIC AND PRIVATE KEY! The above is multi-line only for readability. Please remove the line breaks from the SSH key when adding it to the Dashboard. The key should start with `ssh-rsa`, `ssh-dss` or `ecdsa-sha2-nistp...` if not, it's probably not a supported version.

You can also [import your GitHub keys](/access-methods#toc-github-ssh-key-import).

- - -


## Troubleshooting

The following section helps you solving SSH key specific connection errors. Please also see the [general connection trouble shooting](/access-methods#toc-troubleshooting) before.

### Permission denied error

When you get this after a `git push` or `git pull`:

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

and you have never deployed to fortrabbit before, chances are that the setup is not yet correct. Please see if you have [Git](git) and SSH keys (see above) correctly installed — on your local machine and on fortrabbit. 


### SSH keys under Windows with PuTTY

We advice not to use PuTTY for this. If you installed your SSH keys on Windows with PuTTY, you either know exactly what you are doing, or you might have followed a Google result and have it not yet correctly. You may also have a look these questions:

* [Stack Overflow: Where to find my private RSA key?](http://serverfault.com/questions/194567/how-do-i-tell-git-for-windows-where-to-find-my-private-rsa-key)
* [superuser: Where does Putty store known_hosts information on Windows?](http://superuser.com/questions/197489/where-does-putty-store-known-hosts-information-on-windows)

Putty uses .ppk key files - Putty Private Key. Those don't work here. You can convert those files to standard openSSH keys within PuttyGen or an online service.

### When nothing works for you

If you can't get it to work at all: [remove your SSH keys from your fortrabbit Account](access-methods#toc-how-to-change-from-ssh-key-to-password-authentication) to switch back to password authentication.


### When it worked before

If you have deployed using SSH keys before and now it doesn't work any more: please check if you have changed something, compare your local keys with the remote one, see if any change in [collaboration](/collaboration) (you are not part of team anymore?) happened. If not, have a look at out [status page](https://status.fortrabbit.com) — maybe it's us, not you. Also, don't hesitate to contact our support as well.

### When you've got global SSH Agent Forwarding running

```
A B O R T E D
!! Unsupported request type "auth-agent-req@openssh.com"

fatal: Could not read from remote repository.
Please make sure you have the correct access rights and the repository exists.
```

It could be that you've inadvertently got SSH Agent Forwarding enabled, [this is probably not what you want](https://heipei.github.io/2015/02/26/SSH-Agent-Forwarding-considered-harmful/).

The quick solution would be to disable globally SSH agent forwarding by adding `ForwardAgent no` at the top of the file `~/.ssh/config`, although this could break some other SSH connections.

If you only want to disable it for fortrabbit, then add the following to your `~/.ssh/config` file.

```
Host deploy.*.frbit.com
  ForwardAgent no
```

- - -

## About SSH key authentication

In case you haven't worked with SSH keys before — you might be interested to understand how it works. The bottom line is that SSH key authentication is a bit nerdy, but actually both: convenient and secure. "SSH keys are a way to identify trusted computers, without involving passwords." That's from GitHub and probably the shortest way to explain what it is about and the most crucial benefit.

In public key authentication you have a key pair that consists of a public (eg `id_rsa.pub`) and a private key (eg `id_rsa`). What is encrypted with one (eg the public key) can be decrypted by the other (then: the private key). Further, having only the public key [does not allow you to derive the private key](https://en.wikipedia.org/wiki/List_of_unsolved_problems_in_mathematics). Hence you can safely "give out" your public key.

When you install your public key with fortrabbit it can be used to authenticate you: your SSH clients uses your private key to encrypt plain text data, which is then decrypted, using your public key, on the fortrabbit SSH server. If this decryption succeeds, then it must have been encrypted by your private key and you are let in.


## Advanced usage

Dive deeper, learn some more about the SSH key integration on fortrabbit.

### Account SSH keys

**This is the recommended method:** you store and manage your public SSH keys with your user Account on fortrabbit. This way you always have up-to-date code access on each App you own or you are collaborating with. It also makes managing collaboration easy — add/remove collaborators and code access is handled "automagically".

### App-only SSH keys

In certain cases you might want to add code access to an App without the need to register a new Account with fortrabbit. One case is some hectic ad-hoc hot-fix scenario (good luck!), another case is that you have some advanced deployment with a third party continuous integration service bot going on. So you can install additional App-only custom public SSH keys with each App. You manage those App-only SSH keys in the Dashboard.

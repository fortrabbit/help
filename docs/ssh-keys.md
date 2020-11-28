---

template:      article
reviewed:      2020-11-28
naviTitle:     SSH key setup
title:         SSH key setup
lead:          This article helps setting up your SSH keys on your local machine.
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

Besides [password authentication](/access-methods#toc-password-authentication) you can use SSH keys to authenticate with fortrabbit services, including **[deploying via Git](git)**, [accessing live logs](logging-pro) and [remote MySQL access](mysql#toc-remote-mysql-access). SSH key authentication is more secure and arguably more convenient. Additionally, it allows several users-accounts to collaborate on one or more Apps.

The goals here are:

1. Create an SSH key pair consisting of a public and private key.
2. Store the keys in the right location, so that your Operating System can make use of them.
3. Save the public key with your fortrabbit Account


## Do you have any SSH keys already?

To check if you have any existing SSH keys installed: Open a terminal (Mac OS & Linux) or a Git Bash (Windows) and type:

```
ls ~/.ssh
```

If you see an existing key pair listed (for example `id_rsa.pub` and `id_rsa`) that you would like to use to connect to fortrabbit, you can skip the generation and add the public SSH key (contents of `id_rsa.pub`) to your fortrabbit Account in the Dashboard (see below).

If you don't have any key pairs or you receive an error that `~/.ssh` doesn't exist, then you will need to generate new keys.

On Windows: If you don't know what the Git Bash is, or you have a different setup, check our recommendation on the [Git article](git).


## Generate SSH keys

Currently, we support only RSA keys. Please use 4096 bit keys, or longer.

```
$ ssh-keygen -t rsa -b 4096 -C user@fortrabbit -f ~/.ssh/id_rsa_fortrabbit
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/jaroslav/tmp/key.
Your public key has been saved in /home/jaroslav/tmp/key.pub.
The key fingerprint is:
SHA256:WmTkXErKkJvmFW5GUkg5JjR9BnMi+FEK79dPqehTFZM user@fortrabbit
The key's randomart image is:
+---[RSA 4096]----+
|.o++B*o o..      |
|.oo+O*=*Eo       |
| .o+ Xo.*o       |
| .. +.=o..       |
|  .o.+..S        |
|   ....*         |
|    ..o .        |
|   ..            |
|    ..           |
+----[SHA256]-----+
```

You may omit -f and the file-path to use the default location.


### SSH keys generation on Windows

The procedure to create SSH keys is slightly different on *nix (Mac OS, Linux) systems and Windows.

There are different ways to set up and use Git with Windows, thus there are also different ways set up and store the keys. We recommend using the official installer from the Git website, together with Git Bash. Check out this concise tutorial from GitHub:

* **[Generate a new SSH key & add it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-windows)**

You can also have a look at the very detailed [Git and SSH key setup on Windows from Beanstalk](http://guides.beanstalkapp.com/version-control/git-on-windows.html) to learn about alternative ways.


### SSH keys generation on Mac OS & Linux

This tutorial from GitHub should cover your needs:

* [Generate a new SSH key & add it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-mac)


## Save your public SSH keys with your fortrabbit Account

After you have set up Git and created your local SSH key and stored it in the correct place, you'll need to tell fortrabbit about that. Navigate to:

* Dashboard > Your Account > Access methods > Add a new SSH key

You might be asked to re-enter your fortrabbit Account password to do this. Then you will see a form with a textarea to paste the public key in to. Unless you chose a different file name for your key, this should be the contents of the file `~/.ssh/id_rsa.pub`.

This is what a valid SSH key looks like (don't paste this):

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbez9IDLYECMpQUQgNTWPG5aPMwJFNNP3a0
gAHVz8+N4HgiwFwBll2iUX0YPHIpbfeXN4Kab30qsevw59cjQ1XC7yjkrXy03OyOv/Z9X+KpB
vnf/cRXwz2zxfQqwvmXIQl3jlxyuA+Y4VjvELIvCrnnsfJDETmF8HZG4zA5XFfS95y5xx3TF9
S/eTlx2qWrmhsf20H+P/FK8otXKa+EW4UY6mew/lVxboEYDfCTju8cS5raJBmTehBaYyWI2dy
9oEWvD+qySvrEf1gXRRAMmt0/bOR4jw8G18i5siMtse2s/qMomG08VMeVIAEK9Tp64Mx4mmQv
IvP1bffus+WdY75 you@localhost
```

**Make sure you paste the public part of the key and not the private.**

The above is multi-line only for readability. Please remove the line breaks from the SSH key when adding it to the Dashboard. The key must start with `ssh-rsa`, other key types are not supported with our services.

You can also [import your GitHub keys](/access-methods#toc-github-ssh-key-import).


### SSH keys under Windows with PuTTY

We advise not using PuTTY for this. If you generated an SSH key with PuTTY, you will need to make sure that the private key is saved in the location where `git.exe` or `ssh.exe` are looking for it. These questions and answers may help you if you want to use PuTTY.

* [Stack Overflow: Where to find my private RSA key?](http://serverfault.com/questions/194567/how-do-i-tell-git-for-windows-where-to-find-my-private-rsa-key)
* [Superuser: Where does Putty store known_hosts information on Windows?](http://superuser.com/questions/197489/where-does-putty-store-known-hosts-information-on-windows)

Keep in mind that PuTTY uses a custom .ppk format for storing keys. The PuTTY Private Key format does not work with the OpenSSH ssh client or Git. It is possible extract/convert the private key from a .ppk file with the PuttyGen utility included with PuTTY.

* [Superuser: How to convert .ppk to openssh](https://superuser.com/questions/232362/how-to-convert-ppk-key-to-openssh-key-under-linux)


## About SSH key authentication

SSH key authentication is a more secure alternative to plain old passwords. The main concept is that instead of a short password, one uses a key file which is virtually impossible to guess. You give us the public part of your key and when logging in it will be used, together with the private key and username, to verify your identity.

The public part of the key, ending in `.pub` is safe to give out, while the private part should not be shared.

After you import a public key into your fortrabbit account, it can then be used to authenticate you. When an SSH client connects, the server will encrypt some secret using the public key. If the client is able to decrypt the secret with the private key and send it back to the server, then the server knows that the client has the private key and can be trusted.


## Account SSH keys

Your Account on fortrabbit may store several SSH keys. We recommend you use this method for authentication. Doing so allows you to push code, view logs, and connect to the MySQL database for all of your Apps. If you lose your key and import a new one, then the old one will no longer be valid. The benefit of this is you do not have to update passwords.

In addition, other users who have an Account on fortrabbit may collaborate on your Apps if you add them as a collaborator. Their SSH keys will then be allowed to deal with your App.


## App-only SSH keys

In certain scenarios, you may want to grant someone or something access to an App without creating a new Account. For these purposes, you can use App-only keys that are separate from any Account on fortrabbit. The App-only keys are managed via the Dashboard.


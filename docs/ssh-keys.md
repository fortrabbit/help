---

template:      article
reviewed:      2020-03-18
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

Besides [password authentication](/access-methods#toc-password-authentication) you can use SSH keys to authenticate with fortrabbit services, including **[deploying via Git](git)**, [accessing live logs](logging-pro) and [remote MySQL access](mysql#toc-remote-mysql-access). SSH key authentication is more secure and arguably more convenient. Additionally, it allows several users-accounts to collaborate on one or more Apps.

The goals here are:

1. Create an SSH key pair consisting of public and private key.
2. Store the keys on the right location, so that your Operating System can make use of them.
3. Save the public key with your fortrabbit Account


## Do you have any SSH keys already?

To check if you have any existing SSH keys installed: Mac Os & Linux: Open a terminal (Mac OS & Linux) or a Git Bash (Windows) and type:

```
ls ~/.ssh
```

If you see an existing key pair listed (for example `id_rsa.pub` and `id_rsa`) that you would like to use to connect to fortrabbit, you can skip the generation and add the public SSH key (contents of `id_rsa.pub`) to your fortrabbit Account in the Dashboard, see below.

If you don't have any key pairs or you receive an error that ~/.ssh doesn't exist, then you will need to generate new keys.

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

### Supported key types


### SSH keys generation on Windows

The procedure to create SSH keys is slightly different on *nix (Mac OS, Linux) systems and Windows.

There are different ways to setup and use Git with Windows, thus there are also different ways setup and store the keys. We recommend to use the official installer form the Git website, together with Git Bash. Check out this consise tutorial from GitHub:

* **[Generate a new SSH key & add it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-windows)**

You can also have a look at the very detailed [Git and SSH key setup on Windows from Beanstalk](http://guides.beanstalkapp.com/version-control/git-on-windows.html) to learn about alternative ways.


### SSH keys generation on Mac OS & Linux

This tutorial from GitHub should cover your needs:

* [Generate a new SSH key & add it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-mac)



## Save your public SSH keys with your fortrabbit Account

After you have set up Git and created your local SSH key and stored it on the correct place, you'll need to tell fortrabbit about that. Navigate to:

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

- - -


## Troubleshooting

The following section covers some errors you may encounter when using an SSH key with our services. Please also see the [general connection trouble shooting](/access-methods#toc-troubleshooting) before.

### Permission denied error

If you see this after issuing the commands `git push` or `git pull`,

```
Permission denied (publickey).
fatal: Could not read from remote repository.
```

then you the key you are using is not associated with your Dashboard account. Verify that you have imported your key into your fortrabbit account and that [Git](git) is correctly installed on your machine. It may take a upto two minutes for a new key to get activated, after it is imported. Usually it works within 30 seconds. 

If you still see this error, please review the ssh-setup tutorials above.


### SSH keys under Windows with PuTTY

We advice not to use PuTTY for this. If generate an SSH key with PuTTY, you will need to make sure that the private key is saved in the location where `git.exe` or `ssh.exe` are looking for it. These questions and answers may help you if you want to use PuTTY.

* [Stack Overflow: Where to find my private RSA key?](http://serverfault.com/questions/194567/how-do-i-tell-git-for-windows-where-to-find-my-private-rsa-key)
* [Superuser: Where does Putty store known_hosts information on Windows?](http://superuser.com/questions/197489/where-does-putty-store-known-hosts-information-on-windows)

Keep in mend that putty uses a custom .ppk format for storing keys. The Putty Private Key format does not work with the OpenSSH ssh client or Git. It is possible extract/convert the private key from a .ppk file with the PuttyGen utility included with PuTTY.

* [Superuser: How to convert .ppk to openssh](https://superuser.com/questions/232362/how-to-convert-ppk-key-to-openssh-key-under-linux)

### If you can not get the keys to work

If you tried everyting and it is still not working, you can revert back to password authentication. Read this: [remove the public keys from your fortrabbit Account](access-methods#toc-how-to-change-from-ssh-key-to-password-authentication)


### If it worked before and suddenly stops working

If you have deployed using SSH keys before and now it doesn't work any more: please check if you have changed something, compare your local keys with the remote one, see if any change in [collaboration](/collaboration) (you are not part of team anymore?) happened. If not, have a look at out [status page](https://status.fortrabbit.com) — maybe it's us, not you. Also, don't hesitate to contact our support as well.

<!--

-- The use case described in agent-considered-harmful article does not apply to our clients.
-- If anyone, we should use these precautions, not our clients. Also, the next paragraph suggests using the agent. We either recommend it or we don't.
-- It's fine for out clients to use this feature.


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


-->


### You are asked for a password when using a key and the Account password isn't working

In this case, you have probably used a passphrase with your key. This has nothing to do with the fortrabbit Account or services. When a new key is generated, there is usually this prompt: `Enter passphrase for key`. You want to use whatever you said then.

To avoid typing this may times per day, you can use the `ssh-agent`. See this [help article on GitHub](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases) to find out how.


- - -

<!--
## About SSH key authentication

I disagree with the language here.
SSH keys are not nerdy, passwords are simpleminded and very 1960s 

I don't like this section.
The third paragraph is wrong.
The first is a massive oversimplification, and written in with a lot of bloat.
The second repeats information from other places in this document.

There is no need to explain ssh-keys here. Send people to the wikipedia page, please! or this:
https://www.ssh.com/ssh/public-key-authentication


In case you haven't worked with SSH keys before — you might be interested to understand how it works. The bottom line is that SSH key authentication is a bit nerdy, but actually both: convenient and secure. "SSH keys are a way to identify trusted computers, without involving passwords." That's from GitHub and probably the shortest way to explain what it is about and the most crucial benefit.

In public key authentication you have a key pair that consists of a public (eg `id_rsa.pub`) and a private key (eg `id_rsa`). What is encrypted with one (eg the public key) can be decrypted by the other (then: the private key). Further, having only the public key [does not allow you to derive the private key](https://en.wikipedia.org/wiki/List_of_unsolved_problems_in_mathematics). Hence you can safely "give out" your public key.

When you install your public key with fortrabbit it can be used to authenticate you: your SSH clients uses your private key to encrypt plain text data, which is then decrypted, using your public key, on the fortrabbit SSH server. If this decryption succeeds, then it must have been encrypted by your private key and you are let in.
-->


<!--
-- this is the basic setup, already explained at the top of this document!

## Advanced usage

Dive deeper, learn some more about the SSH key integration on fortrabbit. 

-- what is up-to-date code access?
-- I added "you can collaborate" at the top.
-- Drop this paragraph. Less is more.

### Account SSH keys

**This is the recommended method:** you store and manage your public SSH keys with your user Account on fortrabbit. This way you always have up-to-date code access on each App you own or you are collaborating with. It also makes managing collaboration easy — add/remove collaborators and code access is handled "automagically".
-->


## App-only SSH keys

In certain scenarios, you may want to grant someone or something access to an App without creating a new Account. For these purposes, you can use App-only keys that are separate from any Account on fortrabbit. The App-only keys are managed via the Dashboard.


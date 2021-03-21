---

template:      article
reviewed:      2021-03-21
naviTitle:     SSH key troubleshooting
title:         SSH key troubleshooting
excerpt:       What you may want to know to debug SSH key issues
lead:          'This article aims to help you finding common issues when trying to connect to fortrabbit deploy services using SSH key authentication.'
group:         troubleshooting
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

## Read first please

Please make sure to understand the available [deployment methods](/deployment-methods) and [code access methods](/access-methods) on fortrabbit. Make also sure to read [SSH key setup guide](ssh-keys).


## Common issues

Here are the most common cases why SSH public key authentication isn't working. Please mind the difference between SSH as a protocol and the authentication method. You can use SSH key authentication for SSH, Git and to connect to the database. We suggest to try to connect by SSH first, this is the fastest way to debug issues.


### You are asked for a password

If you are using the SSH public key authentication and you still face a password prompt it can mean a few things:

+ Your SSH key is protected by a local passphrase
+ The rsa key in standard location (~/.ssh/id_rsa) has been rejected
+ All keys in your ssh-agent have been rejected


#### Local passphrases

If your local SSH key is protected by a passphrase, then type that in. We do not know and can not know what you picked as the passphrase for your key. It may or may not be the same as your fortrabbit Account password.


### Rejected keys

If your keys have no passphrases or if they are already unlocked in the agent but have all been rejected, then the SSH server will still ask you for a password. This is because it must support both access modes. If you are using the SSH key access mode, typing in any password will not grant you access, even if you type in your fortrabbit Account password.

Essentially, only one access mode can be active at any particular time. This is explained on the [access methods page](/access-methods).

Likely your SSH client has offered keys that are not known at fortrabbit. Please make sure to have saved the SSH keys in standard locations.


#### Specify a different key than the default

If you use an unprotected key (no passphrase) and still get asked about a password, it may be the case that the key in the default location is not imported into fortrabbit.

To use a specific key run SSH like this:

```bash
$ ssh -i ~/.ssh/id_rsa_fortrabbit {{app-name}}@deploy.{{region}.frbit.com
```

Adjust the path of the key (`~/.ssh/id_rsa_fortrabbit`). It needs to point to private key that you actually have on your machine.



### Other configuration issues

We have seen cases with uncommon SSH configurations. Check your SSH configuration.

```bash
# terminal command to edit your ssh config file
nano ~/.ssh/config
```

Within this file change the preferred authentications accordingly:

```bash
# when using password authentication use:
PreferredAuthentications    password,publickey

# when using SSH key authentication use:
PreferredAuthentications    publickey,password
```


### Connecting from within a container

When your local development environment is containerized with Docker or Vagrant or alike and you want to deploy from within the container, make sure to have the SSH keys installed there as well.


### Using verbose mode to see errors

When trying to connect add the `-v` to show a full connection log. This might help finding the issue.

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com -v
```


### If it worked before and suddenly stops working

If you have deployed using SSH keys before and now it doesn't work any more: 

Please check if you have changed something, compare your local keys with the remote one, see if any change in [collaboration](/collaboration) happened (e.g. you are not part of a team anymore?). If not, have a look at our [status page](https://status.fortrabbit.com) — maybe it's us, not you. Also, don't hesitate to contact our support as well.


### If you still can not get the keys to work

If you tried everything and it is still not working, you can revert back to password authentication.
Read this: [remove the public keys from your fortrabbit Account](access-methods#toc-how-to-change-from-ssh-key-to-password-authentication)



## SSH keys under Windows with PuTTY

If you generated an SSH key with PuTTY, you will need to make sure that the private key is saved in the location where `git.exe` or `ssh.exe` are looking for it or take steps to specify where the key is. Additionally, PuTTY uses a special format to store the private key, which also requires additional steps to use with other tools.

Because of these reasons and previous experience with clients, we advise against using PuTTY. For more info, check out these questions and answers if you want to use PuTTY anyway.

* [Stack Overflow: Where to find my private RSA key?](http://serverfault.com/questions/194567/how-do-i-tell-git-for-windows-where-to-find-my-private-rsa-key)
* [Superuser: Where does Putty store known_hosts information on Windows?](http://superuser.com/questions/197489/where-does-putty-store-known-hosts-information-on-windows)

Keep in mind that PuTTY uses a custom `.ppk` format for storing keys. The PuTTY Private Key format does not work with the OpenSSH ssh client or Git. It is possible extract/convert the private key from a .ppk file with the PuttyGen utility included with PuTTY.

* [Superuser: How to convert .ppk to openssh](https://superuser.com/questions/232362/how-to-convert-ppk-key-to-openssh-key-under-linux)
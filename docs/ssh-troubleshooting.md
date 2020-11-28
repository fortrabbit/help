---

template:      article
reviewed:      2020-11-28
naviTitle:     SSH troubleshooting
title:         SSH troubleshooting
excerpt:       What you may want to know to debug SSH issues
lead:          'This article aims to help you finding common issues when deploying with SSH on fortrabbit.'
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

Please make sure to understand the available [deployment methods](/deployment-methods) and [code access methods](/access-methods) on fortrabbit. When you are using SSH key authentication, make sure to read [SSH key setup](ssh-keys).

## Common reasons and solutions

Can't login by SSH? Here are some common issues users often run into.


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

Please see our [SSH key setup guides](/ssh-keys) to setup SSH key authentication correctly.


### Multiple Accounts with different access methods issues

When you have multiple Accounts here at fortrabbit, or you have had an Account here before and you have used one access method to identify, your SSH config might saves our host to use that access method for our host. Now when using a different method with a different Account, your computer might still need the old saved one.


### If you can not get the keys to work

If you tried everything and it is still not working, you can revert back to password authentication. Read this: [remove the public keys from your fortrabbit Account](access-methods#toc-how-to-change-from-ssh-key-to-password-authentication)


### If it worked before and suddenly stops working

If you have deployed using SSH keys before and now it doesn't work any more: please check if you have changed something, compare your local keys with the remote one, see if any change in [collaboration](/collaboration) happened (e.g. you are not part of a team anymore?). If not, have a look at our [status page](https://status.fortrabbit.com) — maybe it's us, not you. Also, don't hesitate to contact our support as well.


### You are asked for a password when using a key and the Account password isn't working

In this case, you have probably used a passphrase with your key. This has nothing to do with the fortrabbit Account or services. When a new key is generated, there is usually this prompt: `Enter passphrase for key`. You want to use whatever you said then.

To avoid typing this may times per day, you can use the `ssh-agent`. See this [help article on GitHub](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases) to find out how.

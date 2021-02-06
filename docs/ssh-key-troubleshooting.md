---

template:      article
reviewed:      2021-02-06
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

These are the most common cases when SSH key authentication isn't working. Please mind the difference between SSH as a protocol and the authentication method. You can use SSH key authentication for SSH, Git and to connect to the database. We suggest to try to connect by SSH first, this is the fastest way to debug issues.


### You are asked for a password

When you have SSH keys set up correctly. It should not ask for a password at. When it asks for a password, something is wrong. Don't try enter a password. This will not work. Your local machine is not correctly set up correctly. Check your configuration.

+ See if you have the SSH keys in the correct folder
+ See if you have the correct key pairs

Check and correct your SSH configuration like so:

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

#### Local passphrases

You might have used a passphrase to protect your key. This has nothing to do with the fortrabbit Account or services. When a new key is generated, there is usually this prompt: `Enter passphrase for key`. You want to use whatever you said then.

To avoid typing this may times per day, you can use the `ssh-agent`. See this [help article on GitHub](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases) to find out how.


#### Connecting from within a container

When your local development environment is containerized with Docker or Vagrant or alike and you want to deploy from within the container, make sure to have the SSH keys installed there as well.

### Use verbose mode to see errors

When trying to connect add the `-v` to show a full connection log. This might help finding the issue.

```shell
  ssh {{app-name}}@deploy.{{region}}.frbit.com -v
```

### Multiple Accounts with different access methods issues

When you have multiple Accounts here at fortrabbit, or you have had an Account here before and you have used one access method to identify, your SSH config might saves our host to use that access method for our host. Now when using a different method with a different Account, your computer might still need the old saved one.


### If it worked before and suddenly stops working

If you have deployed using SSH keys before and now it doesn't work any more: please check if you have changed something, compare your local keys with the remote one, see if any change in [collaboration](/collaboration) happened (e.g. you are not part of a team anymore?). If not, have a look at our [status page](https://status.fortrabbit.com) — maybe it's us, not you. Also, don't hesitate to contact our support as well.


### If you can not get the keys to work

If you tried everything and it is still not working, you can revert back to password authentication. Read this: [remove the public keys from your fortrabbit Account](access-methods#toc-how-to-change-from-ssh-key-to-password-authentication)

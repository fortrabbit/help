---

template:      article
reviewed:      2020-11-28
naviTitle:     SSH troubleshooting
title:         SSH troubleshooting
lead:          Learn about the different authentication methods with fortrabbit.
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
---

template:      article
reviewed:      2020-11-28
naviTitle:     SSH troubleshooting
title:         SSH troubleshooting
excerpt:       What you may want to know to debug SSH connection issues
lead:          'This article aims to help you finding common issues establishing an SSH connection on fortrabbit.'
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

Please make sure to understand the available [deployment methods](/deployment-methods) and [code access methods](/access-methods) on fortrabbit. 

## Common reasons and solutions

Can't login by SSH? Here are some common issues users often run into.

### SSH key issues

If you have trouble setting up a connection, using SSH keys as your authentication, method - especially when you are doing this for the first time - see our [SSH key troubleshooting guide](ssh-key-troubleshooting).


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


### Use verbose mode to see errors

When trying to connect add the `-v` to show a full connection log. This might help finding the issue.

```shell
  ssh {{app-name}}@deploy.{{region}}.frbit.com -v
```

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


**Related topcs**

- [Code access methods](/access-methods) on fortrabbit
- [Deployment methods](/deployment-methods) on fortrabbit.
- [SFTP access](/sftp-uni)
- [SFTP troubleshooting](/sftp-troubleshooting)
- [SSH access troubleshooting](/ssh-troubleshooting)
- [SSH key setup guide](ssh-keys)
- [SSH key setup](ssh-keys)


### You are asked for a password

See this page: [SFTP troubleshooting: ssh-key-mode](sftp-troubleshooting#toc-option-2-strong-ssh-key-mode-strong-)

### Specify a different key than the default

If you use an unprotected key and still get asked about a password, it may be the case that the key in the default location is not imported into fortrabbit.
To use a specific key run ssh like this:

    $ ssh -i ~/.ssh/id_rsa_fortrabbit tripple-w-app@deploy.eu2.frbit.com

Adjust the path of the key (`~/.ssh/id_rsa_fortrabbit`). It needs to point to private key that you actually have on your machine.

### Connecting from within a container

When your local development environment is containerized with Docker or Vagrant or alike and you want to deploy from within the container, make sure to have the SSH keys installed there as well.

### Use verbose mode to see errors

When trying to connect add the `-v` to show a full connection log. This might help finding the issue.

```shell
  ssh {{app-name}}@deploy.{{region}}.frbit.com -v
```

### If it worked before and suddenly stops working

If you have deployed using SSH keys before and now it doesn't work any more: please check if you have changed something, compare your local keys with the remote one, see if any change in [collaboration](/collaboration) happened (e.g. you are not part of a team anymore?). If not, have a look at our [status page](https://status.fortrabbit.com) — maybe it's us, not you. Also, don't hesitate to contact our support as well.


### If you can not get the keys to work

If you tried everything and it is still not working, you can revert back to password authentication.
Read this: [remove the public keys from your fortrabbit Account](access-methods#toc-how-to-change-from-ssh-key-to-password-authentication)


<!--
not a troubleshooting topic.

### Local passphrases

You might have used a passphrase to protect your key. This has nothing to do with the fortrabbit Account or services. When a new key is generated, there is usually this prompt: `Enter passphrase for key`. You want to use whatever you said then.

To avoid typing this may times per day, you can use the `ssh-agent`. See this [help article on GitHub](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases) to find out how.

-->

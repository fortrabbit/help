---

template:      article
reviewed:      2021-03-21
title:         SFTP deployment
naviTitle:     SFTP deployment
lead:          Learn about the classical way to access files on your Universal App on fortrabbit.

stack:         uni
group:         deployment

keywords:
    - beginner
    - deployment
    - dreamweaver
    - FTP
    - FTPS

---


## SFTP access steps

1. Grab the access credentials from the fortrabbit Dashboard:  
   Apps > {{appname}} > Access > SFTP
2. Copy paste the credentials into your SFTP client
3. Connect and upload files


### SFTP access details

```nohighlight
Host:       deploy.{{region}}.frbit.com
User name:  {{ssh-user}}
Password:   Your Account password OR private SSH key
Protocol:   SFTP (not FTP)
Port:       22
```

**When using username + password authentication**: The user name will have a random string after the app-name like `appname.f4n4gkrx90ot4yxm.`. In this case you will need to provide your fortrabbit Account password.

**When using SSH key authentication**: in most cases you can leave the password field empty. The SFTP client will look up the keys. In rare cases you might need to specify the exact key by showing the client the path to the key.


## SFTP clients

There are various SFTP clients out there. We recommend [Cyberduck](https://cyberduck.io/) (on Mac and Windows). Modern editors and IDEs have support for SFTP, including Sublime, PhpStorm, Notepad++ and so on. In a hurry, you can use `sftp` from a terminal which should be installed if `ssh` is installed.


## Other deployment methods

It is also possible to [deploy with Git](git-deployment) here and also use [SSH](/ssh-uni). It is not recommended to mix the different deployment methods. Please see our [deployment methods article](deployment-methods-uni) to learn how the different ways to deploy code work side by side.


## Some more about SFTP

SFTP is short for SSH File Transfer Protocol. It is used for uploading and downloading files over a SSH connection. Despite the similar name, SFTP is very different than FTP or FTPS but for most practical purposes they seem very similar. SFTP is preferable to FTP because the the transferred data is encrypted and not visible to everyone on the network.


## Troubleshooting SFTP

Having issues? Please see our [SFTP troubleshooting page](/sftp-troubleshooting).
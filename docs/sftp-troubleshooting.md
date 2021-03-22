---

template:      article
reviewed:      2021-03-19
naviTitle:     SFTP troubleshooting
title:         SFTP troubleshooting
excerpt:       What you may want to know to debug SFTP issues
lead:          'This article helps solving common issues when trying to connect by SFTP to on a Universal App hosted on fortrabbit.'
group:         troubleshooting
stack:         all

keywords:
     - ftp
     - username
     - password
     - access
     - authentication

---


Are you seeing a "**Permission denied, please try again**" error when trying to connect by SFTP to fortrabbit? Please proceed like so:

## Check your access method first

Make sure that you know which access method you are currently using. We have "username + password" or "SSH public key authentication". If you are unsure, please please see the section about [figuring out which access method is currently active](access-methods#toc-available-authentication-methods) for your fortrabbit Account.
[]

### Tips to proceed when username + password is enabled

If you are using the username + password access mode: 

* the username will look like this `{{app-name}}.{{randomletters}}`
* use your fortrabbit Account password as your SFTP password


Make sure to copy-paste the correct credentials for hostname and user from the Dashboard. Find the Access to your App.


### Tips to proceed when SSH public key is enabled

If you are using the SSH public key authentication and still face a password prompt it can mean a few things:

#### General SSH connection issues

Often SFTP problems are general connection issues. That's why we advice to test if SSH connections are working for you. This is easier to debug.

* [See here](ssh-uni) how to connect by SSH
* [See here](ssh-troubleshooting) if you have trouble connecting by SSH
* [See here](ssh-key-troubleshooting) if the trouble is connected to your SSH keys
 

After you have solved connecting by SSH, it's likely that SFTP will also work. Continue reading here if SSH is working for you, but you can not get it work for SFTP.

### Settings with your SFTP client

Also double check the following settings with your SFTP client:

* Use SFTP as the protocol, nor FTP or something else
* Use standard port 22
* Make sure to have copied the correct credentials from our Dashboard
* When using SSH public key authentication you might need to point your SFTP client to the location of the key, a password is not required in that case


### Other general connection issues

If it hangs for a long time while trying to connect, then it is possible that you have problems dialing in. Please see our [SSH troubleshooting guide](/ssh-troubleshooting#toc-general-connection-errors) for common cases.


### Problems logging in to a Pro App by SFTP

There is no SFTP for Pro Apps. There is only [remote SSH execution](/remote-ssh-execution-pro).


## Related topics

- [Code access methods](/access-methods) on fortrabbit
- [Deployment methods](/deployment-methods) on fortrabbit
- [SFTP access](/sftp-uni)
- [SSH access troubleshooting](/ssh-troubleshooting)
- [SSH key setup](ssh-keys)
- [SSH key troubleshooting](/ssh-key-troubleshooting)

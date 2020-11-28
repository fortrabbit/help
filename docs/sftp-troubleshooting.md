---

template:      article
reviewed:      2020-11-28
naviTitle:     SFTP troubleshooting
title:         SFTP troubleshooting
excerpt:       What you may want to know to debug SFTP issues
lead:          'This article helps solving common issues when trying to connect by SFTP to on an App hosted on fortrabbit.'
group:         troubleshooting
stack:         all

keywords:
     - ftp
     - username
     - password
     - access
     - authentication

---

## Read first please

Please make sure to understand the available [deployment methods](/deployment-methods) and [code access methods](/access-methods) on fortrabbit. When you are using SSH key authentication, make sure to read [SSH key setup](ssh-keys) and [SSH troubleshooting](/ssh-troubleshooting) guides first. Also make sure to have read the main [SFTP article](/sftp-uni).

## Common reasons and solutions

Can't login by SFTP? Here are some common issues users often run into.


### FTP is not working

fortrabbit is only support the secure **SFTP** protocol. Not FTP. Please check if your SFTP client connection is configured to use SFTP.


### My password is not working

Make sure to grab the correct credentials from the Dashboard. Sometimes this also happens when you have incorrectly configured your SSH keys. The fortrabbit service is setup to use SSH keys, but your computer is not correctly sending these over. It will try password after that, but that fail whatever you enter, since it's only configured to use SSH keys. 

Make sure to follow all guides linked above under "read first please".


### I can not login to my Pro App by SFTP

There is no SFTP for Pro Apps. There is only [remote SSH execution](/remote-ssh-execution-pro).
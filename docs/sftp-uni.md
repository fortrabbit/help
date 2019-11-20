---

template:      article
reviewed:      2019-09-30
title:         SFTP
naviTitle:     SFTP
lead:          Learn about the classical way to deploy and access your App on fortrabbit.

stack:         uni
group:         deployment

keywords:
    - beginner
    - deployment
    - dreamweaver
    -

---

Albeit [deploying with Git](git-deployment) has many advantages, sometimes it's more of a burden than it helps. Especially when working with CMS. These systems are often not designed for Git-workflows and write on the file system when installing plugins and upgrading versions via web-based interfaces. Those changes can not be back-ported to the version control. Stuff gets out of control.

All [Universal Stack Apps](app-uni) come with SFTP access out-of-the-box to support legacy applications and workflows.


## Accessing SFTP

There are various GUIs out there, which make your life easier. We recommend [Cyberduck](https://cyberduck.io/) (Mac, Windows).

Many modern editors or IDEs also feature SFTP integrations by plugin.

* **Mode**: SFTP (not regular FTP)
* **Host**: deploy.{{region}}.frbit.com
* **Port**: 22
* **Username**: {{ssh-user}}
* **Password**: Your Account password OR public SSH key


### File synchronization

Most SFTP clients feature a file synchronization mode. You can choose your local folder and sync it to the remote folder on fortrabbit. All files will be compared and only changed ones will be uploaded. This works in the other direction as well, of course.

#### Setting up SFTP file sync

This example shows how to configure your SFTP client to quickly sync code:

1. Create a bookmark (favorite) that stores the access informations to connect with one click
2. Set a local directory to be synced with the online directory in the bookmark
3. Set rules for exclude patterns (a huge time saver!)
4. Run simulation first, the first run will take longer, results will be cached, execution will be fast
5. First sync down, then sync up

The workflow has been tested with (macOS commercial) SFTP client Transmit from Panic.


## Troubleshooting SFTP

Got an error when trying to login? fortrabbit supports username + password and public key authentication. Please continue here to troubleshoot access:

* [See the access methods article](/access-methods)

### Blacklisting

When nothing works any more, see if you are blacklisted, more [here](/troubleshooting#toc-blacklisting).

## Further readings

### About SFTP

SFTP stands for SSH File Transfer Protocol. It's a separate protocol packaged with [SSH](/ssh-uni) — think of it as the little sister of SSH. SFTP is very different than FTP or FTPS but all clients will speak it anyways, so for the usage it doesn't really makes a difference. Mostly, SFTP is preferable to FTP because of its underlying security features.

### Mixing deployment methods

Please see our [deployment methods article](deployment-methods-uni) to learn how the different ways to deploy code work side by side.

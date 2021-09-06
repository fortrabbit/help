---

template:      article
reviewed:      2021-03-22
naviTitle:     SSH keys
title:         SSH key troubleshooting
excerpt:       What you may want to know to debug SSH key issues
lead:          'This article aims to help you finding common issues when trying to connect to fortrabbit deploy services such as SFTP and SSH using SSH public key authentication.'
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

If you are using the SSH public key authentication and you still face a password prompt it can mean a few things. In most cases this is related to your local configuration:

+ Your SSH key is protected by a local passphrase
+ The rsa key in standard location `~/.ssh/id_rsa` has been rejected
+ All keys in your ssh-agent have been rejected
+ Other configuration issues on your side


#### Key passphrases

If your local SSH key is protected by a key passphrase, then type that in. We do not know and can not know what you picked as the passphrase for your key. It likely is NOT the same as your fortrabbit Account password.


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

# Unsuccessful login example output

# OpenSSH_8.4p1, OpenSSL 1.1.1i  8 Dec 2020
# debug1: Reading configuration data /home/user/.ssh/config
# debug1: /home/user/.ssh/config line 6: Applying options for *
# debug1: Reading configuration data /home/user/.ssh/config-fortrabbit
# debug1: Reading configuration data /etc/ssh/ssh_config
# debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
# debug1: Connecting to deploy.eu2.frbit.com [52.212.159.57] port 22.
# debug1: Connection established.
# debug1: identity file /home/user/.ssh/keys/rusty-2020-user-generic.ed25519 type 3
# debug1: identity file /home/user/.ssh/keys/rusty-2020-user-generic.ed25519-cert type -1
# debug1: identity file /home/user/.ssh/id_rsa type 0
# debug1: identity file /home/user/.ssh/id_rsa-cert type -1
# debug1: identity file /home/user/.ssh/id_ed25519 type -1
# debug1: identity file /home/user/.ssh/id_ed25519-cert type -1
# debug1: Local version string SSH-2.0-OpenSSH_8.4
# debug1: Remote protocol version 2.0, remote software version OpenSSH_6.6.1p2 Ubuntu-2ubuntu2
# debug1: match: OpenSSH_6.6.1p2 Ubuntu-2ubuntu2 pat OpenSSH_6.6.1* compat 0x04000002
# debug1: Authenticating to deploy.eu2.frbit.com:22 as 'tripple-w-app'
# debug1: SSH2_MSG_KEXINIT sent
# debug1: SSH2_MSG_KEXINIT received
# debug1: kex: algorithm: curve25519-sha256@libssh.org
# debug1: kex: host key algorithm: ssh-rsa
# debug1: kex: server->client cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
# debug1: kex: client->server cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
# debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
# debug1: Server host key: ssh-rsa SHA256:Z+Vtiym9ZzdhKWgUdKM/ZLjoR6jxNB51OcRLzH0uuJ4
# debug1: Host 'deploy.eu2.frbit.com' is known and matches the RSA host key.
# debug1: Found key in /home/user/.ssh/known_hosts:37
# debug1: rekey out after 4294967296 blocks
# debug1: SSH2_MSG_NEWKEYS sent
# debug1: expecting SSH2_MSG_NEWKEYS
# debug1: SSH2_MSG_NEWKEYS received
# debug1: rekey in after 4294967296 blocks
# debug1: Will attempt key: /home/user/.ssh/keys/rusty-2020-user-generic.ed25519 ED25519 SHA256:nvt77cGLmGL/bfTy8o# +Bj4AeEzmMQ7R+GAmwL8U/LoE explicit
# debug1: Will attempt key: /home/user/.ssh/id_rsa RSA SHA256:7dTAWY5hmXpmMegNAk2g3bmYXRaB5ZUPwIm4pWIhc7U explicit
# debug1: Will attempt key: /home/user/.ssh/id_ed25519  explicit
# debug1: SSH2_MSG_SERVICE_ACCEPT received
# debug1: Authentications that can continue: password,publickey
# debug1: Next authentication method: publickey
# debug1: Offering public key: /home/user/.ssh/keys/rusty-2020-user-generic.ed25519 ED25519 SHA256:nvt77cGLmGL/# bfTy8o+Bj4AeEzmMQ7R+GAmwL8U/LoE explicit
# debug1: Authentications that can continue: password,publickey
# debug1: Offering public key: /home/user/.ssh/id_rsa RSA SHA256:7dTAWY5hmXpmMegNAk2g3bmYXRaB5ZUPwIm4pWIhc7U # explicit
# debug1: Authentications that can continue: password,publickey
# debug1: Trying private key: /home/user/.ssh/id_ed25519
# no such identity: /home/user/.ssh/id_ed25519: No such file or directory
# debug1: Next authentication method: password
# tripple-w-app@deploy.eu2.frbit.com's password:


# Unsuccessful login example output

# OpenSSH_8.4p1, OpenSSL 1.1.1i  8 Dec 2020
# debug1: Reading configuration data /home/user/.ssh/config
# debug1: /home/user/.ssh/config line 6: Applying options for *
# debug1: Reading configuration data /home/user/.ssh/config-fortrabbit
# debug1: Reading configuration data /etc/ssh/ssh_config
# debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
# debug1: Connecting to deploy.eu2.frbit.com [52.212.159.57] port 22.
# debug1: Connection established.
# debug1: identity file /home/user/.ssh/id_rsa_fortrabbit type 0
# debug1: identity file /home/user/.ssh/id_rsa_fortrabbit-cert type -1
# debug1: identity file /home/user/.ssh/keys/rusty-2020-user-generic.ed25519 type 3
# debug1: identity file /home/user/.ssh/keys/rusty-2020-user-generic.ed25519-cert type -1
# debug1: identity file /home/user/.ssh/id_rsa type 0
# debug1: identity file /home/user/.ssh/id_rsa-cert type -1
# debug1: identity file /home/user/.ssh/id_ed25519 type -1
# debug1: identity file /home/user/.ssh/id_ed25519-cert type -1
# debug1: Local version string SSH-2.0-OpenSSH_8.4
# debug1: Remote protocol version 2.0, remote software version OpenSSH_6.6.1p2 Ubuntu-2ubuntu2
# debug1: match: OpenSSH_6.6.1p2 Ubuntu-2ubuntu2 pat OpenSSH_6.6.1* compat 0x04000002
# debug1: Authenticating to deploy.eu2.frbit.com:22 as 'tripple-w-app'
# debug1: SSH2_MSG_KEXINIT sent
# debug1: SSH2_MSG_KEXINIT received
# debug1: kex: algorithm: curve25519-sha256@libssh.org
# debug1: kex: host key algorithm: ssh-rsa
# debug1: kex: server->client cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
# debug1: kex: client->server cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
# debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
# debug1: Server host key: ssh-rsa SHA256:Z+Vtiym9ZzdhKWgUdKM/ZLjoR6jxNB51OcRLzH0uuJ4
# debug1: Host 'deploy.eu2.frbit.com' is known and matches the RSA host key.
# debug1: Found key in /home/user/.ssh/known_hosts:37
# debug1: rekey out after 4294967296 blocks
# debug1: SSH2_MSG_NEWKEYS sent
# debug1: expecting SSH2_MSG_NEWKEYS
# debug1: SSH2_MSG_NEWKEYS received
# debug1: rekey in after 4294967296 blocks
# debug1: Will attempt key: /home/user/.ssh/id_rsa_fortrabbit RSA # SHA256:QuLDaljPIExVTlsCZkyKiByU7p9RlKp8ABmJcyC5F54 explicit
# debug1: Will attempt key: /home/user/.ssh/keys/rusty-2020-user-generic.ed25519 ED25519 SHA256:nvt77cGLmGL/bfTy8o# +Bj4AeEzmMQ7R+GAmwL8U/LoE explicit
# debug1: Will attempt key: /home/user/.ssh/id_rsa RSA SHA256:7dTAWY5hmXpmMegNAk2g3bmYXRaB5ZUPwIm4pWIhc7U explicit
# debug1: Will attempt key: /home/user/.ssh/id_ed25519  explicit
# debug1: SSH2_MSG_SERVICE_ACCEPT received
# debug1: Authentications that can continue: password,publickey
# debug1: Next authentication method: publickey
# debug1: Offering public key: /home/user/.ssh/id_rsa_fortrabbit RSA # SHA256:QuLDaljPIExVTlsCZkyKiByU7p9RlKp8ABmJcyC5F54 explicit
# debug1: Server accepts key: /home/user/.ssh/id_rsa_fortrabbit RSA # SHA256:QuLDaljPIExVTlsCZkyKiByU7p9RlKp8ABmJcyC5F54 explicit
# debug1: Authentication succeeded (publickey).
# Authenticated to deploy.eu2.frbit.com ([52.212.159.57]:22).
# debug1: channel 0: new [client-session]
# debug1: Requesting no-more-sessions@openssh.com
# debug1: Entering interactive session.
# debug1: pledge: network
# debug1: Sending environment.
# debug1: Sending env COLORTERM = rxvt-xpm
# debug1: Sending env LANGUAGE = en_US.UTF-8
# debug1: Sending env LANG = en_US.UTF-8
# debug1: Sending env LC_ALL = en_US.UTF-8
# debug1: Sending command: uptime
# debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
#  07:42:49 up 41 days, 16:56,  0 users,  load average: 0.03, 0.08, 0.08
# debug1: channel 0: free: client-session, nchannels 1
# Transferred: sent 3864, received 1952 bytes, in 0.5 seconds
# Bytes per second: sent 7888.5, received 3985.1
# debug1: Exit status 0
```


### If it worked before and suddenly stops working

If you have deployed using SSH keys before and now it doesn't work any more: 

Please check if you have changed something, compare your local keys with the remote one, see if any change in [collaboration](/collaboration) happened (e.g. you are not part of a team anymore?). If not, have a look at our [status page](https://status.fortrabbit.com) — maybe it's us, not you. Also, don't hesitate to contact our support as well.


### If you still can not get the keys to work

If you tried everything and it is still not working, you can revert back to password authentication.
Read this: [remove the public keys from your fortrabbit Account](access-methods#toc-how-to-change-from-ssh-key-to-password-authentication)


### Recently imported SSH key not working

After importing a key in the Dashboard, please allow up to 7 minutes for it to be activated. If you try to access the App before the SSH key has been activated, you will be asked for a password, because the old "username + password" is still in effect.



## SSH keys under Windows with PuTTY

If you generated an SSH key with PuTTY, you will need to make sure that the private key is saved in the location where `git.exe` or `ssh.exe` are looking for it or take steps to specify where the key is. Additionally, PuTTY uses a special format to store the private key, which also requires additional steps to use with other tools.

Because of these reasons and previous experience with clients, we advise against using PuTTY. For more info, check out these questions and answers if you want to use PuTTY anyway.

* [Stack Overflow: Where to find my private RSA key?](http://serverfault.com/questions/194567/how-do-i-tell-git-for-windows-where-to-find-my-private-rsa-key)
* [Superuser: Where does Putty store known_hosts information on Windows?](http://superuser.com/questions/197489/where-does-putty-store-known-hosts-information-on-windows)

Keep in mind that PuTTY uses a custom `.ppk` format for storing keys. The PuTTY Private Key format does not work with the OpenSSH ssh client or Git. It is possible extract/convert the private key from a .ppk file with the PuttyGen utility included with PuTTY.

* [Superuser: How to convert .ppk to openssh](https://superuser.com/questions/232362/how-to-convert-ppk-key-to-openssh-key-under-linux)
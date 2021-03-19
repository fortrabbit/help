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


**Related topcs**

- [Code access methods](/access-methods) on fortrabbit
- [Deployment methods](/deployment-methods) on fortrabbit.
- [SFTP access](/sftp-uni)
- [SFTP troubleshooting](/sftp-troubleshooting) (please read this too because it covers some other common issues)
- [SSH key setup guide](ssh-keys)
- [SSH key troubleshooting](/ssh-key-troubleshooting)

If you are completely new to ssh-key authentication, please read our [SSH key setup guide](ssh-keys) and the ssh-key parts of the [SFTP access guide](/sftp-uni).

### Host authenticity warning

The first time you are connecting to fortrabbit service via SSH, there will be a warning about connection to a new ssh-server.
Just type yes, which will record the fingerprint of the fortrabbit deploy service into the ~/.ssh/known_hosts file.
If this fingerprint changes in the future, then you will se an error about that and will have to take further action depending on the situation.

```
The authenticity of host '…' can't be established
RSA key fingerprint is …
Are you sure you want to continue connecting (yes/no)?
```

In some edge cases (special environments like Git bash), you will have to [manually define where the known-hosts file is](http://stackoverflow.com/questions/9299651/git-says-warning-permanently-added-to-the-list-of-known-hosts).


### General connection errors

If you can't establish a connection at all, or if it takes "forever" to connect and nothing actually happens a few things may be the cause.

- a firewall on your end blocking outgoing connections (not very likely)
- your IP has been quarantined by our service due to many failed arrempts (very possible)

Try to figure out if you can connect to external service on port 22 from your network.
Try disabling your (corporate) VPN connection, if any.
If the issue persists, contact us via the chat-support and describe your situation.

### Use verbose mode to determine what's going on

Connect to your app with the `-v` (verbose) flag enabled. The output from this command is useful for troubleshooting issues.

    $ ssh {{app-name}}@deploy.{{region}}.frbit.com -v

For example, this is an unsuccessful attempt because none of the keys were accepted and the server is asking for a password, which will always fail becasue password authentication is disabled if you are using ssh-key authentication for your fortrabbit Account.

    $ ssh tripple-w-app@deploy.eu2.frbit.com -v
    OpenSSH_8.4p1, OpenSSL 1.1.1i  8 Dec 2020
    debug1: Reading configuration data /home/jaroslav/.ssh/config
    debug1: /home/jaroslav/.ssh/config line 6: Applying options for *
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit.usx
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit.us
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit.eu
    debug1: Reading configuration data /etc/ssh/ssh_config
    debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
    debug1: Connecting to deploy.eu2.frbit.com [52.212.159.57] port 22.
    debug1: Connection established.
    debug1: identity file /home/jaroslav/.ssh/keys/rusty-2020-jaro-generic.ed25519 type 3
    debug1: identity file /home/jaroslav/.ssh/keys/rusty-2020-jaro-generic.ed25519-cert type -1
    debug1: identity file /home/jaroslav/.ssh/id_rsa type 0
    debug1: identity file /home/jaroslav/.ssh/id_rsa-cert type -1
    debug1: identity file /home/jaroslav/.ssh/id_ed25519 type -1
    debug1: identity file /home/jaroslav/.ssh/id_ed25519-cert type -1
    debug1: Local version string SSH-2.0-OpenSSH_8.4
    debug1: Remote protocol version 2.0, remote software version OpenSSH_6.6.1p2 Ubuntu-2ubuntu2
    debug1: match: OpenSSH_6.6.1p2 Ubuntu-2ubuntu2 pat OpenSSH_6.6.1* compat 0x04000002
    debug1: Authenticating to deploy.eu2.frbit.com:22 as 'tripple-w-app'
    debug1: SSH2_MSG_KEXINIT sent
    debug1: SSH2_MSG_KEXINIT received
    debug1: kex: algorithm: curve25519-sha256@libssh.org
    debug1: kex: host key algorithm: ssh-rsa
    debug1: kex: server->client cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
    debug1: kex: client->server cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
    debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
    debug1: Server host key: ssh-rsa SHA256:Z+Vtiym9ZzdhKWgUdKM/ZLjoR6jxNB51OcRLzH0uuJ4
    debug1: Host 'deploy.eu2.frbit.com' is known and matches the RSA host key.
    debug1: Found key in /home/jaroslav/.ssh/known_hosts:37
    debug1: rekey out after 4294967296 blocks
    debug1: SSH2_MSG_NEWKEYS sent
    debug1: expecting SSH2_MSG_NEWKEYS
    debug1: SSH2_MSG_NEWKEYS received
    debug1: rekey in after 4294967296 blocks
    debug1: Will attempt key: /home/jaroslav/.ssh/keys/rusty-2020-jaro-generic.ed25519 ED25519 SHA256:nvt77cGLmGL/bfTy8o+Bj4AeEzmMQ7R+GAmwL8U/LoE explicit
    debug1: Will attempt key: /home/jaroslav/.ssh/id_rsa RSA SHA256:7dTAWY5hmXpmMegNAk2g3bmYXRaB5ZUPwIm4pWIhc7U explicit
    debug1: Will attempt key: /home/jaroslav/.ssh/id_ed25519  explicit
    debug1: SSH2_MSG_SERVICE_ACCEPT received
    debug1: Authentications that can continue: password,publickey
    debug1: Next authentication method: publickey
    debug1: Offering public key: /home/jaroslav/.ssh/keys/rusty-2020-jaro-generic.ed25519 ED25519 SHA256:nvt77cGLmGL/bfTy8o+Bj4AeEzmMQ7R+GAmwL8U/LoE explicit
    debug1: Authentications that can continue: password,publickey
    debug1: Offering public key: /home/jaroslav/.ssh/id_rsa RSA SHA256:7dTAWY5hmXpmMegNAk2g3bmYXRaB5ZUPwIm4pWIhc7U explicit
    debug1: Authentications that can continue: password,publickey
    debug1: Trying private key: /home/jaroslav/.ssh/id_ed25519
    no such identity: /home/jaroslav/.ssh/id_ed25519: No such file or directory
    debug1: Next authentication method: password
    tripple-w-app@deploy.eu2.frbit.com's password:

Successful example (using a specific key):

    $ ssh -i ~/.ssh/id_rsa_fortrabbit   tripple-w-app@deploy.eu2.frbit.com -v uptime
    OpenSSH_8.4p1, OpenSSL 1.1.1i  8 Dec 2020
    debug1: Reading configuration data /home/jaroslav/.ssh/config
    debug1: /home/jaroslav/.ssh/config line 6: Applying options for *
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit.usx
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit.us
    debug1: Reading configuration data /home/jaroslav/.ssh/config-fortrabbit.eu
    debug1: Reading configuration data /etc/ssh/ssh_config
    debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
    debug1: Connecting to deploy.eu2.frbit.com [52.212.159.57] port 22.
    debug1: Connection established.
    debug1: identity file /home/jaroslav/.ssh/id_rsa_fortrabbit type 0
    debug1: identity file /home/jaroslav/.ssh/id_rsa_fortrabbit-cert type -1
    debug1: identity file /home/jaroslav/.ssh/keys/rusty-2020-jaro-generic.ed25519 type 3
    debug1: identity file /home/jaroslav/.ssh/keys/rusty-2020-jaro-generic.ed25519-cert type -1
    debug1: identity file /home/jaroslav/.ssh/id_rsa type 0
    debug1: identity file /home/jaroslav/.ssh/id_rsa-cert type -1
    debug1: identity file /home/jaroslav/.ssh/id_ed25519 type -1
    debug1: identity file /home/jaroslav/.ssh/id_ed25519-cert type -1
    debug1: Local version string SSH-2.0-OpenSSH_8.4
    debug1: Remote protocol version 2.0, remote software version OpenSSH_6.6.1p2 Ubuntu-2ubuntu2
    debug1: match: OpenSSH_6.6.1p2 Ubuntu-2ubuntu2 pat OpenSSH_6.6.1* compat 0x04000002
    debug1: Authenticating to deploy.eu2.frbit.com:22 as 'tripple-w-app'
    debug1: SSH2_MSG_KEXINIT sent
    debug1: SSH2_MSG_KEXINIT received
    debug1: kex: algorithm: curve25519-sha256@libssh.org
    debug1: kex: host key algorithm: ssh-rsa
    debug1: kex: server->client cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
    debug1: kex: client->server cipher: aes128-ctr MAC: hmac-sha2-256 compression: none
    debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
    debug1: Server host key: ssh-rsa SHA256:Z+Vtiym9ZzdhKWgUdKM/ZLjoR6jxNB51OcRLzH0uuJ4
    debug1: Host 'deploy.eu2.frbit.com' is known and matches the RSA host key.
    debug1: Found key in /home/jaroslav/.ssh/known_hosts:37
    debug1: rekey out after 4294967296 blocks
    debug1: SSH2_MSG_NEWKEYS sent
    debug1: expecting SSH2_MSG_NEWKEYS
    debug1: SSH2_MSG_NEWKEYS received
    debug1: rekey in after 4294967296 blocks
    debug1: Will attempt key: /home/jaroslav/.ssh/id_rsa_fortrabbit RSA SHA256:QuLDaljPIExVTlsCZkyKiByU7p9RlKp8ABmJcyC5F54 explicit
    debug1: Will attempt key: /home/jaroslav/.ssh/keys/rusty-2020-jaro-generic.ed25519 ED25519 SHA256:nvt77cGLmGL/bfTy8o+Bj4AeEzmMQ7R+GAmwL8U/LoE explicit
    debug1: Will attempt key: /home/jaroslav/.ssh/id_rsa RSA SHA256:7dTAWY5hmXpmMegNAk2g3bmYXRaB5ZUPwIm4pWIhc7U explicit
    debug1: Will attempt key: /home/jaroslav/.ssh/id_ed25519  explicit
    debug1: SSH2_MSG_SERVICE_ACCEPT received
    debug1: Authentications that can continue: password,publickey
    debug1: Next authentication method: publickey
    debug1: Offering public key: /home/jaroslav/.ssh/id_rsa_fortrabbit RSA SHA256:QuLDaljPIExVTlsCZkyKiByU7p9RlKp8ABmJcyC5F54 explicit
    debug1: Server accepts key: /home/jaroslav/.ssh/id_rsa_fortrabbit RSA SHA256:QuLDaljPIExVTlsCZkyKiByU7p9RlKp8ABmJcyC5F54 explicit
    debug1: Authentication succeeded (publickey).
    Authenticated to deploy.eu2.frbit.com ([52.212.159.57]:22).
    debug1: channel 0: new [client-session]
    debug1: Requesting no-more-sessions@openssh.com
    debug1: Entering interactive session.
    debug1: pledge: network
    debug1: Sending environment.
    debug1: Sending env COLORTERM = rxvt-xpm
    debug1: Sending env LANGUAGE = en_US.UTF-8
    debug1: Sending env LANG = en_US.UTF-8
    debug1: Sending env LC_ALL = en_US.UTF-8
    debug1: Sending command: uptime
    debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
     07:42:49 up 41 days, 16:56,  0 users,  load average: 0.03, 0.08, 0.08
    debug1: channel 0: free: client-session, nchannels 1
    Transferred: sent 3864, received 1952 bytes, in 0.5 seconds
    Bytes per second: sent 7888.5, received 3985.1
    debug1: Exit status 0


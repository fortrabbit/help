---

template:      article
reviewed:      2024-06-21 16:56:02
naviTitle:     SSH access troubleshooting
title:         SSH access troubleshooting
excerpt:       What you may want to know to debug SSH connection issues
lead:          'This article aims to help you finding common issues establishing an SSH connection on fortrabbit.'
group:         ssh
order:         10
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


## Common reasons and solutions

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
# Some error here!
```

Can't login by SSH? Here are some common issues users often run into.

## SSH public key issues

If you have trouble setting up a connection, using SSH public keys as your authentication method - especially when you are doing this for the first time - see our [SSH key troubleshooting guide](ssh-key-troubleshooting) first. This is common source of issues.

## Host authenticity warning

The first time you are connecting to fortrabbit service via SSH, there will be a warning about connection to a new SSH server. Just type yes, which will record the fingerprint of the fortrabbit deploy service into the `~/.ssh/known_hosts` file. If this fingerprint changes in the future, then you will se an error about that and will have to take further action depending on the situation.

```bash
The authenticity of host '…' can't be established
RSA key fingerprint is …
Are you sure you want to continue connecting (yes/no)?
```

In some edge cases (special environments like Git bash), you will have to [manually define where the known-hosts file is](http://stackoverflow.com/questions/9299651/git-says-warning-permanently-added-to-the-list-of-known-hosts).

## General connection errors

If you can't establish a connection at all, or if it takes "forever" to connect and nothing actually happens a few things may be the cause.

* a firewall on your end blocking outgoing connections (not very likely)
* your IP has been quarantined by our service due to too many failed attempts (possible)

Try to figure out if you can connect to external services on port 22 from your network. Try disabling your (corporate) VPN connection, if any. If the issue persists, contact our support and describe your situation in detail.

## Use verbose mode to determine what's going on

Connect to your App with the `-v` (verbose) flag enabled. The output from this command is useful for troubleshooting issues.

For example, this is an unsuccessful attempt because none of the keys were accepted and the server is asking for a password, which will always fail because password authentication is disabled if you are using ssh-key authentication for your fortrabbit Account.

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com -v
```

## Internal error I01 after an App is created

If the App was recently created, please wait about 5 minutes for it to become ready. Trying to connect (git, ssh, sftp) before the App is ready produces an error like shown below.

```bash
A B O R T E D
  !! Internal error I01 - please try again later.
Connection to deploy.eu2.frbit.com closed.
```

If more than 10 minutes have passed and you still get a similar error (despite successful authentication), then please contact us via the <a href="#asd" onclick="Intercom('showNewMessage', 'I see an !! Internal error I01 for my App ______.')">support chat</a>.

## SSH key passphrases

You might be asked for a passphrase when trying to connect by SSH. Mind that a passphrase is a local security mechanism that you have set up to protect your SSH key. Under macOS the SSH key passphrase can be stored with the keychain. It's not related to fortrabbit services. See this [GitHub help page](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases) for more on passphrases.

## Check your your local SSH config

Sometimes SSH connection issues are related to configuration with your local SSH configuration. Look into the `~/.ssh` folder on your computer (Linux & macOS). In that folder there is a `config` file and a `known_hosts` file. Both may contain entries for fortrabbit servers (frbit.com), instructing your system to use a specific connection method. To debug this, remove anything related to fortrabbit. After changing the config, best restart SSH. `killall sshd` might do the trick.

## Related topics

- [Code access methods](/access-methods) on fortrabbit
- [Deployment methods](/deployment-methods) on fortrabbit
- [SFTP access](/sftp-uni)
- [SFTP troubleshooting](/sftp-troubleshooting)
- [SSH key setup guide](ssh-keys)
- [SSH key troubleshooting](/ssh-key-troubleshooting)
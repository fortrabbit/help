---

template:      article
reviewed:      2021-03-19
title:         SFTP deployment
naviTitle:     SFTP deployment
lead:          Learn about the classical way to deploy and access your App on fortrabbit.

stack:         uni
group:         deployment

keywords:
    - beginner
    - deployment
    - dreamweaver
    -

---

All [Universal Stack Apps](app-uni) support access via SFTP which is similar to the classical FTP-based workflow.

It is also possible to [deploy with Git](git-deployment). This has some advantages, but may not be suitable for every application and user. For example, most CMS application are not designed to take a Git repository into account. If a plugin is installed from the WebUI, and the application downloads new files, then those files are not automatically added to Git. It is certainly possible to deploy any PHP application via Git on fortrabbit, but you need consider scenarios like the above.


## SFTP access: username + password

1. Remove all ssh-keys from your profile, if you have any
2. Visit the Dashboard and find your username and sftp-host address
3. Configure your GUI client (not explained here)
4. Upload your files for great success

### Credentials and hostname (username+password access mode)

<!-- fixme: this is detected a s PHP snippet -->

    Host     deploy.«region».frbit.com
                    (us1 or eu2)
    Login    tripple-w-app.«randomletters»
                           (the suffux is random)
    Password Your fortrabbit Account password
    Protocol SFTP
    Port     22


Note that this password is the the same as the fortrabbit Dashboard password.
The username will have a random string after the app-name like tripple-w-app.f4n4gkrx90ot4yxm.
In this username+password access mode, the login consists of two parts {{app-name}}.{{random-letters}}.
You can find this in the Dashboard under:

- dasboard.fortrabbit.com -&gt; Apps -&gt; {{YourApp}} -&gt; Access -&gt; SFTP

### Verify username+password mode

For example, if you have an App named tripple-w-app in the eu2 region, then the following applies.

    App      tripple-w-app
    Region   eu2
    Host     deploy.eu2.frbit.com
    Login    tripple-w-app.f4n4gkrx90ot4yxm
    Password Your fortrabbit Account password

To verify that SFTP access works, open a terminal and type the following,
but use your own App name and the correct region.

    $ unset SSH_AUTH_SOCK
    $ echo hello welt > f
    $ sftp -F /dev/null tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com
    tripple-w-app.f4n4gkrx90ot4yxm@deploy.eu2.frbit.com's password:

Paste your password, output continues...  
When you get to `sftp>`, type put f, enter, exit, enter.

    Connected to deploy.eu2.frbit.com.
    sftp> put f
    Uploading f to /srv/app/tripple-w-app/htdocs/f
    f                                         100%   11     0.3KB/s   00:00
    sftp> exit

Now access the file with a http client (aka web browser)

    $ curl -D- http://tripple-w-app.frb.io/f
    HTTP/1.1 200 OK
    Date: Thu, 18 Mar 2021 09:50:21 GMT
    Server: Apache 2.x
    Last-Modified: Thu, 18 Mar 2021 09:43:32 GMT
    ETag: "b-5bdcc6ebed7b6"
    Accept-Ranges: bytes
    Content-Length: 11

    hello welt

## SFTP access: ssh-key

1. Create a key on your machine
2. Import the public key into fortrabbit Account from Dashboard
3. Profit (use ssh and git like a boss)

### Using sftp without a password (ssh-key access mode)

First create a key as described in our [ssh key setup guide](ssh-keys), you need to use an RSA key.  
Then, import the key into your fortrabbit account.  
Finally, test the connection.

To import the key, log in to your account and find the add-ssh-key-button.

- dasboard.fortrabbit.com -&gt; Your Account -&gt; [Add a new SSH key]
  - do not import the _private key_
  - copy-paste the contents of `id_rsa(_fortrabbit).pub` into the text field
  - do not paste any newline characters

The value to paste into the textfield can be read into the clipboard from a terminal.  
On windows, just use notepad to open the `id_rsa.pub` file and select all, then use control+c.  
You can also [import your GitHub keys](/access-methods#toc-github-ssh-key-import).

    xclip -i < ~/.ssh/id_rsa.pub           # linux
                                           # or...
    pbcopy < ~/.ssh/id_rsa_fortrabbit.pub  # Mac OS

The public key looks like this.

    $ cat ~/.ssh/id_rsa_fortrabbit.pub
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDQEKK60BoNOQ0Cns76npf3V2p357WrfIkG0Ih+
    fpQ40Yrdp7XdrsJ1S28g6utK50CP8Mvxm8ldgRcP4xUGCrVyu0+3xJaLHVPVO8dseg8spMrtVR7C
    G1YVekR85dHw9XkGgoPMmncrcmzO91DK6kcdmdKr3S3usXLvbmbEBxfhrp80/OYYHlKFV4fRVUyO
    GZjxF/mtI40i9VW4K0OY2bJufREeMhGYQWF+lYQ3jGNgH24oFpBC1Vq4l/sSsq68IuN/e5lyNW8X
    uBzTh1CyisdqysCzOTFpUG1o/SOeajMWJoqos6VQYIxmC5KwkJW8LbvCiwvr+T/8VVKv0z/CMsZv
    uMOhochKZQHl33AgmiHZwrkfeWfMUdBdxmGFfMj6nIWhywWijYvWnHpWq6+PdarRo1CqqTqxFRhJ
    VshQin1JdtnT2kuzoFAil8oYBpAS0z73dgt08pxTyYJ74PXY/dpKHE7Z7KaRd5SIDoSN6ibPv1Qv
    3nQN7qNTAxK02xn4tV15l5CyK/rBX+5z6oBOoyHN87xI3rRxDt598ixJk6AX0Jod5GjumJxoAB8j
    vOk+sHRerMGdPcXRVoyZFa4RXI4nnZP0UM0tuA2FAdXQQVyogezhcTdvfqVyDshrOFLRaMiUO3HN
    n2F+p1uTyFhzXEnfB5W8TKjR+WGUxp/C6Dhm4Q== user@fortrabbit


If you followed our [ssh key setup guide](ssh-keys) then your public key will be in one of these locations:

    ~/.ssh/id_rsa_fortrabbit.pub
    ~/.ssh/id_rsa.pub


### Verify ssh-key mode

Given that you have an App named  _tripple-w-app_ in EU, then the following applies

<!-- fixme: this is detected as PHP -->

    App Name  tripple-w-app (example)
    Region    eu2
    Username  tripple-w-app
    Host      deploy.eu2.frbit.com
    Password  None or whatever you used before (not fortrabbit Account password)


Try this, if the output is "app", then you are good to go.

    $ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
    app

If you see a password prompt, then you either created the key with a passphrase or the key
was very recently imported and the system thinks you are still using the username+password mode.

    $ ssh tripple-w-app@deploy.eu2.frbit.com -F /dev/null -i ~/.ssh/id_rsa_fortrabbit whoami
    tripple-w-app@deploy.eu2.frbit.com's password:

Either type in the passphrase that was used when the key was created or allow the system 40-180
seconds to activate the new key.

### Upload a file with sftp in ssh-key mode

<!-- fixme: this is detected as PHP -->

    $ echo greetings, supaninja\! > f
    $ echo put f | sftp -b - -F /dev/null -i ~/.ssh/id_rsa_fortrabbit tripple-w-app@deploy.eu2.frbit.com
    sftp> put f

Check the file via HTTP

    $ curl -D- http://tripple-w-app.frb.io/f
    HTTP/1.1 200 OK
    Date: Thu, 18 Mar 2021 11:54:43 GMT
    Server: Apache 2.x
    Last-Modified: Thu, 18 Mar 2021 11:54:19 GMT
    ETag: "16-5bdce427595ab"
    Accept-Ranges: bytes
    Content-Length: 22

    greetings, supaninja!



## Further reading

Related topics

* [ssh key setup guide](ssh-keys)

### Help! Nothing works...

Please read [ssh-key troubleshooting page](ssh-key-troubleshooting) before opening a support ticket.
These "authentication issues" are quite commonly reported, but SSH access and SFTP access is very rarely broken on our end.
Some common issues are also covered on the [SFTP troubleshooting page](/sftp-troubleshooting).

### Troubleshooting SFTP

* [ssh-key troubleshooting page](ssh-key-troubleshooting)
* [SFTP troubleshooting page](/sftp-troubleshooting)


### About SFTP

SFTP is short for SSH File Transfer Protocol. It is used for uploading and downloading files over a [SSH](/ssh-uni) connection. Despite the similar name, SFTP is very different than FTP or FTPS but for most practical porposes they seem very similar. SFTP is preferable to FTP because the the transferred data is encrypted and not visible to everyone on the network.

There are various SFTP clients out there. We recommend [Cyberduck](https://cyberduck.io/) (on Mac and Windows). Modern editors and IDEs have support for SFTP, including Sublime, PhpStorm, Notepad++ and so on. In a hurry, you can use `sftp` from a terminal which should be installed if `ssh` is installed.



### Mixing deployment methods

Please see our [deployment methods article](deployment-methods-uni) to learn how the different ways to deploy code work side by side.



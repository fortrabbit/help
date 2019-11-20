---

template:    article
reviewed:    2019-09-30
title:       How to use private Composer repos with SSH keygen
naviTitle:   Private Composer repos
lead:        Generate a unique SSH key-pair for your App on fortrabbit to use private Composer repos.
group:       platform
stack:       all

keywords:
    - Composer
    - Git
    - ssh
    - GitHub
    - Bitbucket
    - Hooks

---

Modern PHP App development utilizes [Composer](composer) as a dependency manager. There are many great open source packages [out there](http://packagist.org). But your company code is probably not intended to be released to public. That's when you use private Composer repositories — protected by SSH public key authentication.

To use your private Composer repo in [Git deployment](git-deployment) you need to set up authentication so your fortrabbit App can access your external repo (probably hosted on Bitbucket, GitHub etc). For this you need a public and private SSH key-pair. Here is how you generate it for your App:

```bash
ssh {{ssh-user}}@deploy.{{region}}.frbit.com keygen
# Generating new SSH key pair
#   Done 447ms
#
# Your SSH public key:
# ssh-rsa AAAAB3NzaC1yc2EAA...ixx47pDIa1xtMV4odTimp
```

The private key will not be displayed (you don't need it either). The public key in the example is starting at `ssh-rsa AAA...` and ending at `..odTimp`.

You can now install the key in your private Composer repository - something like BitBucket, GitHub or the one from your company. You can re-run this command at any time to view or change the current key of your App.

### Link your private repo

Now you can add your private repositories into your `composer.json` file like usual:

```
    "repositories": [
        {
            "type": "vcs",
            "url":  "git@github.com:my-company/my-package.git"
        }
    ],
    "require": {
        "my-company/my-package": "1.2.*"
    }
}
```

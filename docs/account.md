---

template:      article
reviewed:      2019-09-30
title:         Account
naviTitle:     Account
excerpt:       What you can do with your Account
lead:          An Account with fortrabbit represents YOU, it's your personal login and access here — no more no less.
group:         platform
stack:         all

keywords:
    - beginner
    - platform
    - teamwork
    - sharing
    - roles
    - access
    - security
    - mfa
    - 2fa

---

Most classical hosting works like Twitter: You have a personal account and additional accounts for your corporate tweets. fortrabbit is more like Facebook: Your Account represents you as an individual. You add company pages to represent your businesses.

```nohighlight
┌─────────────────────────────────────────────────────────┐
│                                                         │
│                         Company                         │
│                                                         │
└─────▲─────────┬─────────────┬────────────┬────────▲─────┘
      │                                             │      
    Owner      owns          owns         owns    Admin    
      │                                             │      
┌─────┴──────┐  │             │            │ ┌──────┴─────┐
│            │                               │            │
│  Account1  ├──┼───access    │    access──┼─┤  Account3  │
│            │           │         │         │            │
└─────┬──────┘  │        │    │    │       │ └──────┬─────┘
      │                  │         │                │      
    access      │        │    │    │       │     access    
      │                  │         │                │      
┌─────▼─────────▼─┐  ┌───▼────▼────▼───┐ ┌─▼────────▼─────┐
│                 │  │                 │ │                │
│      App1       │  │      App2       │ │      App3      │
│                 │  │                 │ │                │
└─────────────────┘  └─────────────────┘ └────────────────┘
```

| Account 😀           | [Company](/company) 🏢 | [App](/app)                            |
| -------------------- | ---------------------- | --------------------------------------- |
| represents a person  | represents a company   | represents a web application or website |
| is actively managing | is managed by Accounts | is managed by Accounts                  |




## Managing your Account


### Creating an Account

Creating an Account with fortrabbit is free, of course. If you haven't already: just head over to [our signup page](https://dashboard.fortrabbit.com/signup). You will be asked for your e-mail & you will choose a safe password.


### Changing the Account e-mail address

Within the [Dashboard](dashboard) > Your Account > e-mail address. Follow the procedure. A security notice will be send to your old e-mail address, then.

### Changing the Account password

Within the [Dashboard](dashboard) > Your Account > password. Here you have to enter your old password to setup a new password. Please mind that this will also change your logins (SSH/SFTP/Git) when you use password (not SSH key) as [access method](/access-methods).

### Resetting the Account password

With the [Dashboard login screen](https://dashboard.fortrabbit.com), you'll see a little link "**Forgot password?**". This will bring you to our password reset form [(direct link)](https://dashboard.fortrabbit.com/password) where you can recover your Account. In that form you will enter the e-mail address associated with your fortrabbit Account. The system will then send you a recovery link to your e-mail address. With that you can set a new password to login in again. Please mind that billing e-mail address and Account e-mail address might differ.

### Setting up 2FA

We recommend to use [two-factor-authentication](https://en.wikipedia.org/wiki/Multi-factor_authentication) with your fortrabbit Account to add an additional layer of security. You can setup and disable 2FA from your Account in the Dashboard:

* Dashboard > Your Account > [2FA](https://dashboard.fortrabbit.com/account/security/enable2fa)

During the guided setup you will enable your second device to generate OneTimePasswords for fortrabbit.

#### Using 2FA with your Account

Once 2FA is enabled with your fortrabbit Account, you'll need to enter the extra TOTP code when logging or performing critical actions requiring a SUDO. 

#### Recovering 2FA when second device is lost

During the 2FA setup the Dashboard showed you some recovery codes. These codes are always valid for your Account. Please store those in a save place you still remember later on. Use those codes to login and disable (an re-enable) 2FA in case of a lost device.

When you have also lost your 2FA recovery codes: Contact us and ask us to disable 2FA for you manually. Please provide proof that we can safely identify you, for example: invoice numbers, account e-mail address, number and name of Apps, information about code or recent conversations.

### Managing SSH keys

Also in the Dashboard under your Account, you can add your own SSH keys. Also see the [access methods article](access-methods) for more.


### Setting the SUDO mode

For many actions you are performing within the Dashboard you'll need to enter your Account password. The SUDO mode makes this a little less painless, while keeping it still secure. With the SUDO settings you can define how often you'll be asked for your Account password within a session.

### Setting the session time

With the session time setting you can define how long you like to stay logged in within the Dashboard.

### Notifications

With the notification settings you can enable/disable certain e-mails you receive from our friendly mail bot, toggle the newsletter and set to receive status messages.

### Setting a name

Please be so kind and provide your real name. This way, support is more personal and fun.

### Configuring your Avatar image

There is no setting for this. We'll show your face with your Account, when you have your e-mail registered with [Gravatar](https://en.gravatar.com/).

### Your team profile

Within the Dashboard under your Account, you can view your team profile. The team profile is a limited read only version of your Account. This is how your collaborating coworkers will see your Account.

### Setting up Companies

Your Account is also a staring point for Companies you own or are member of.

### Sharing your Account

**Please just don't!** Your Account is your personal access to fortrabbit. We have [powerful collaboration](collaboration) features.

### Deleting an Account

No longer like fortrabbit? Sorry! To cancel your Account completely:

1. Dashboard
2. Your account
3. **Cancel Account** and follow the instructions.

Please be aware that: The [billing](/billing) cycle is monthly after usage. When you cancel today, you will get one last invoice next month for the usage this month so far. For example: Today is the 7th, so the first seven days of march will be billed after the march is over.

When you have a [Company](/company) with other Owners, the Company will not be deleted, you will leave the Company. The fortrabbit Account itself is free, you can also delete all Apps and cancel all Company plans so that no future costs will applied. You'll still might get "service" e-mails by us.

---

template:      article
reviewed:      2019-09-30
title:         Collaborating on fortrabbit
naviTitle:     About Collaboration
lead:          Learn about the powerful teamwork features and how to map your real world business relationships on your favorite hosting platform — fortrabbit.
group:         collaboration
stack:         all

keywords:
    - collaboration
    - beginner
    - platform
    - teamwork
    - sharing
    - permissions
    - roles
    - organization
    - access
    - multi-client capability
    - multitenancy
    - tenant
    - client
    - employees
    - developer
    - collaborator
    - administrator
    - platform design

---


## The pitch

**Usual business scenario**: Most people are running three or more Apps on fortrabbit. And not only that: They want this App to be paid from that card, and the other App from that other card — while still being able to share access with coworkers.

**The natural reflex**: is to set up a new Account for each App and share the login details to the Dashboard across the team. The more Apps you manage that way, the more of a mess you will end up with. There is no bird's eye view, you need to logout and login again to switch to another App, manage all kind of stuff twice, send passwords by e-mail, reset passwords when someone leaves …

**What you really want**: Easily add new members to all of your team’s projects. A multi client model that maps to your real world in nearly any context: freelancer, startup, digital agency.



## Understanding the Account

When signing up with fortrabbit you'll be asked for your first and your last name. We expect that an Account represents a human being, not a Company. An Account is a person. An Account can login to the Dashboard, manage Apps and Companies. An Account can own or be part of multiple Companies. That is useful for example when you use fortrabbit privately for your week-end projects and for business. Or if you as a freelancer collaborate with various digital agencies.

You must never share your fortrabbit Account; it's "private". It's only you, with all your personal [public SSH keys](/ssh-keys) and your unique access to the [Dashboard](/dashboard).

### Personal Account access to Apps

In classical hosting you have one set of server access credentials for SFTP/SSH which you then have to share. With fortrabbit it's better: each Account has it's own personal access to each App.

```
# Personal code access with fortrabbit
┌────────────────┐                     ┌──────────────────────────┐
│                │                     │                          │
│    Account1    ├───personal access───▶                          │
│                │                     │                          │
└────────────────┘                     │           App            │
┌────────────────┐                     │                          │
│                │                     │                          │
│    Account2    ├───personal access───▶                          │
│                │                     │                          │
└────────────────┘                     └──────────────────────────┘
```


Instead of sending an e-mail with a FTP password to another developer, you are sending an invitation to create a free Account and to get instant personal access to the Apps. This is more secure, more transparent and easier to handle. Also see the [access methods article](/access-methods) to learn about the different ways (SSH key or password) to access code.


## Levels of team collaboration

```
┌────────────────────────────────────────────────────────┐    •
│                                                        │    •
│                                                        │    •
│                                                        │    •
│                        Company                         │    •
│                                                        │    •
│                                                        │    •  Level 2
│                                                        │    •  Advanced
└─────▲─────────┬─────────────┬────────────┬────────▲────┘    •  Company level
      │                                             │         •  team cooperation
    Owner       │             │            │      Admin       •  where Owners & Admins
      │                                             │         •  delegate on behalf
┌─────┴──────┐  │             │            │ ┌──────┴─────┐   •  of the Company
│            │                               │            │   •
│  Account1  ├──┼────────┐    │    ┌───────┼─┤  Account3  │   •
│            │           │         │         │            │   •
└─────┬──────┘  │        │    │    │       │ └──────┬─────┘   •
      │                  │         │                │         •
      │         │        │    │    │       │        │         •
      │                  │         │                │         •
┌─────▼─────────▼─┐  ┌───▼────▼────▼───┐ ┌─▼────────▼──────┐  •
│                 │  │                 │ │                 │  •
│      App1       │  │      App1       │ │      App1       │
│                 │  │                 │ │                 │  °
└─────────────────┘  └────────▲────────┘ └─────────────────┘  °  Level 1
                              │                               °  Simple
                              │                               °  App level
                       ┌──────┴─────┐                         °  collaboration
                       │            │                         °  where an Account
                       │  Account2  │                         °  has access to a
                       │            │                         °  single App
                       └────────────┘                         °
```

There are two levels of teamwork:

1. **[App level collaboration](/app-collaboration)**: Collaborators have access on per-App basis
2. **[Company level collaboration](company-collaboration)**: Company members act on behalf of the Company

Both levels work side by side and can be combined. Also check out our [fancy explaining video](https://help.fortrabbit.com/teamwork-video).



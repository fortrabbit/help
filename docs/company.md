---

template:      article
reviewed:      2019-06-11
title:         Companies on fortrabbit
naviTitle:     Company
excerpt:       How to work with Companies on fortrabbit
group:         platform
stack:         all

keywords:
    - beginner
    - platform
    - teamwork
    - sharing
    - roles
    - organization
    - access
    - tenant
    - client
    - employees

---


While your [fortrabbit Account](/account) represents YOU and is your personal login and path to access Apps, a Company here on fortrabbit helps you to structure and manage team and billing aspects of your Apps.

```nohighlight
┌─────────────────────────────────────┐  
│               Company               │  
│                                     │          Accounts  
│      Apps         Billing Contacts  │       ┌──────────────┐
│  ┌─────────────┐  ┌──────────────┐  │  <——  │  {{owner1}}  │
│  │ {{app-one}} │  │ {{Billing1}} │  │       ├──────────────┤
│  ├─────────────┤  ├──────────────┤  │  <——  │  {{owner2}}  │
│  │ {{app-two}} │  │ {{Billing1}} │  │       ├──────────────┤
│  ├─────────────┤  └──────────────┘  │  <——  │  {{admin2}}  │
│  │{{app-three}}│                    │       ├──────────────┤
│  └─────────────┘                    │  <——  │  {{admin1}}  │
│                                     │       └──────────────┘
└─────────────────────────────────────┘
```

The above illustration shows how Companies, Apps, Accounts and Billing Contacts relate to each other:

* Each App is owned by a Company.
* A Company can have multiple Apps.
* A Company can have multiple team members.
* A Company also can have multiple Billing Contacts to pay for the Apps.
* Each App is associated with one Billing Contact of the Company.

## Managing Companies

Don't worry when this all sounds very complex at the beginning. We believe in graceful disclosure: Companies are virtually invisible and for all intends and purposes synonym to your Account when working alone - until you need them.

### Creating a Company

You can create as many Companies as you need — free of charge. In the the Dashboard > Your Account > Companies > "Create a Company" button. Following the guided create process, you will not only create a new Company, but also it's first Billing Contact. Once created, you will be the Owner of the new Company.

<div data-markdown="1" data-user="known">

* [Create a new Company](https://dashboard.fortrabbit.com/account/company/new)

</div>

### Deleting a Company

As an Owner you can also decide to delete your Company. This is a huge step, as all Apps owned by the Company will be deleted and all the team will loose access. To do so:

* In the Dashboard (Home) >
* Companies >
* {{ Your Company }} >
* Hit the **Delete Company** button and follow the instructions

Deleting a Comoany will not cancel your Account, [see dedicated instructions for that](/account#toc-deleting-an-account). When a Company has more than one Owner, you can also just leave it:

#### Leaving a Company

With Company collaboration (see below) you can also leave a Company. The Company with stay, but you will loose access. When you cancel your Account and you are part of a Company with other Owners left, you will leave the Company.


### Collaboration

There are two levels of collaboration

1. [App level collaboration](app-collaboration): Grant code access on a per-App basis
2. **[Company level collaboration](company-collaboration)**: Grant company-wide access to all Apps and the Companies themselves

Since collaboration can easily become rather complex, we also have [collaboration overview article](collaboration), which shows you the big picture.


### Company plans

Creating a Company itself is free of any charges and it comes with a basic free Company plan by default. Company plans are booked on behalf of the Company and are available for the whole team. Higher Company plans coming are with better support levels and extended collaboration features:

* [See Company plan features](//www.fortrabbit.com/company-plans)


### Working with multiple Companies

Just like in real life, you can create or join any number of Companies with your Account. So one Account can own multiple Companies while also being involved in other Companies as Admins or even App level collaborators.


### Moving an App from one Company to another

In some cases you might want to move the App to a different Company. This will change billing AND team. Of course you first need to be at least Admin in two Companies.


### Changing the Company of an App

You can also change the ownership of Apps at any time. It starts with the App, find [more details on App ownership changing over there](/app#toc-changing-app-ownership).

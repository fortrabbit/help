---

template:      article
reviewed:      2021-02-11
title:         Company collaboration
naviTitle:     Company collaboration
lead:          Leverage Company level collaboration to map your real world structures back to fortrabbit.
group:         collaboration
stack:         all

keywords:
    - collaboration
    - administrator
    - platform design

---


## Problem

In the real world, you are working with a team. The members of this team have different functions in your company: Some are pure code developers, others take care of your accounting and others oversee your various projects.

## Solution

A [Company](/company), within the fortrabbit platform, represents your business and Company collaboration allows you to map your business setup to fortrabbit. To name a few use cases:

* Designate a responsible Admin to handle maintenance tasks, such as scaling and monitoring metrics
* Let your project managers handle their projects by themselves, by granting them rights to invite and manage App level collaborators
* Grant your accounting access to fortrabbit, so that they can download the invoices on their own


## Booking a Company plan

Company collaboration is (mostly) a paid feature, see our [fancy marketing page](https://www.fortrabbit.com/company-plans) for plans and prices. They are optional and always booked for an individual Company. Like any other resource with fortrabbit, they are billed on a precise daily settlement.

To book a Company collaboration plan: In the Dashboard > navigate to your Account > Companies > {{ Your Company }} > and there to "Company plan", this will show you a screen to book a plan. This way you can also upgrade and downgrade the Company collaboration plan.



## Access roles

A unique quality of Company collaboration are the role based permissions. Delegation is the key motivation. Only with Company collaboration members with the according roles will get automatic access to newly created Apps and members with according rights can create Apps on behalf of the Company. Each Company can have multiple Accounts associated with it:

### Owner

The Owner role can NOT be modified by someone else, multiple Owners per Company are possible. Each Company must have at least one Owner. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company
* access code of all Apps of the Company
* delete, create, change Billing Contacts for the Company
* manage all roles of the Company
* invite new Users for any role to the Company
* leave the Company (if other Owners are present)
* delete the Company


### Admin

The Admin role can be modified by Owners. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company
* access code of all Apps of the Company
* manage all Admins and Collaborators of the Company
* invite new Admins or Collaborators to the Company
* leave the Company


### App Collaborator

The App Collaborator role can be modified by Owners and Admins. It's the same as with [App collaboration](app-collaboration) and has the same permissions. When any paid Company collaboration plan is booked, all Apps, including all Universal Apps, are granted unlimited App collaboratortion.


## Inviting a Company member

* When logged in to the Dashboard, go to your Account, find "Invite a developer" or directly call: [dashboard.fortrabbit.com/users/invite](https://dashboard.fortrabbit.com/users/invite)
* If you are part of multiple Companies, you will be asked to choose the Company
* Enter name, e-mail and role
* Click "Send invitation"


## Advanced collaboration usage

Still reading? Cool go on to dive even deeper.


### Working with the same person in different Companies

If you have multiple Companies and you want to collaborate with same person on all or a subset of them: Just invite them to each Company. The same person then will have access to all Companies while logged in with their Account.



### Downgrading a Company plan

Visit the booking page as described above and select the new, smaller plan or no plan, as needed. If you want to downgrade a paid Company collaboration plan to the free Company plan, then you need first to make sure, that:

* Only one Owner exists
* No Admins exist
* No App collaborators exist


### Promoting and demoting people

Just like in real life the roles of Accounts inside a Company can change like so:

* Accounts can not downgrade or upgrade themselves to another role
* Admins can promote App collaborators to become Admins
* Owners can promote App collaborators to become Admins or Owners
* Owners can promote Admins to become Owners
* Owners can demote Admins to become App collaborators

Role changes have immediate effect, they do not require another confirmation by the affected Account.


#### Changing the role of a Company team member

If Company collaboration is active and you are either Owner or Admin, you can promote and demote other people like so: Visit the Company the Account is part of, click on the role of Account. This will open a form in which you can change the role of the Account.

### Moving a Company member to another Company

Well, that's not what you do, actually. What you'll do is: invite the same person again to the another Company. The Account will then be able to see and access Apps from both Companies under one login. The Account itself or you (as the Admin or Owner) can then revoke access to the first Company.


### When people leave

The Account who leaves a Company will loose the ability to see and edit the App in the Dashboard and also will loose all personal code access to the App. There are two directions of leaving a Company:

#### Active: An Account leaves a Company

You might want to leave the Company when the project you have collaborated on has ended or you are actually leaving the Company in real live as well. Each Account can leave a Company at any time.


##### How to actively leave a Company

In the Dashboard > "Your Account" > "Companies" > {{ Company Name }} > "Leave Company" button. There is one special case in which you can not leave a Company and that is when you are the last Owner. A Company must have at least one Owner.


#### Passive: An Owner or Admin terminates Company access for another Account

Let's say a project phase has ended, so that an App Collaborator does not need have access any more. Who can can fire whom:

* An Owner can remove Owners, Admins and App collaborators
* An Admin can remove Admins and App collaborators
* An App Collaborator can remove nobody

##### How to remove Accounts

In the Dashboard > "Your Account" > "Companies" > {{ Company Name }} > Under Collaborators > Click the role of the Account you would like to remove. This will bring up a form in which you can revoke access.


#### Confirmation on cancellations

To make the process of leaving transparent, all involved parties will get a notification mail about the action: All owners, the account who did the removal and the removed account itself.


#### A note on security when people leave

Please mind that the person who has left still might have local code copies and thereby access to sensitive data like MySQL passwords. We advice to reset service passwords. Please see the [security article](security#toc-password-reset) for more.

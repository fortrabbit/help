---

template:      article
reviewed:      2019-09-30
title:         App collaboration
naviTitle:     App collaboration
lead:          Leverage App level collaboration to easily develop code with others on fortrabbit.
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

## Problem

You want to give another developer access to your App, so that the developer can modify the code or change App configurations on your behalf. You don't want to give them your personal access details nor allow them to take actions which result in additional costs for you.

## Solution

App collaboration allows you to invite other developers to a single or multiple Apps. Those developers will be granted the following rights:

* CAN access code via Git, SSH and SFTP
* CAN see the App in the Dashboard
* CAN see the Company the App belongs to
* CAN change settings of an App in the Dashboard
* CAN NOT invite other collaborators
* CAN NOT scale an App
* CAN NOT delete an App
* CAN NOT create an App for you

## Use cases

The list is endless. Following the most common, as we see them:

* Continuously develop a single App with other developers
* Collaborate with 3rd party freelancer on a single project
* Grant temporary code access to developers, just as long as you need them


## App collaboration vs Company collaboration

fortrabbit offers [Company level collaboration](company-collaboration), which allows you to invite Accounts not only to Apps but directly to your Company. Those Accounts will then be able to invite App collaborators and manage Apps on a different level. In contrast, App collaboration is meant for working with others only on one or a few Apps, without giving them too many rights.

## Inviting an App collaborator

Easy as pie:

1. In the [Dashboard](/dashboard) overview: Click the "Invite a developer" button
2. Enter name and e-mail of your colleague
3. If you are part of multiple Companies, you will be asked to choose the Company
4. Next you will be asked, which kind Level of Collaboration you want, choose "App collaboration"
5. In the last step you can choose between Apps you want your colleague to be invited to

This will send an invitation e-mail. The recipient then can either refuse, by just ignoring the e-mail, or accept by following the sign-up link. You can start this process also from your Company or from your App.


## Removing an App collaborator

You can find the User App collaborator access form in two locations:

1. **From the Company:**  In the Dashboard: Navigate to the Company the Collaborator is part of, under App collaborators, click on the collaborators access role.
2. **From the App:** In the Dashboard: Navigate to the App the Collaborator is part of, under Team access click on the collaborators access role.

Within this form you can add or remove Apps for this Collaborator.


## Promoting an App collaborator

When you already have a Company collaboration plan you can promote a Collaborator to become Admin or Owner. In the Dashboard: Navigate to the Company the App is part of. Under App collaborators, click on the collaborators access role. This will open a form where you can change the role. You can also find the Collaborators profile page from the respective App's overview.


## Assigning more Apps to the same App collaborator

You can, of course, grant the same Account collaborating rights on App level to multiple of your Apps. In the Dashboard: Navigate to the Company the Collaborator is part of, under App collaborators, click on the collaborators access role. There you can select all Apps with enabled collaboration for this collaborator.
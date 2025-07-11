---

template:      article
reviewed:      2025-07-09 14:40:00
naviTitle:     Backups
title:         Working with fortrabbit backups
excerpt:       Download web storage and MySQL database backups
group:         platform
stack:         all

---


## Problem

Data loss is something which can happen to everybody: be it an unpatched security hole in your underlying CMS or an overworked content editor, who accidentally deletes rather important data. Whatever the cause, you should have backups to recover in case of disaster. Backups are notoriously underused, because it can be hard to set them up properly and also hard to monitor them continuously, to guarantee that they are there when needed.


## Solution

Automatic, out-of-the box, off-site, encrypted daily backups, without configuration and without hassle.


## Availability

Backups are not included with all App plans. See our [pricing page](https://www.fortrabbit.com/pricing) for more.


## Backup features

* Daily backups
* 14 days retention (30 days for Pro Apps)
* File backups (code + assets + user uploads)
* Low disk I/O > copy on write filesystem > only changes
* MySQL backups
* Zero downtime
* No performance impact on live database
* Off site backups
* End-to-end encrypted transport
* AES-256 encrypted storage
* Stored in a non-public S3 bucket
* Easy access from the Dashboard
* Unlimited backup storage size


## Accessing backups

Find them in the Dashboard > Your App > Backups. Two backup files per day are automatically generated: One of your App's web storage and one of your App's MySQL storage. The web storage backup file is a `tar` archive of all the contents of the `htdocs` folder of your App. The MySQL storage files is a plain `sql` file, containing the result of a `mysqldump` of your App's database.


### Older backups

If you need backups available for a longer time period than the retention time allows (see [specs page](https://www.fortrabbit.com/specs)), we recommend periodically downloading the backups from the Dashboard and store them locally.


## Recovering

As mentioned above, there are two types of backups: MySQL and web storage. Here is how you recover from either:


### Recover MySQL

The MySQL backup file is a regular MySQL dump, generated by the `mysqldump` tool. You can recover from it the same way you would (re-)import MySQL data into your database. Please read on [in the MySQL import/export guide](mysql#toc-export-amp-import).


### Recover web storage (Uni Apps)

Web storage backups are stored in an uncompressed `.tar` archive file. How to recover your web storage data, depends on what you want to recover: Just a single file, a couple of files or everything?

**Recover a single file or a couple of files**: Just download the web storage backup file, unpack it locally, copy the files you want to recover manually back to your App.

**Recover everything**: After downloading and unpacking the web storage `.tar` archive locally, you need to remove all files from your current App, then upload the previously unpacked archive again.


### No file backups with Pro Apps

Unlike with Universal Apps — there are no file backups with Pro Apps. The implicit reason is that the file system contents of Pro Apps are ephemeral and deployments atomic. So the contents of the file system can always be regenerated with the contents of the Git repository. So file backups for Pro Apps are not required. The Git repo is the file backup.


## Environment variables and App secrets

Please mind that [environment variables](/env-vars) and [App secrets](/secrets) will not be backed up.


## Backups from deleted Apps

Associated backups will also get removed when an App is deleted. There is a short delay of 5 days in which backups from already deleted Apps — only if the previous App plan contained backups - can be supplied on request by fortrabbit.


## Backup excludes

Some run-time and cache files, like a `storage` folder, are excluded from the file backups to save some space within the backups. For the same reason `*.mysql` files are excluded - we already provide MySQL backups without affecting performance while creating. Whole exclude list:


```
# Craft 2
craft/storage/backups
craft/storage/logs
craft/storage/runtime

# Craft 3 and 4
storage/backups
storage/logs
storage/runtime
web/cpresources

# Laravel
storage/framework
storage/logs

# Misc
.git
.idea
*.sql
```

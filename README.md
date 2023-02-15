# fortrabbit help pages

Welcome to the source of the official fortrabbit documentation. These files here are written in Markdown, extended with frontmatter meta-data. Contents are getting published on: [help.fortrabbit.com](https://help.fortrabbit.com)

## Contributing

Found a typo or an error? Do you want to add something about your framework or service of choice? Do you run a 3rd party service or an open source project that can be integrated with fortrabbit? You are more than welcome to contribute.

### Deploying

This repo only contains the contents of the help. Rendering the contents is done by a PHP engine App running on the fortrabbit platform itself. To deploy latest changes from this repo (master branch), the fortrabbit App needs to be deployed (by fortrabbit staff).

### Sending pull requests

Please find a good balance in the number of commits contained with a pull request. For small typos, just use one commit and one PR. For larger text changes, consider combining commits for readability.

## Front Matter syntax

Each markdown file has a yaml block at the top.

### Available attributes

```yaml
# Rendering

# which template to use - "article", if in doubt — REQUIRED
template: article

# Headline & display

# title shown in navigation -> short but descriptive — REQUIRED
naviTitle: About Apps

# title shown in article header on display
title: What is an App anyways?

# lead text for article detail view -> what to expect from content
lead: Forget servers. Think services instead. Learn the basic fortrabbit concepts.

# alternative to lead text. when lead is intro & excerpt is overview or lead is not available
excerpt: Get to know about the basic concepts.

# disable TOC generation for this article
noToc: true

# Set support level
supportLevel: a
supportLevel: b
supportLevel: c


# Stack meta

# where to show this: uni, pro old, all — REQUIRED when required
stack:     uni

# link to the Universal App version
uniLink:   mysql-uni

# link to the professional version
proLink:   mysql-pro


# General meta data

# When the article was last checked?
# Don't forget to edit this each time you have reviewed a page
# pages older than one year will be marked outdated
reviewed: 2023-02-15 08:21:37

# set a warning that this article is outdated, no matter the date
deprecated: 1

# show that this is work in progress
workInProgress: yes

# Set under which headline the content will be shown on home
# (use underscore instead of space)
group: Extending_fortrabbit

# Order to show in lists in groups
# higher number is later down the list
order: 7

# don't show in list on homepage
dontList: true

# do not include in search, don't show in search results
dontIndex: true

# additional keywords for document search to help users find this article
keywords:
    - foo
    - bar

# Extended meta data

# The actual link you will send people to (for external services)
websiteLink: https://www.wordpress.org?utm_source=fortrabbit

# when there is another version available
otherVersions:
    3 : install-craft-2-uni


# A human readable version of that same link, exclude clutter!
websiteLinkText: www.wordpress.org

# set for latency relevant 3rd party services, otherwise use nor acceptable
dataCenters: n/a

# logo shown for 3rd party service or open source
image: myfunkyimage.png

# type, for CMS/framework
type:  CMS

# Search result ranking with Algolia
rank: 100

```

## Horizontal rulers

* `---` don't use this, otherwise the whole help page won't be rendered
* `- - -` use this

## Dynamic help

Those values will be replaced by JS when users are logged in:

```raw
SSH user: {{ssh-user}}
Region:   {{region}}
Your app: {{app-name}}
```

It will dynamically show the correct code examples and Dashboard links.

## Dashboard links

You can have certain parts in Markdown available only for Users who are logged in like so:

```html
<div markdown="1" data-user="known">

[Set up a new domain for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/new/name)

</div>
```

This parses markdown inside the DIV. With the data-user attribute it checks if the user is logged in. links to the Dashboard will be styled as buttons — use a verb to start them!

## Writing conventions

### Support driven documentation

These help pages are often used to point clients to answers on common questions. That's why some topics are covered im much detail. Clients are asking for it. So instead of writing an answer in client support, one might just update the help pages and point the client to the new section. Headlines have anchors for deep-linking like [help.fortrabbit.com/craft-3-assets-uni#toc-options-to-sync-assets](https://help.fortrabbit.com/craft-3-assets-uni#toc-options-to-sync-assets).

### Know the target audience

While some implicit technical details are obvious to you, they might not be for the client. Explaining one acronym only with another acronym might not help. Sometimes it needs some more words.

### Code examples

* Open and close code blocks with ````shell` where shell is the language for syntax highlighting
* Try to keep code examples together in one block, avoid mixing paragraphs and code blocks
* Code blocks follow standard markdown formatting
* Show output only when necessary
* Output should be a comment `#`
* Use comments in between commands to explain what's going on
* `$` to start a command
* Start code examples right away: PHP without `<?php`, Bash without `#!/bin/bash`

### Writing

* Use sentence case in headlines
* Don't use too many headlines to structure text
* Available headlines in text: h2, h3, h4, don't go deeper
* Avoid "eg", otherwise choose one style to write it
* Keep lists readable (no sublists, etc)
* Text structure is important, but readability is even more important
* Use ASCII art to illustrate topographies and such things
* Don't use italic > it looks ugly, it's rendered by the browser not the font
* Don't use a paragraph for every sentence
* Write speaking links, not like so: `[here](http://somewhere)` but so: `[GitHub project](http…)`
* Don't bullshit. Avoid the use of marketing buzzwords

### Maintainability

* Find the right balance between being general and being precise (aka Captain Obvious)
* Very detailed step-by-step articles are easy to follow but get outdated very quickly
* Don't use screenshots and images, use words and ASCII graphics
* Don't bury numbers (like prices and limits) in articles
* All those numbers must be managed in the "pricing" and the "specs" page
* Keep it DRY! Don't repeat yourself
* Have one SSOT, link to it
* Don't fully cover the same topic on different places, just link to the location

### File name conventions

When creating new files:

* use dashes instead of spaces or low dashes for file names
* use the short versions: `-uni`, `-pro` at the end for different stacks

### fortrabbit owned words and common casing

* **Account** — ~~profile~~, ~~account~~
* **Activities** ~~history~~, ~~diary~~, ~~log~~
* **Admin** ~~admin~~
* **App Secrets** — ~~Secrets~~
* **App URL**
* **App** — ~~app~~, ~~website~~, ~~application~~
* **Billing Contact** ~~bc~~
* **Company** — ~~company~~
* **Cron Job** — ~~cron~~, ~~cron job~~
* **Dashboard** — ~~control panel~~, ~~console~~
* **Developer** ~~developer~~
* **fortrabbit scheduler** — ~~scheduler~~
* **fortrabbit**
* **fortrabbit.yml** — ~~deployment file~~
* **Help** — ~~documentation~~, ~~handbook~~, ~~docs~~
* **HTTP** — ~~http~~
* **HTTPS** — ~~https~~
* **Owner** ~~owner~~
* **root path** ~~document root~~, ~~doc root~~
* **terminal** ~~Terminal~~, ~~shell~~, ~~bash~~
* **Tinkering level** ~~Development plans~~ (single Node App plans on Professional stack)
* **URL** — ~~Url~~, ~~url~~
* **User** — ~~member~~, ~~account~~
* **Workers** — ~~Worker~~
* **Software Preset** — ~~stack chooser~~
* **Professional Stack** — ~~Pro Stack~~, ~~Professional stack~~
* **Universal Stack** — ~~Hobby stack~~, ~~hobby stack~~
* **{{ interchangeable-value }}** < something the users will need to modify
* **{{ your-app }}** ~~{{ my-app }}~~, ~~{{ app-name }}~~

### Collaboration words

* **Collaboration** = Company collaboration + App collaboration
* **Company collaboration** with Company plans, Admins, Owners & App Collaborators
* **Company plans** — the plans one can book
* **App collaboration** with App Collaborators
* **App Collaborator** = Access to Apps
* **Team members** = Company members + App Collaborators
* **Company member** — ~~Company collaborator~~
* **Admin** = the Role
* **Owner** = the Role
* **App Collaborator**
* **Role** — ~~permission~~, ~~access role~~

### Times & dates

* use the same time-zone E V E R Y W H E R E when communicating times
* the communicated time-zone is UTC
* use relative times (an hour ago, a few seconds ago)
* Markup example: `<time datetime="2016-01-01 08:22:10 UTC" title="2018-01-01 08:22:10 UTC">A few minutes ago</time>`
* **ms** — ~~MS~~
* **s** - ~~scds~~
* **m** - ~~mins~~, ~~min~~
* **h** - ~~hrs~~

### Metric units

* **16 MB** — ~~mb~~, capital letters, a space between value and unit
* **3 GB** ~~3078 MB~~ < prefer a readable unit
* **0 B**
* **16 KB**
* 10k — ~~10,000~~
* times a value: ×, `&times`; ~~x~~

### Other words

* €23 < without space char
* $23 < without space char
* **add-on** — add-ons, ~~Add-on~~, ~~Add-On~~, ~~AddOn~~
* **client** — ~~customer~~, ~~user~~
* **Component** ~~plan~~
* **default domain**
* **e-mail** — ~~eMail~~, ~~email~~, ~~Email~~, ~~E-Mail~~
* **free trial** — ~~test flight~~, ~~test drive~~
* **host** — ~~server~~
* a space char belongs before the unit
* **MySQL** — ~~MySql~~
* **OPcache** — ~~OPCache~~ ~~Opcache~~
* **node** — ~~server~~
* **non-persistent storage** — ~~storageless~~
* **payment details** — ~~complete account~~, ~~payment credentials~~
* **PHP requests** — ~~requests~~
* **PHP response time** — ~~response time~~
* **PHP** — ~~Php~~
* **Scale** — ~~upgrade~~
* **session time**
* **SFTP**
* **SSH Key** — ~~pubkey~~, ~~public SSH key~~
* **SSH**
* **SSL cert** — ~~SSL certificate~~
* **web storage** — ~~webspace~~
* **Composer** — ~~composer~~
* **macOS** — ~~Mac OS X~~

## Avoid words list

Please do not use the following "bullshit" words:

* great
* awesome
* innovative
* game-changing
* empowering
* revolutionizing

### Gender-neutral language

When referencing a hypothetical person, such as "a user with a session cookie", use gender-neutral pronouns (they/their/them). For example, instead of:

* he or she, use they
* him or her, use them
* his or her, use their
* his or hers, use theirs
* himself or herself, use themselves

### Corporate identity

There is no fortrabbit logo as graphic file. To create the brand logo just type: "• fortrabbit" — bullet character, a normal space and then name of the company with a f. Use the [Georgia Typeface](http://en.wikipedia.org/wiki/Georgia_(typeface)) in bold and italic. Use lot's of whitespace around the logo, don't put other text nearby the logo. When using the company name within a paragraph of text, write "fortrabbit" with a f, even at the beginning of a sentence. Don't use the bullet or any other typeface here.

### Logos and brand assets

Place logos for external services and projects in the media folder. They ideally should be svg or either PNG with transparent background. Favor an "image mark" over a "word image mark".

---

template:   article
reviewed:   2019-10-30
title:      Using environment variables in PHP and on fortrabbit
naviTitle:  Environment variables
lead:       ENV vars help to create and shape the environment of where the code runs. It's a good modern practice.
group:      platform
stack:      all

keywords:
    - ENV vars
    - Environment variables
    - php
    - beginner
    - phpdotenv
    - dotenv

---

## Problem

You most likely run at least two environments of your App: A [local one for development](/local-development) and one here on fortrabbit for production. Both instances probably have access to a database. Your local MySQL has of course different credentials than the remote one. Your config file, storing this information, is under Git version control. So how to deal with different environment-specific configurations? Also, how to work in a team when everyone has it's own local settings? How to separate code from configuration, so that the code is portable?

## Solution

Everything specific to the environment should be stored in Environment variables or short "ENV vars".

### About ENV vars

An ENV var is a key value pair, like so: `MY_SQL_PASS:sCRAmblEDegGGs`. These variables are, as you hopefully can guess by now, specifically stored per environment. ENV vars are potentially stored in multiple places. This is how you can define an ENV var with an Apache configuration:

```
<VirtualHost hostname:80>
   SetEnv MY_SQL_PASS sCRAmblEDegGGs
</VirtualHost>
```

This is a basic example on how you can do that locally. In most cases you will not need to touch that level here. Please read on for further usage and how to do it on fortrabbit.

## ENV vars in modern PHP

You can access ENV vars from PHP. And it is a commonly wide spread best practice to do that.


### The .env file

Dealing with server settings is not convenient for many developers. So the Ruby community figured out the `.env` file format. This a plain configuration file with `KEY=VALUE` pairs, easy to write by humans and runtime agnostic.
All you need is a parser library, that reads the file and makes the vars accessible from your code base. 

### PHP dotenv

The `.env` file concept has become quite common and there are ports to all languages. Here are the most popular ones for the PHP:

* [github.com/vlucas/phpdotenv](https://github.com/vlucas/phpdotenv)
* [github.com/symfony/dotenv](https://github.com/symfony/dotenv)

Modern PHP frameworks — [Laravel](/install-laravel), [Symfony](/install-symfony) …) and CMS ([Craft](/craft-3-about)) use one or the other under the hood.

### ENV vars on fortrabbit

Recap: ENV vars are environment specific. So, in consequence, the `.env` file will NOT be deployed to your fortrabbit App. It's usually excluded from tracking in Git. 

Now, how should your fortrabbit App know about it's ENV vars? Some users think they need to set up an additional `.env` file on the fortrabbit App. That's not the way it works. fortrabbit Apps have their ENV vars set directly with the server. Those can be set in the Dashboard with the App. 

Recap: The PHP application itself will just query the ENV vars. A library just helps to populate the ENV vars into the code base, but only when they are not set already. 

The fortrabbit [Software Preset](/app#software-preset) is where the magic happens. While creating an App on fortrabbit, you'll choose your desired CMS or framework. This selection will configure the server ENV vars in ways, the software can work with it. For example, for Laravel and Craft, the ENV var `DB_PASSWORD` will be populated with the password of the Apps database. For Symfony we provide a ready to use DSN in the `DATABASE_URL` variable. Here is the link to the settings of your App:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

So, most likely, your fortrabbit App will work out of the box. As a bonus you even reset the database password without touching any configurations.

### Adding and editing ENV vars on fortrabbit

You can add ENV vars of your App in the [Dashboard](dashboard) > Your App > Settings > ENV Vars. The input supports the dotenv file format and allows you to create or update multiple variables at once.

<div markdown="1" data-user="known">
[Add ENV vars to your App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

The changes will be distributed after you save the page. It may take around 60 seconds, a re-deploy is not necessary. Some frameworks and CMS might cache the ENV vars, like Laravel, see [here](https://laravel.com/docs/5.6/configuration#configuration-caching).

### Accessing ENV vars from raw PHP

You can access your ENV vars from PHP either using the global variable `$_SERVER` or the function `getenv()`:

```php
echo $_SERVER["MY_ENV_VAR"];
# or
echo getenv("MY_ENV_VAR");
```

## Advanced topics

So far we have covered the basics read on to learn to dive deeper into ENV vars and how they can help you.

<!--
Environment detection usually the opposite

### Environment detection

ENV vars, by definition, also can play a major role when it comes to detecting which environment the application currently runs in. Beside different access credentials, you may want to set additional parameters, when running locally, please see our [local development](local-development#toc-environment-detection) article for more.
-->

### ENV var types on fortrabbit

There are four different kinds of ENV vars here on fortrabbit which are available to your App at runtime.

#### Custom ENV vars

Those are the ones you add yourself in the Dashboard.

#### App ENV vars

Generic ENV vars cannot be overwritten by you. They are always available.

* `APP_NAME` contains the name of your App
* `APP_SECRETS` contains the path to a JSON encoded file containing [App Secrets](secrets)


#### Software Preset ENV vars

Depending on what you have selected in the [Software Preset](/app#software-preset) when creating your App, additional ENV vars will be seeded for you. For example: When choosing Laravel the ENV var `APP_KEY` with a random, 32 char long string will created (among others). You can replace or remove those Stack ENV vars after App creation the same way you can replace or remove your manually created ENV vars.


#### Dynamic ENV vars

Dynamic ENV vars become available if you have enabled *"Dynamic ENV vars"* in the Dashboard, which is the default for new Apps. They contain access details for services offered by fortrabbit. You can find them in the Dashboard > {{app-name}} > ENV vars on the right hand side. They will never overwrite existing, manually created ENV vars. This means: if you manually create an ENV var, we guarantee that we won't replace it's value by a dynamically generated ENV var.

Following a list of available dynamic ENV vars:

**MySQL** (All Universal Apps or Professional App Component)

* `MYSQL_DATABASE`: Contains the App's database name
* `MYSQL_HOST`: Contains the App's database hostname
* `MYSQL_PASSWORD`: Contains the App's database password
* `MYSQL_PORT`: Contains the App's database port
* `MYSQL_USER`: Contains the App's database user

**Object Storage** (Professional App Component)

* `OBJECT_STORAGE_BUCKET`: Contains the App's bucket name
* `OBJECT_STORAGE_KEY`: Contains the App's bucket access key
* `OBJECT_STORAGE_REGION`: Contains the App's bucket region
* `OBJECT_STORAGE_SECRET`: Contains the App's bucket access secret
* `OBJECT_STORAGE_SERVER`: Contains the App's Object Storage API server

**Memcache** (Professional App Component)

* `MEMCACHE_COUNT`: Contains amount of available memcache hosts (Production plan: 2, otherwise: 1)
* `MEMCACHE_HOST1`: Contains the first Memcache node hostname
* `MEMCACHE_PORT1`: Contains the first Memcache node port
* `MEMCACHE_HOST2`: Contains the second Memcache node hostname (if using a Production plan)
* `MEMCACHE_PORT2`: Contains the second Memcache node port (if using a Production plan)

#### Nested ENV vars

You can use simple, [nested variables](https://github.com/vlucas/phpdotenv#nesting-variables) in your custom ENV vars. Simple means, that you can set variables which reference other variables, which contain a value, for example:

```plain
# will work:
MY_VAR=${OTHER_VAR}
OTHER_VAR=something
```

We do not support multiple levels of interpolation, which means that you cannot use variables, which reference other variables, which again reference other variables, for example:

```plain
# will not work:
MY_VAR=${OTHER_VAR}
OTHER_VAR=${ANOTHER_VAR}
ANOTHER_VAR=something
```

### ENV vars vs security

Storing credentials (passwords, secrets, ..) in environment variables is not without risk. They can be exposed, due to programming errors or oversights, for example when you forget to remove the `phpinfo()` from production. Please read an [in-depth discussion in our Blog](how-to-keep-a-secret). We offer a convenient solution for this problem with our [App secrets](secrets).




### ENV var validation

Strict validation rules for ENV vars are in use in the Dashboard while entering. Chars like the "$" sign can be harmful in Linux systems. Here is the regex we use to validate the ENV var input in the Dashboard:

```plain
/^[\p{L}\p{N}\ _\-\+=\.,:;\?!@~%&\*\(\)\[\]\{\}<>\/\\#]+$/u
```

So sometimes, when you want to store an external API key as an ENV var, you might get a validation error like: "Variable value contains invalid characters". If you can not change the value of your ENV var, you can encode and decode those:


### Encode and decode ENV vars

Use a base64 encoded string and decode it when applying it to your configuration in your code. Encoding works like this:

```php
php -r "echo base64_encode('YOUR-M$Pa#A-VALUEx') . PHP_EOL;"
```

And [this example](https://github.com/laravel/ideas/issues/416#issuecomment-280436034) should give you an idea how you can do the decoding.



## Reserved environment variable names

```
There are some names you can not use here:

- APP_NAME
- DOCUMENT_ROOT
- FCGI_ROLE
- GATEWAY_INTERFACE
- GROUP
- HOME
- PATH
- PS1
- QUERY_STRING
- SESSION
- USER

Also names cannot begin with:

- HTTP_
- PHP_
- REDIRECT_
- REMOTE_
- REQUEST_
- SCRIPT_
- SERVER_
- LC_
```

<!--

outdated

### ENV var encryption

EXPERIMENTAL IDEA:  If you prefer not to use [App secrets](secrets), but want to secure your ENV vars we recommend to store them encrypted and encoded in the Dashboard. You then decode and decrypt them later on in your App. Following a generic example:

Create a secret key and use the `mcrypt_encrypt()` (PHP 5.6 only) function to encrypt and then base64 encode your variables. An exemplary PHP script doing that can be [downloaded from this Gist](https://gist.github.com/ukautz/3573878af39e81c009fa) and then executed locally.

Start with generating a new key and storing this key somewhere in your app's bootstrap code:

```bash
php encrypt.php genkey
# QEbfdTYNH23YddNUa1srixRAwBVs2L5p
```

Then you can encrypt the values of each environment variable, you want to store safely:

```bash
php encrypt.php enc "The Key" "Some Value"
# ENC:YToyOntp...t8CI7fQ==
php encrypt.php enc "The Key" "Some Other Value"
# ENC:XioptiwF...tmMNs9p==
```

To access the decrypted values you simply do the reverse: decode from base64 and decrypt using `mcrypt_decrypt()`. Again, here is an [exemplary Gist](https://gist.github.com/ukautz/0b430aafc7cc996fc946). Using it, you can then decode the environment variables in your app's bootrap:

```php
// ..
bootstrapSecEnv("Your Key");
// ..
```

Later, when you need to access your now decrypted environment variables:

```php
// ..
$dbUser = secEnv("DB_USER");
$dbPass = secEnv("DB_PASS");
// ..
```
-->

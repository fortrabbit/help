---

template:      article
reviewed:      2025-09-15 08:20:42
title:         Our security recommendations
naviTitle:     Security tips
lead:          "Essential tips to keep your fortrabbit Account and your code base secure."
group:         tips
stack:         all

---

## Dashboard and passwords security

* The password to login with the fortrabbit Dashboard must be secure.
  * Use a password manager or a long pass-phrase.
  * Don't share your Account password, use our collaboration features instead.
* Enable two-factor authentication (2FA) with your fortrabbit Account.
* Use SSH key authentication to access code instead of username+password
* Rotate internal passwords via the Dashboard for your App:
  * MySQL database
  * Object storage
  * When company members leave
* Go over the list of imported SSH public keys periodically and keep only those being used.

## PHP code security

* Make sure to follow common security guidelines - see [PHP the right way](http://www.phptherightway.com/#security).
* Mind the [OWASP Cheat Sheets](https://www.owasp.org/index.php/OWASP_Cheat_Sheet_Series) to negate attacks before they can start.
* Best practice for security and portability is to store secrets like database password not with code but with our App Secrets or ENV vars (as long as they are not exposed as well).
* Don't store sensitive information in plain text in the database, use ciphertext.

---

template:      article
reviewed:      2021-08-06
title:         Our security recommendations
naviTitle:     Security design
lead:          
group:         tips
stack:         all

---

## Code

* Make sure to follow common security guidelines - see [PHP the right way](http://www.phptherightway.com/#security). 
* It's good practice to perform a security check against the most common attack vectors before going live. 
* Mind the [OWASP Cheat Sheets](https://www.owasp.org/index.php/OWASP_Cheat_Sheet_Series) to negate attacks before they can start.
* Best practice for security and portability is to store secrets like database password not with code but with our App Secrets or ENV vars (as long as they are not exposed as well).
* Don't store any sensible information in plain text in the database, use ciphertext.

## Passwords

* The password to login with the fortrabbit Dashboard is a master password. Use a password manager or a pass-phrase. Don't share your Account password, use collaboration features instead.
* Enable two-factor authentication (2FA) with your fortrabbit Account.
* Use SSH key authentication to access code.
* Rotate passwords: reset the fortrabbit service passwords for MySQL and Object Storage in the Dashboard with your Apps. It is recommended to reset those passwords periodically and when a Company member leaves for each App. Also revisit your list of SSH keys from time to time and keep it as short as possible. Only keep those keys you are really using.
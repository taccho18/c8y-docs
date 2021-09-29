---
weight: 10
title: Configuring login settings
layout: redirect
---

Click **Authentication** in the **Settings** menu to view or configure the login settings.

![Login settings](/images/users-guide/Administration/admin-settings-authentication.png)

>>**Required role:** Tenant management ADMIN or Tenant management CREATE

You can choose one of the following login modes:

* [OAuth Internal](#oauth-internal) - Recommended, since it provides high security, using authorization tokens to prove your identity (to the server).
* Basic Auth - Should be chosen only for specific compatibility reasons, since it only provides basic security.
* [Single sign-on](#single-sign-on) redirect - Can be selected only if SSO is configured. If selected, will remove Basic Auth and OAuth Internal login options.

This login mode will be used by the platform's applications as the default method to authenticate users. Device authentication stays unchanged.

>**Info:** If OAuth Internal is enforced, Basic Auth cannot be used to login to applications anymore. Older applications might fail to display the login correctly and need to be updated.

In the field **Limit password validity for**, you can limit the maximum validity of user passwords by specifying the number of days after which users have to change their passwords. If you do not want to force your users to change passwords, use "0" for unlimited validity of passwords (default value).

>**Info:** The password validity limit is not imposed on users with a "devices" role. This prevents devices passwords from expiring.

By default, users can use any password with eight characters or more. If you select **Enforce that all password are strong (green)**, your users must provide strong passwords as described in [Getting Started > Accessing and logging into the {{< product-c8y-iot >}} platform](/users-guide/getting-started/#login).

>**Info:** The password validity limit and the enforcing of strong passwords may not be editable, if configured by the platform administrator.

 <!--Moreover, the system restricts the use of passwords already used in the past. The number of the password history is set to 10 per default but can be configured by the platform administrator as well as the the number of characters required for strong passwords and the password validity. For details on platform configuration, see the *Cumulocity IoT Core - Operations guide*.-->

Click **Save** to apply the settings.

>**Important:** Each time you change the login mode you will be forced to log out. Other users will need to log out and log in again so that the change is applied.

Select the checkbox **Allow two-factor authentication** if you want to allow TFA in your tenant, see [Two-Factor Authentication](#tfa) for details.

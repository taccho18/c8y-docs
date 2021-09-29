---
weight: 10
title: Application settings
layout: redirect
---

Click **Application** in the **Settings** menu to change applications settings.

![Default application](/images/users-guide/Administration/admin-settings-application.png)

Under **Default application**, you can select a default application from the list which will apply to all users within the tenant.

>**Info:** All users must have access to this application.

Under **Access control**, administrators can enable cross-origin resource sharing or "CORS" on the {{< product-c8y-iot >}} API.

The **Allowed Domain** setting will enable your JavaScript web applications to directly communicate with REST APIs.

* Set it to "*" to allow communication from any host.
* Set it to `http://my.host.com`, `http://myother.host.com` to allow applications from `http://my.host.com` and from `http://myother.host.com` to communicate with the platform.

For further information, see [http://enable-cors.org](http://enable-cors.org).

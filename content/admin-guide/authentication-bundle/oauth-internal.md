---
weight: 20
title: OAuth Internal configuration
layout: redirect
---

{{< product-c8y-iot >}} OAuth Internal is based on JWT stored in a browser cookie. However, it doesn't support refresh and after the token validity time has ended, the user will have to log in again.

The lifespan for both, token and cookie, is configurable by tenant options belonging to the category `oauth.internal`.

#### Token settings
The default token validity time is two weeks and this can be changed with tenant options:
 - category: `oauth.internal`;
 - key: `basic-token.lifespan.seconds`;

The minimum allowed value is 5 minutes.

#### Cookies settings
Cookies used to store a token in a browser have their own validity time that can be changed with tenant options:
- category: `oauth.internal`;
- key: `basic-user.cookie.lifespan.seconds`;

The default value is two weeks. It can also be set to any negative value so that the cookie will be deleted when the user closes the browser.

Refer to the [Tenant API](https://{{< domain-c8y >}}/api/#tag/Tenant-API) in the {{< openapi >}} for more details.

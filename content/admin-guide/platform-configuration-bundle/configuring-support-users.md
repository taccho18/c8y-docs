---
weight: 20
title: To configure a support user
layout: redirect
---

A support user is a user in the {{< management-tenant >}} with specific permissions. This user can log in to the target tenant and impersonate the target user.

To configure a user in the {{< management-tenant >}} as support user, you need to assign the relevant roles to the user. This can either be done by using a global role or by inventory roles.  

**Using a global role**

1. Create a role "Support" with "Support READ" and "Support ADMIN" permission.
2. Assign the role "Support" to the respective user and remove all other roles for the user.

**Using inventory roles**

Using inventory roles, you can selectively assign a support user for specific subtenants.

1. Create an inventory role called "Support" with type = "*" and permission = "All".
2. Create a group of all subtenants which you want to be supported by the user.
3. Assign the "Support" inventory role to above group as described in [Administration > Managing permissions > Assigning inventory roles to users](/users-guide/administration#attach-inventory).

> **Info:** The support user feature does not work when the support user has two-factor authentication enabled, but no phone number is provided. The phone number has to be provided first, in order to login as a support user.

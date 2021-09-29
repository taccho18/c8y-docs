---
title: Getting started
weight: 10
layout: bundle

---

The following sections will walk you through all functionalities of the Administration application in detail. For your convenience find an overview on the content of this document below.

|SECTION|CONTENT|
|:---|:---|
|[Home Screen](#home-screen)|Providing information on your [capacity usage and subscribed applications](#home-screen).
|[Managing Users](#managing-users)|How to [create users](#creating-users), edit, disable or delete them.
|[Managing Permissions](#managing-permissions)|How to add and edit [global roles](#global) and [inventory roles](#inventory), how to assign them to users, and how to [grant application access](#app-access).
|[Managing applications](#managing-applications)|How to manage subscribed applications and how to [manage and configure own applications](#managing-applications) in your {{< product-c8y-iot >}} account.
|[Managing business rules](#business-rules)|How to set up real-time [event processing](#event-processing) scripts and reprioritize alarms by [alarm mappings](#reprio-alarms).
|[Managing data retention](#data-retention)|How to manage and configure [retention rules](#retention-rules) for your data and how to [manage stored files](#files) in the file repository.
|[Two-factor authentication](#tfa)|How to activate/deactivate [two-factor authentication](#tfa) for a user.
|[Changing settings](#changing-settings)|How to change account settings like [application settings](#default-app) or [authentication settings](#authentication), how to manage the [properties library](#properties) and how to configure [single sign-on](#single-sign-on).



The Home screen of the Administration application provides

* a welcome message,
* quick links to the main parts of the Administration application,
* your capacity usage for the current and for the last month,
* the optional applications you are subscribed to.

<img src="/images/users-guide/Administration/admin-home.png" alt="Home screen">

The capacity sections show:

* API requests: The total number of API requests, counting whenever some function in {{< product-c8y-iot >}} is invoked, regardless of whether the function is invoked from a device (for example, sending a measurement) or from an application (for example, viewing the list of devices).
* Device API requests: Counting only when the API is called from a device (for example, sending a measurement).
* Storage: The total amount of data stored in your account. This amount can be changed by [retention policies](/users-guide/administration/#retention-rules) and by the amount and size of [stored files](/users-guide/administration#files).
* Storage quota: If the storage limit per device is set, the user is restricted to a [maximum data usage](/users-guide/enterprise-tenant/#storage-quota).
* Root devices: The number of root devices connected to your account, excluding child devices.
* Devices: The total number of devices connected to your account. This is the sum of the devices listed in the [All devices](/users-guide/device-management#viewing-devices) page of the Device Management application and their direct and indirect child devices.
* Users: The sum of all users configured in this account, active and inactive.

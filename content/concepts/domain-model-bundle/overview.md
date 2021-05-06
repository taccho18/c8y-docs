---
weight: 10
title: Overview
layout: redirect
---


The following image shows the relevant aspects of devices and assets in the Internet of Things:

![model](/images/concepts-guide/model.png)

* The **inventory** stores all master data related to devices, their configuration and their connections. It also contains all related assets (like vehicles, machines, buildings) and their structure.

* **Measurements** contain numerical data produced by sensors (like temperature readings) or calculated data based on information from devices (service availability of a device).

* **Events** contain other real-time information from the sensor network, such as the triggering of a door sensor. Events can also be **alarms**.The user or operator of the system has to take action to resolve the alarm (like a power outage). In addition, security-related events are shown as audit logs.

* **Operations** relate to data that is sent to devices for execution or processing, such as switching a relay in a power meter or sending a credit to a vending machine.

One of the great innovations in Cumulocity is its standardized representation of common devices and sensors as well as concepts for flexibly extending and modifying this representation. By default, Cumulocity comes with detailed visualizations of  sensors, smart meters, trackers and other devices. It has many options to fit in local customizations.

As a result, Internet of Things applications can be written independently from connected devices and underlying sensor networks, customized for specific cases in different web configurations or different devices from manufacturers.

The following sections are a walk-through of those concepts and will describe the ideas behind it and give you examples. The format these examples use is Cumulocity's REST APIs. For usability with other programming languages, refer to the specified sections in the [Reference guide](/reference).
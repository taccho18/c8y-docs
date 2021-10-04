---
weight: 40
title: Monitoring microservices
layout: redirect
---

You can monitor microservices hosted by {{< product-c8y-iot >}} in two ways.

### Monitoring status information

The status of the microservice can be checked on the **Status** tab of the respective application.

<img src="/images/users-guide/Administration/admin-microservice-status.png" alt="Microservice status" style="max-width: 100%">

The following information is provided on the **Status** tab:

* Instances: Number of active, unhealthy and desired microservice instances for the current tenant
* Subscribed tenants: Number of active, unhealthy and desired microservice instances for all subtenants subscribed to the microservice
* Alarms: Alarms for given application, provided in realtime
* Events: Events for given application, provided in realtime

The status information is available for subscribed applications as well as for own applications. Information on subscribed subtenants is only visible for the application owner.

To view the status you need the following permissions: ROLE&#95;APPLICATION&#95;MANAGEMENT&#95;READ and ROLE&#95;INVENTORY&#95;READ

#### Alarms and events

Most of the alarms and events visible in the **Status** tab are strictly technical descriptions of what's going on with the microservice.

There are two user-friendly alarm types:

* `c8y_Application_Down` - critical alarm which is created when no microservice instance is available
* `c8y_Application_Unhealthy` - minor alarm which is created when there is at least one microservice instance working properly, but not all of them are fully operating

User-friendly alarms are created for the microservice owner tenant only. They are also automatically cleared when the situation gets back to normal, i.e. all the microservice instances are working properly.

User-friendly alarms can be used to create smart rules. For details on creating smart rules of various types, see [Smart rules](/users-guide/cockpit/#smart-rules).

For example, to send an email, if a microservice is down, create an "On alarm send email" Smart Rule.

In the **On alarm matching** section, use `c8y_Application_Down` as an alarm type. As a target asset select the microservice which you would like to monitor, for example "echo-agent-server".

### Viewing log files

{{< product-c8y-iot >}} offers viewing logs which provide more details on the status of microservices.

To view logs, open the **Logs** tab of the respective microservice.

<img src="/images/users-guide/Administration/admin-applications-logs.png" alt="Microservice log" style="max-width: 100%">

At the top of the page, you can select the instance of the microservice, for which you want to view the logs.

> **Info:** If your microservice was re-scaled into two instances you should be able to switch between them, but it is not possible to see the logs from both instances at once.

Next to the instance dropdown you can select the time range for the log entries to be shown by selecting a date from the calendar and entering a time.

> **Info:** The time entered here may differ from the server time due to different time zones.

At the top right, additional functionality is provided:

* **Download** - To download the log data for a specified time range.
* **Dark theme** - To turn dark theme on or off.
* **Auto refresh** - To activate the auto refresh functionality. If activated, the displayed log data will automatically be refreshed every 10 seconds.
* **Unsubscribe** - To unsubscribe the microservice.
* **Delete** - To delete the microservice.

Initially, the **Logs** tab shows the latest logs of the microservice instance.

At the bottom right you find navigation buttons:

* **First** - directly navigates to the oldest available log entries for the microservice after its restart (maximum capacity 350MB of logs).
* **Previous** - increases the time range in 10 minutes steps.
* **Next** - reduces the time range in 10 minutes steps.
* **Last** - directly navigates to the latest available log entries.

If no logs are available in the selected time range, a message is shown accordingly:

<img src="/images/users-guide/Administration/admin-microservice-no-logs.png" alt="Microservice log">

> **Info:** There is no possibility to see the logs from the previously running instances. However, inside the instance there is a Docker container running, and if only this one was restarted (not the whole instance) you should see the logs from the currently running and also lately terminated Docker container.

>Logs are always loaded from the Docker container using both `stdout` and `stderr` sources, and there is no possibility to distinguish/filter by the source.

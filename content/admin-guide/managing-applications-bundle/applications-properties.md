---
weight: 10
title: Application properties
layout: redirect
---

In the [Own applications](#own-applications) or [Subscribed applications](#subscribed-applications) list, available through the **Applications** menu in the navigator click on an application item to view the application properties.

<img src="/images/users-guide/Administration/admin-application-properties.png" alt="Application properties" style="max-width: 100%">

Each application will show the following properties, depending on the application type:

<table>
<col width= 20%>
<col width= 20%>
<col width= 20%>
<col width= 20%>
<col width= 20%>
<thead>
<tr>
<th style="text-align:left">Field</th>
<th style="text-align:left">Description</th>
<th style="text-align:left">Hosted (Web app)</th>
<th style="text-align:left">Microservice</th>
<th style="text-align:left">External</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">ID</td>
<td style="text-align:left">Unique ID to identify the application</td>
<td style="text-align:left">Automatically provided</td>
<td style="text-align:left">Automatically provided</td>
<td style="text-align:left">Automatically provided</td>
</tr>
<tr>
<td style="text-align:left">Name</td>
<td style="text-align:left">Application name. Will be shown as title of the application in the top bar and in the application switcher.</td>
<td style="text-align:left">Automatically created</td>
<td style="text-align:left">Automatically created, based on the ZIP file name</td>
<td style="text-align:left">Specified by the user</td>
</tr>
<tr>
<td style="text-align:left">Application key</td>
<td style="text-align:left">Used to identify the application and to make the application available for subscription, see the <a href="/concepts/applications" class="no-ajaxy">Concepts guide</a>.</td>
<td style="text-align:left">Automatically created</td>
<td style="text-align:left">Automatically created based on the ZIP file name</td>
<td style="text-align:left">Specified by the user</td>
</tr>
<tr>
<td style="text-align:left">Type</td>
<td style="text-align:left">Application type</td>
<td style="text-align:left">Hosted application</td>
<td style="text-align:left">Microservice</td>
<td style="text-align:left">External</td>
</tr>
<tr>
<td style="text-align:left">Path</td>
<td style="text-align:left">Part of the URL invoking the application</td>
<td style="text-align:left">Automatically created</td>
<td style="text-align:left">Automatically created as .../service/&lt;microservice name&gt;</td>
<td style="text-align:left">Specified by the user. For example, if you use "hello" as application path, the URL of the application will be "/apps/hello".</td>
</tr>
</tbody>
</table>

In case of applications of the type "microservice", you will additionally find information on its version, as well as on its isolation level and billing mode, see [{{< enterprise-tenant >}} > Managing tenants > Microservice usage](/users-guide/enterprise-tenant/#microservice-usage) for details on these parameters.  

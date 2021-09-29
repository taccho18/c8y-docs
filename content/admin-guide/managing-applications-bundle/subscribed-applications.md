---
weight: 20
title: Subscribed applications
layout: redirect
---

{{< product-c8y-iot >}} provides a variety of applications for different purposes.

Depending on your installation and/or optional services your tenant will show a selection of the potentially available applications listed below.

The columns show the following information:

* **Application**: Application name as visible in the Administration application.
* **Functionality**: Brief description.
* **Name**: Identification of the application in the API. In case you want to subscribe a tenant to the application using an API, use this string in the argument (as name).
* **Type**: Technical type of the application. "Feature" refers to built-in applications subscriptions, i.e. these applications are not represented by an explicit artefact (microservice or web application).

### Default applications

In the {{< standard-tenant >}} you will find the following default applications:

<table>
<col width="200">
<col width="350">
<col width="200">
<thead>
<tr>
<th style="text-align:left">Application</th>
<th style="text-align:left">Functionality</th>
<th style="text-align:left">Name (as used in the API)</th>
<th style="text-align:left">Type</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left"><a href="/users-guide/administration" class="no-ajaxy">Administration</a></td>
<td style="text-align:left">Lets account administrators manage users, roles, tenants and applications.</td>
<td style="text-align:left">administration</td>
<td style="text-align:left">Web app</td>
</tr>
<tr>
<td style="text-align:left"><a href="/apama/overview-analytics/" class="no-ajaxy">Apama-ctrl*</a></td>
<td style="text-align:left">Runtime for Analytics Builder, EPL Apps, and smart rules.</td>
<td style="text-align:left">apama-ctrl-* (different strings for different size/capability options)</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/apama/overview-analytics/" class="no-ajaxy">Streaming Analytics</a></td>
<td style="text-align:left">Manage and edit Analytics Builder models and EPL apps (if enabled).</td>
<td style="text-align:left">Streaming Analytics</td>
<td style="text-align:left">Web app</td>
</tr>
<tr>
<td style="text-align:left"><a href="/event-language" class="no-ajaxy">Cep</a></td>
<td style="text-align:left"><b>This application is deprecated and no longer a default application in the {{< standard-tenant >}}. Apama now is the standard CEP engine.</b> <br>Define business operations based on realtime data by using the Esper CEP engine. This CEP variant uses a shared instance for multiple tenants. See "Cep-small" for a per-tenant approach.</td>
<td style="text-align:left">cep</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/cockpit" class="no-ajaxy">Cockpit</a></td>
<td style="text-align:left">Manage and monitor IoT assets and data from a business perspective.</td>
<td style="text-align:left">cockpit</td>
<td style="text-align:left">Web app</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/device-management" class="no-ajaxy">Device Management</a></td>
<td style="text-align:left">Manage and monitor devices, and control and troubleshoot devices remotely.</td>
<td style="text-align:left">devicemanagement</td>
<td style="text-align:left">Web app</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/device-management#simulator" class="no-ajaxy">Device simulator</a></td>
<td style="text-align:left">Simulate all aspects of IoT devices.</td>
<td style="text-align:left">device-simulator</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/cockpit#reports" class="no-ajaxy">Report agent</a></td>
<td style="text-align:left">Allows to schedule data exports from within the Cockpit application.</td>
<td style="text-align:left">report-agent</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/cockpit#smart-rules" class="no-ajaxy">Smart Rules</a></td>
<td style="text-align:left">Use the Smart Rule engine and create <a href="/users-guide/cockpit#smart-rules" class="no-ajaxy">Smart Rules</a> to perform actions based on realtime data. Requires one of the following applications: "Cep", "Apama"</td>
<td style="text-align:left">smartrule</td>
<td style="text-align:left">Microservice</td>
</tr>
</tbody>
</table>


### Enterprise applications

In the {{< enterprise-tenant >}} you will find the following additional applications:

<table>
<col width="200">
<col width="350">
<col width="200">
<thead>
<tr>
<th style="text-align:left">Application</th>
<th style="text-align:left">Functionality</th>
<th style="text-align:left">Name (as used in the API)</th>
<th style="text-align:left">Type</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left"><a href="/users-guide/enterprise-tenant/#branding" class="no-ajaxy">Branding</a></td>
<td style="text-align:left">Customize the look of your tenants to your own preferences.</td>
<td style="text-align:left">feature-branding</td>
<td style="text-align:left">Feature</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/enterprise-tenant/#data-broker" class="no-ajaxy">Data Broker</a></td>
<td style="text-align:left">Lets you share data selectively with other tenants.</td>
<td style="text-align:left">feature-broker</td>
<td style="text-align:left">Feature</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/enterprise-tenant#customization" class="no-ajaxy">SSL management</a></td>
<td style="text-align:left">Activate your own custom domain name by using a SSL certificate.</td>
<td style="text-align:left">sslmanagement</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/enterprise-tenant/#user-hierarchies" class="no-ajaxy">User hierarchies</a></td>
<td style="text-align:left">Reflect independent organizational entities in {{< product-c8y-iot >}} that share the same database.</td>
<td style="text-align:left">feature-user-hierarchy</td>
<td style="text-align:left">Feature</td>
</tr>
</tbody>
</table>

### Optional applications

<table>
<col width="200">
<col width="350">
<col width="200">
<thead>
<tr>
<th style="text-align:left">Application</th>
<th style="text-align:left">Functionality</th>
<th style="text-align:left">Name (as used in the API)</th>
<th style="text-align:left">Type</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left"><a href="/protocol-integration/lora-actility" class="no-ajaxy">Actility</a></td>
<td style="text-align:left">Interface with LoRa devices through the Actility ThingPark.</td>
<td style="text-align:left">actility</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/event-language" class="no-ajaxy">CEP custom rules</a></td>
<td style="text-align:left"><b>This application is deprecated.</b><br>Upload your own CEP rules created with Esper in a per-tenant deployment. You need to be subscribed to the application "Cep-small" to be able to use this feature.</td>
<td style="text-align:left">feature-cep-custom-rules</td>
<td style="text-align:left">Feature</td>
</tr>
<tr>
<td style="text-align:left"><a href="/event-language" class="no-ajaxy">Cep-small</a></td>
<td style="text-align:left"><b>This application is deprecated.</b><br>CEP variant. Lets you work with CEP rules based on Esper in a per-tenant deployment (as opposed to "Cep" which uses a shared instance). You need to be subscribed to "CEP custom rules" to upload your own Esper CEP rules.</td>
<td style="text-align:left">cep-small</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/protocol-integration/cloud-fieldbus" class="no-ajaxy">Cloud Fieldbus</a></td>
<td style="text-align:left">Collect data from fieldbus devices and remotely manage them in {{< product-c8y-iot >}}.</td>
<td style="text-align:left">feature-fieldbus4</td>
<td style="text-align:left">Feature</td>
</tr>
<tr>
<td style="text-align:left"><a href="/cloud-remote-access/cra-general-aspects" class="no-ajaxy">Cloud Remote Access</a></td>
<td style="text-align:left">Implements Virtual Network Computing (VNC) to remotely access operating panels and other devices via a web browser.</td>
<td style="text-align:left">cloud-remote-access</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/users-guide/device-management/#connectivity" class="no-ajaxy">Connectivity</a></td>
<td style="text-align:left">Interface with mobile devices through various SIM providers like Jasper, Ericsson and Comarch.</td>
<td style="text-align:left">connectivity-agent-server</td>
<td style="text-align:left">Microservice</td>
</tr>
<td style="text-align:left">Microservice hosting</td>
<td style="text-align:left">Host your own microservices on top of {{< product-c8y-iot >}}.</td>
<td style="text-align:left">feature-microservice-hosting</td>
<td style="text-align:left">Feature</td>
</tr>
<tr>
<td style="text-align:left"><a href="/protocol-integration/opcua" class="no-ajaxy">OPC UA</a></td>
<td style="text-align:left">Communicate with OPC UA servers through an OPC UA device gateway. </td>
<td style="text-align:left">opcua-mgmt-service</td>
<td style="text-align:left">Microservice</td>
</tr>
<tr>
<td style="text-align:left"><a href="/protocol-integration/sigfox" class="no-ajaxy">Sigfox</a></td>
<td style="text-align:left">Interface with Sigfox devices through the Sigfox cloud.</td>
<td style="text-align:left">sigfox-agent</td>
<td style="text-align:left">Microservice</td>
</tr>
</tr>

</tbody>
</table>

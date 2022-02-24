---
weight: 70
title: Changing settings
layout: redirect
---


From the **Settings** menu, administrators can manage various settings for the account:

- Configure [authentication settings](#authentication), [two-factor authentication](#tfa) and [single sign-on](#single-sign-on).
- Change the [application settings](#default-app).
- Manage the [properties library](#properties).
- Provide [SMS provider credentials](#sms-provider).
- Manage the [connectivity settings](#connectivity).


<a name="default-app"></a>
### Application

Click **Application** in the **Settings** menu to change applications settings.

![Default application](/images/users-guide/Administration/admin-settings-application.png)

Under **Default application**, you can select a default application from the list which will apply to all users within the tenant. Whenever the platform is accessed, for example, by domain name only, without mentioning a specific application, the application selected as default application is used as default landing page.

>**Info:** All users must have access to this application.

Under **Access control**, administrators can enable cross-origin resource sharing (CORS) on the {{< product-c8y-iot >}} API.

The **Allowed Domain** setting will enable your JavaScript web applications to directly communicate with REST APIs.

* Set it to "*" to allow communication from any host.
* Set it to `http://my.host.com`, `http://myother.host.com` to allow applications from `http://my.host.com` and from `http://myother.host.com` to communicate with the platform.

For further information, see [http://enable-cors.org](http://enable-cors.org).

<a name="properties"></a>
### Properties library

Click **Properties library** in the **Settings** menu, to add custom properties to inventory objects, alarms, events and tenants.

![Properties library](/images/users-guide/Administration/admin-settings-properties-library.png)

With custom properties, you can extend the data model of {{< product-c8y-iot >}} built-in objects. You may create the following custom values:

- Custom inventory properties are used to extend the inventory data model. They can be used in the "Asset table" and "Asset properties" widgets.
- Custom tenant properties are available during tenant creation. The custom properties can be edited under **Subtenants** in the **Custom properties** tab of each tenant. Additionally, these properties can be viewed and exported in the **Usage statistics**.
- Custom alarm and event properties can be used as custom fields which can be added to your reports and will be available in the **Export** page in the Cockpit application.

>**Info:** Custom properties are visible to all authenticated users of the tenant, regardless of their inventory role permission.

<a name="add-property"></a>
#### To add a custom property

1. Select the tab for the desired property and click **Add property**.

	![Add new property](/images/users-guide/Administration/admin-settings-property-add.png)

1. In the resulting dialog box, provide a unique name as identifier and a label for the property and select its data type from the dropdown list.

1. Additionally, select validation rules for the new property:

<table>
<colgroup>
<col width="20%">
<col width="80%">
</colgroup>
<thead>
<tr>
<th style="text-align:left">Checkbox</th>
<th style="text-align:left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">Required</td>
<td style="text-align:left">If selected, the property needs to be provided, i.e. during alarm creation. Not available if the property type is "Boolean".</td>
</tr>
<tr>
<td style="text-align:left">Default Value</td>
<td style="text-align:left">Provide a default value to be automatically filled in the custom property field. Only available for properties with type "String".</td>
</tr>
<tr>
<td style="text-align:left">Minimum</td>
<td style="text-align:left">Enter a minimum integer value.</td>
</tr>
<tr>
<td style="text-align:left">Maximum</td>
<td style="text-align:left">Enter a maximum integer value.</td>
</tr>
<tr>
<td style="text-align:left">Minimum length</td>
<td style="text-align:left">Enter the minimum length required for the string.</td>
</tr>
<tr>
<td style="text-align:left">Maximum length</td>
<td style="text-align:left">Enter the maximum length required for the string.</td>
</tr>
<tr>
<td style="text-align:left">Regular expression</td>
<td style="text-align:left">Add a regular expression which will be required in order to fill the custom property field.</td>
</tr>
</tbody>
</table>

4. Click **Save** to create the new property.

#### To edit a custom property

1. Click on the name of a property in the list to open it.
2. Do your edits. For details on the fields see [To add a custom property](#add-property).
3. Click **Save** to save your settings.


#### To remove a custom property

1. Click on the name of a property in the list to open it.
2. Click **Remove** to delete the property.

<a name="sms-provider"></a>
### SMS provider

SMS are used throughout the platform for various features like [two-factor authentication](/users-guide/administration#tfa) and user notifications, for example, on alarms. By providing your credentials you enable platform features that utilize SMS services.

>**Requirements** You need the following permissions:
- To see the **SMS provider** page: READ rights for the permission category "SMS"
- To modify the **SMS provider** configuration: ADMIN rights for the permission category "SMS"

#### To enter SMS provider credentials

1. Click **SMS provider** in the **Settings** menu.

    ![Select SMS provider](/images/users-guide/Administration/admin-settings-sms-provider.png)

2. In the **SMS provider** page, select one of the available SMS providers from the **SMS provider** dropdown field. You can start typing to filter items to more easily find your preferred provider.

3. In the resulting dialog, enter the required credentials and properties or specify optional settings, which differ depending on the provider you selected.

4. Click **Save** to save your settings.

>**Info:** OpenIT does not serve new customers anymore and is in the process of shutting down their SMS provider business. We therefore recommend you to select one of the other SMS providers.


<a name="connectivity"></a>
### Connectivity

In the **Connectivity** page, you can manage credentials for different providers. In order to add or replace credentials ADMIN permissions are required.

The following provider settings may be specified:

- [Actility LoRa](/protocol-integration/lora-actility)
- [Sigfox](/protocol-integration/sigfox)
- [SIM](/users-guide/device-management/#connectivity)

![Provider settings](/images/users-guide/Administration/admin-settings-connectivity.png)

#### To provide or replace credentials

1. Switch to the tab of your desired provider.
2. Enter the URL of the provider.
3. Enter the credentials of your provider platform. Depending on the provider, these credentials will be either the credentials of your account in the provider platform or the credentials with which you can register in the {{< product-c8y-iot >}} connectivity page, will be displayed in your account in the provider platform.
4. Click **Save** to save your settings.

Depending on the provider you have selected, there may be additional fields, which will be explained in the respective protocol integration section, see [Protocol integration guide](/protocol-integration/overview/).

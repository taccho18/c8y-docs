---
weight: 30
title: Own applications
layout: redirect
---

Own applications may be

* duplicates of subscribed applications (in order to be able to customize them)
* web-based UI applications, either deployed as standalone applications or as plugins deployed into a specific application (e.g. a widget to the Cockpit dashboard)
* server-side business logic deployed through microservices

Your applications are available through the application switcher in the top bar which allows to easily switch between applications.

<img src="/images/users-guide/Administration/admin-app-switcher.png" alt="App switcher">

You manage your applications under **Own applications**, accessible through the **Applications** menu.

In the **Own applications** page you will find a list of the applications available in your account.

<img src="/images/users-guide/Administration/admin-applications-own.png" alt="Own applications">

To display further information on the application, simply click its card. For details on the fields, refer to [Application properties](#application-properties).

To directly open an application from here, click **Open** on the respective application card.


<a name="adding-applications"></a>
### To add an own application

Click **Add application** in the **Own applications** page.

<img src="/images/users-guide/Administration/admin-application-add.png" alt="Add application methods">

In the resulting dialog box, choose one of the following methods:

* [Upload web application](#uploading-zip-files) - by dropping a ZIP file or browsing for it on your computer.
* [Upload microservice](#uploading-microservices) - by dropping a ZIP file or browsing for it on your computer
* [External application](#external-application) - by linking to an application running elsewhere
* [Duplicate existing application](#clone-application) - by creating a copy of an existing application

<a name="uploading-zip-files"></a>
#### To upload a web application

1. Click **Add application** in the **Own applications** page.
2. Select **Upload web application**.
3. In the resulting dialog box, drop a ZIP file or browse for it on your computer.

The application is created once the ZIP file has been successfully uploaded.

>**Important:** The ZIP file must contain the *index.html* and *cumulocity.json* in its root directory, otherwise the application will not work.

<a name="uploading-microservices"></a>
#### To upload a microservice

1. Click **Add application** in the **Own applications** page.
2. Select **Upload microservice**.
3. In the resulting dialog box, drop a ZIP file or browse for it in your file system. Note that the size limit of the file to be uploaded is 500 MB.

The microservice application is created once the ZIP file has been successfully uploaded.

>**Important:** The ZIP file must contain the application manifest and the Docker image of the microservice. Refer to [Packing](/microservice-sdk/concept/#packing) in the *Microservice SDK guide* under **General aspects** in order to prepare and deploy the microservice package.


<a name="external-application"></a>
#### To link to an external application

1. Click **Add application** in the **Own applications** page.
2. Select **External application**.
<br><br>
<img src="/images/users-guide/Administration/admin-application-external.png" alt="External application">
<br><br>
3. In the resulting dialog box, enter the name of the application. The name will be shown as title of the application.
5. Enter an application key, used to identify this application.
6. Enter the external URL where the application can be reached.
7. Click **Save** to create the application.

For details on the fields, see also [Application properties](#application-properties) below.

<a name="clone-application"></a>
#### To duplicate an application

Duplicating an application might be useful if you want to customize a subscribed application according to your needs. Duplicating a subscribed application creates a copy of the application as an own application, with a link to the original application.

1. Click **Add application** in the **Own applications** page.
2. In the upcoming dialog, select **Duplicate existing application**.
3. Select the desired application from the dropdown list.
<br><br>
<img src="/images/users-guide/Administration/admin-application-duplicate.png" alt="Duplicate application">
<br><br>
4. In the next window, provide a name for the application. By default, the name of the original application is provided, extended by a number.
<br><br>
<img src="/images/users-guide/Administration/admin-application-duplicate-2.png" alt="Duplicate application">
<br><br>
5. Provide an application key, used to identify this application. By default, the key of the original application is provided, extended by a number.
6. Provide the application path as part of the URL to invoke the application. By default, the path of the original application is provided, extended by a number. If you set it to the path of the original subscribed application, your own application will overrule the subscribed application.
7. Finally, click **Duplicate** to create the application.

For details on the fields, see also [Application properties](#application-properties) below.

> **Info:** If you want your "own application" to overrule a subscribed standard application, the path of the "own application" needs to be set to the path of the original subscribed application.


<a name="editing-and-removing"></a>
### To edit an own application

Simply click the application or click the menu icon at the right of an entry and then click **Edit**.

In the **Properties** tab, several fields can be modified, depending on the application type (see [Application properties](#application-properties)).

>**Important:** Never change the system application names (e.g. "Device Management", "Cockpit"). Otherwise, tenant initialization will fail.


### To remove an own application

Click the menu icon at the right of an entry and then click **Remove**.

If you remove an application that overwrites a subscribed application, the currently subscribed application becomes available to all users. Additionally, the users will then also benefit from future upgrades of the subscribed application.

It is not possible to remove subscribed applications. This can only be done by the owner of the subscribed application.

### Uploading archives

Multiple archive file versions can be stored in {{< product-c8y-iot >}} when they were created by uploading either a ZIP file or a MON file. Each version is called an archive. You can upload different versions at the same time and switch between these versions.

#### To upload an archive

1. Open the application by clicking on it.
2. Switch to the **Archives** tab.
3. Click **Upload archive** and browse for the archive on your computer or simply drop the archive file.
4. Click **Upload** to upload the archive to your {{< product-c8y-iot >}} account.

<img src="/images/users-guide/Administration/admin-application-archive.png" alt="Application archive">

Once uploaded, the recently uploaded version is automatically the active version, i.e. the version of the application that is currently being served to the users of your account. This version cannot be deleted.

> **Info:** The **Archive** tab is not available for subscribed applications, as only the owner of the application can perform this action.

#### To restore an older application version

Users can restore previous versions of an application from an archive.

1. Open the application by clicking on it.
2. Switch to the **Archives** tab.
3. Open the context menu for the desired version by clicking the menu icon and select **Set as active** to make it the active version.
4. Click **Remove** to remove the version from the archive.

<img src="/images/users-guide/Administration/admin-application-set-as-archive.png" alt="Application set as archive">

#### To reactivate a single application

If a hosted application is not deployed correctly, users may reactivate it.

1. Open the application by clicking on it.
2. Switch to the **Archives** tab.
3. Click **Reactivate** at the top right corner of the **Archives** tab.

<img src="/images/users-guide/Administration/admin-reactivate.png" alt="Refresh application">  

The selected application will be reactivated by removing the respective files from the application directory and unpacking the host application package again.

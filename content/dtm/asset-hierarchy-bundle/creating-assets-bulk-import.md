---
weight: 20
title: Creating assets via bulk import
layout: redirect
---


If you want to import the entire **Asset hierarchy** with all assets at once, you can use the bulk import feature. For each [Asset type](/dtm/asset-types/#asset-types), a CSV template is provided.

You can fill in the required details in this template and upload this file, to create the assets in bulk for the selected **Asset type**.

##### Creating a role to use Bulk import feature

To use the Bulk import feature, you must give permissions for **Digital Twin** role, following these steps:


Navigate to [**Administration**](/users-guide/administration/#overview) via the application switcher in the top right corner.

Under **Quick links** click **Roles**.

In the **Global roles** tab, select the role which is already assigned for your username.

Set all available permissions for **Digital Twin** under "Permissions" section by selecting the following check boxes:

                * Under **Application access**, select the "Digital Twin".

                     * Under **Custom applications**, select the "Digital Twin Manager".

Click **Save**.

![Permissions for bulk import](/images/dtm/bulk-import/dtm-bulk-import-assign-permissions.png)


Once the permissions are assigned, bulk import feature can be used.

{{< c8y-admon-info>}}
If the permissions are not assigned, then the CSV template will not get downloaded correctly.
{{< /c8y-admon-info>}}

##### Download CSV template for bulk import

You have to download the CSV template for the required **Asset type** first.


* Navigate to **Assets** page. Click on **Import assets**.

* IMPORT ASSETS window loads with a “Choose Asset type” dropdown.

* If no asset types are created yet, then only “Group” asset type is listed in the dropdown.

* If you have added the asset types, all the Root Asset types will be listed in the dropdown.

* Select the **Asset type** for which the Asset hierarchy must be created.

![bulk-import-asset](/images/dtm/bulk-import/dtm-bulk-import-import-assets-window.png)


* On selection, below 2 additional options appear.
1.	**Drop file here** section: An option to upload the CSV template as file, for importing assets in bulk.
2.	**Download Template** option: To download the CSV template for chosen **Asset type**.

* Click on **Download Template** option to download the CSV template.


##### CSV template attributes


The CSV template has the following fields:

*	AssetType/DeviceType : Fill the Key of the Asset type here. This field is mandatory.
*	AssetName : Provide the name of the Asset to be created here. AssetName is mandatory.
*	Path : If you are creating a root asset, this field will be blank. If you are creating a child asset, then the path must be provided until the parent asset.

**Example:**

If the hierarchy is like “Building --> Floor --> Room” and you want to enter details to create a Room asset “Room 1”. Then **Path** value for “Room 1” asset will be “Building 1/Floor 1”, where “Building 1” and “Floor 1” are the assets for Building and Floor Asset types respectively.

*	Device ID/External ID : If the asset being created has a device associated with it, then provide the Device ID of the device here. This field is optional.
*	Description : Optional field Description briefly describes the asset being created.
*	Custom properties for the asset type : 6th column onwards (in the CSV template), all the custom properties for the root asset types and all its subsequent child asset types are listed.

The custom properties which are mandatory are mentioned with a “required” label.

![bulk-import-template](/images/dtm/bulk-import/dtm-bulk-import-template.png)


The [**Type**](/dtm/asset-types/#types-of-custom-property) of the custom property is also mentioned as a label, for better understanding.

{{< c8y-admon-info>}}
Modify the excel settings so that date can be provided in yyyy-mm-dd format, so that date is not auto corrected on input.
{{< /c8y-admon-info>}}

If the **Type** of Custom property is **Date**, then input must be provided in yyyy-mm-dd format.

If you want to provide a file input as custom property value, then first the file must be uploaded in Cumulocity tenant using {{< product-c8y-iot >}} API.

Refer to this section to upload a file in Cumulocity : [Provide link here]

The Binary ID in the API response must be provided as input for the “File” type custom property field in the CSV template.

If the **Type** is **Boolean**, then input field must be either “true” or “false”.

If the **Type** of Custom property is **Enumeration**, then the input field must belong to the list of values mentioned during creation of Custom property.

If the **Type** is **Text** or **Number** and criteria was provided during Custom property creation, then the input value in CSV template must fulfil all the criteria.

Fill in details for all the assets which must be created as part of the Asset hierarchy.

Start with the asset details for root asset type. Then enter the asset details for child asset types under the root asset.
Next, enter the details for second level hierarchy of child assets and continue this until details for all the assets are entered in the CSV template.

Once you fill in all details, save the CSV template in your system.



##### Uploading the CSV template

To upload the CSV template follow the below steps:

* Click on **Import assets** in **Assets** page.

* Choose the asset type in IMPORT ASSETS window.

* Upload the previously filled and saved CSV template in the **Drop file here** section.

* If all the inputs are provided correctly, bulk import is successful and same is notified with a pop-up notification. The Asset hierarchy with all the Assets are created successfully.

* You can view the newly created assets in **Assets** page.

If the inputs are not provided in the correct format, bulk import fails with validation errors and same is displayed in a FAILED ASSET IMPORTS window. No Assets are created.

![bulk-import-failed-import](/images/dtm/bulk-import/dtm-bulk-import-failed-asset-imports.png)


You can rectify the issues and try the import again.

If there is an issue with the template being uploaded (example: invalid template or incorrect asset hierarchy), then the same is notified with a pop-up notification and assets will not be created.



##### Partial Import

In case the Asset hierarchy is already existing, and you want to import some more Asset/s, then same can be achieved by Partial import.

To import a single child asset or the child asset hierarchy, first navigate to the child asset page and download the template using **Import assets** option.

Next, fill in details for the child asset hierarchy in the CSV template.

Refer to this section for details on filling the CSV template: [Filling CSV template for bulk import](/dtm/asset-hierarchy/#csv-template-attributes)

{{< c8y-admon-info>}}
From any hierarchy level, template will be downloaded only for its succeeding child hierarchy. And Assets must be imported only for that child hierarchy.
{{< /c8y-admon-info>}}

Once you enter all the details correctly for child asset in the CSV template, upload the template using below steps:

* Click on **Import assets**.

* Upload the CSV template in the **Drop file here** section in IMPORT ASSETS window.

* If all details are mentioned correctly in CSV template, the child asset hierarchy is created successfully.

If partial import is not successful, same is notified with a pop-up notification.

In case there are validation issues, it is displayed in **Failed Asset imports** window. Rectify the errors and try again with valid inputs.
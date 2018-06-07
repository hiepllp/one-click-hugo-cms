---
title: Importing data into Acumatica
date: 2018-06-06T09:58:56.326Z
description: Some you need to know about Import by Scenario
---
First thing first - Prepare the data, the excel sheet

![excel](/img/excel.png)

Normally, the template includes 2 sheets, Template and Data, but it can be more, it's not important since you will have a chance to tell the system later which will contain the data you want the system to import to. Likewise, you can have as many columns as you want in the Data sheet, or any sheet of the file. Usually only some key fields will be necessary.

In the below example, we're about to import the Vendor list into the system. As it requires the Vendor Class for each vendor, so you need to define the Vendor Classes first. Here I have some already:

![vendor class](/img/vendorclass1.png)

Now we need to map all the vendors with the appropriate classes you had in the current sytem, MAS for example. Of course before that, assuming that you already exported the list with basic info such as VendorID, Vendor Name, Payment Terms, Credit Limit, etc.

Now everything is ready for preparation. It's time to upload the template and proceed the import. Enter 'data provider' into the Search bar, you should see the Data Providers menu under Integration Profiles. Select it, and you're ready to go:

![Data Provider](/img/step0.png)

Here you landed on the definition for the data provider, on the _**Name**_, just name it a descriptive name such as **Import Vendors **this case, and select the **Excel Provider** for the _**Provider Type**_.

On the **Parameters** tab, you need to click on the **File** on the top right corner to specify the file you want it to be the template. So let's select the file you just prepared in the previous step. Mine is this Vendor list file. After upload completed, you should see the File(1) as the pic shown.

Next, navigate to the second tab, the **Schema**. Here on the right pane, you should see all the sheets in the excel just uploaded, then you need to check only on the **Data** sheet, which is the one containing all the fields you want to feed with Vendor info into Acumatica. Hit **Fill Schema Fields** button. It will automtaically check all the fields on the selected sheet as defaut. Now click on the **Toggle Activation** button to uncheck all, then manually select only the key fields which the Vendor screen needs (will talk to it later below), which are the VendorId, Description (Vendor Name), and Vendor Class.

![Step 1](/img/step1.png)

Next click **Save **button to save the Data Provider. You're done at this step.

Now we continue with second step - Import Scenarios

![Step 2](/img/step2.png)

At this step you need to define Import Scenarios for Vendors screen. Now search for Import Scenarios in the Search bar. Select it and the main page shows up, lets you specify following: 

_**Name:**_ Enter a descriptive name, I used _Import Vendors_

_**Screen Name:**_ at the pop-up window, navigate like below, then double click the _Vendor_ to select it:

![Screenname](/img/screenname.png)

_**Provider:**_ select the provider name you created last step, mine is Import Vendors

**_Provider Object:_** select **Data**, which is the Data sheet in the template

_**Sync Type:**_ leave it as default, **Full** mode for the first time import. You may use the latter 2 for incremental import if you need to update the current records which were imported before, or to add a new record to the existing set.

Leave the _**Format Locale**_ and **_Inverse Mapping ID_** empty as we don't need it now.

Now  you're done step 2

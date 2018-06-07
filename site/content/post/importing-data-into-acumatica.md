---
title: Importing data into Acumatica
date: 2018-06-06T09:58:56.326Z
description: Some you need to know about Import by Scenario
---
First thing first - Prepare the data, the excel sheet.

![excel](/img/excel.png)

Normally, the template includes 2 sheets, _Template_ and _Data_, but it can be more, it's not important since you will have a chance to tell the system later which will contain the data you want the system to import to. Likewise, you can have as many columns as you want in the _Data_ sheet, or any sheet of the file. Usually only some key fields will be necessary.

In the below example, we're about to import the Vendor list into the system. As it requires the Vendor Class for each vendor, so you need to define the Vendor Classes first. Here I have some already:

![vendor class](/img/vendorclass1.png)

Now we need to map all the vendors with the appropriate classes you had in the current sytem, MAS for example. Of course before that, assuming that you already exported the list with basic info such as VendorID, Vendor Name, Payment Terms, Credit Limit, etc.

Now everything is ready for preparation. It's time to upload the template and proceed the import. Enter '_data provider_' into the Search bar, you should see the _Data Providers_ menu under Integration Profiles. Select it, and you're ready to go:

![Data Provider](/img/step0.png)

Here you landed on the definition for the data provider, on the _**Name**_, just name it a descriptive name such as **Import Vendors** this case, and select the **Excel Provider** for the _**Provider Type**_.

On the **Parameters** tab, you need to click on the **File** on the top right corner to specify the file you want it to be the template. So let's select the file you just prepared in the previous step. Mine is this Vendor list file. After upload completed, you should see the File(1) as the pic shown.

Next, navigate to the second tab, the **Schema**. Here on the right pane, you should see all the sheets in the excel just uploaded, then you need to check only on the **Data** sheet, which is the one containing all the fields you want to feed with Vendor info into Acumatica. Hit **Fill Schema Fields** button. It will automatically check all the fields on the selected sheet as defaut. Now click on the **Toggle Activation** button to uncheck all, then manually select only the key fields which the Vendor screen needs (will talk about it later below), which are the VendorId, Description (Vendor Name), and Vendor Class.

![Step 1](/img/step1.png)

Next click **Save** button to save the Data Provider. You're done at this step.

Now we continue with second step - _Import Scenarios_

![Step 2](/img/step2.png)

At this step you need to define Import Scenarios for Vendors screen. Now search for Import Scenarios in the Search bar. Select it and the main page shows up, lets you specify following: 

_**Name:**_ Enter a descriptive name, I used **Import Vendors**

_**Screen Name:**_ at the pop-up window, navigate like below, then double click the _Vendor_ to select it:

![Screenname](/img/screenname.png)

_**Provider:**_ select the provider name you created last step, mine is **Import Vendors**

**_Provider Object:_** select **Data**, which is the _Data_ sheet in the template

_**Sync Type:**_ leave it as default, **Full** mode for the first time import. You may use the latter 2 for incremental import if you need to update the current records which were imported before, or to add a new record to the existing set.

Leave the _**Format Locale**_ and **_Inverse Mapping ID_** empty as we don't need it now.

Now it comes to the most important part, you need to map the fields in the template with the Vendor screen fields. Basically import by scenario as its name implied, is the way to simulate exactly the steps it does/ behaves while processing user data entry, and such we need to respect when it submits the data for processing and when it doesn't. So let's take a look a little bit at how should we define the mapping.

First, click on the **View Screen** button to see **what are the key fields that the system requires** in this Vendor screen, **and where are they located exactly**. You will see that the key fields are marked as red star fields, which are the obligatory ones. So basically it's all the 3 fields we already included it in the template. It means you need to know this screen and necessary fields at the very beginning stage - the data template to prepare. For the 'where', just look at the area the screen showing. Main object would be something Summary, this case is Vendor Summary, which usually contains the Key field, in this case is _Vendor ID,_ and the related object will contain other fields, such as _General Info --> Financial Settings_ will contain _Vendor Class_, which in turns will pull out all the necessary inputs as it was set up in Vendor Class and fill up for the Vendor while importing.

Commit column checked indicates that the field will be submitted while processing right away, which will load other necessary related info and do the temporal save, which might automatically load other related fields and make the effect to the rest of inputs. Usually the fields with the magnify symbol will do this action.

After defining all the necessary information, you need to add an <Action: Save> to the end of the mapping to make the screen saved all info and finish the data entry.

You can refer more details here - _Target Objects and Fields in Import Scenarios_: https://help.acumatica.com/(W(28))/Main?ScreenId=ShowWiki&pageid=dca226e4-e566-4066-bf45-5930d06b3e5f 

Now  you're done step 2. Let's start with the last step - _Import by Scenarios_.

![Import by Scenario](/img/step3.png)

This is the easy step, you need to click on **Prepare**, and then **Import** to wait for the system doing the rest. If you're lucky then all the stuff will go through, and green check will show, indicates that all import are finished successfully.

After **Prepare**:

![prepare done](/img/afterprepare.png)

Now click Import:

![after import](/img/afterimport.png)

But sadly, it's usually not :). Sometimes because your data in the excel file has some problem with exceeding max data length that one of the fields allows, or some of the configured object like Item Class, Vendor Class, or anything related were incorrect. If the data is wrong, you need to modify the excel template and do the upload again before the next attempt to import, if the issue is at the configuration then nothing to do with the excel file, but that specific configuration to fix.

Above you can see that not all Vendors are imported, and the error mesasge doesn't helful much in telling us what was wrong. Here you need to review the specific failed row, and try to play the guess game. Here I recognized that all the Vendors bearing the MISC Vendor Class were not imported. So it should be something wrong with this Vendor Class. And I see that I set up the Payment method for it as FedWire, and the successfull ones are with Check (see the second pic on the top of the article). So I made a change to Check.

In reality, you may need to see what's actually wrong with this FedWire and fix it before next attempt of import, instead of just simply changing the class to another option like Check. But for the illustration purpose, I just do this. And surprisingly you can see they all get imported. Green visa approved. 

Congrats!

![alldone](/img/alldone.png)

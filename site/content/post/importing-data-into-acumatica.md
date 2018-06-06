---
title: Importing data into Acumatica
date: 2018-06-06T09:58:56.326Z
description: Some you need to know about Import by Scenario
---
First thing first - Prepare the data, the excel sheet

![excel](/img/excel.png)

Normally, the template includes 2 sheets, Template and Data, but it can be more, it's not important since you will have a chance to tell the system which will contain the data you want the system to import. Likewise, you can have as many columns as you want in the Data sheet, or any sheet of the file. Usually only the key fields will be necessary.

In the below example, we're about to import the Vendor list into the system. As it requires the Vendor Class for each vendor, so you need to define the Vendor Classes first. Here I have some already:

![vendor class](/img/vendorclass1.png)

Now we need to map all the vendors with the appropriate classes you had in the current sytem, MAS for example. Of course before that, assumed that you already export the list with basic info such as VendorID, Vendor Name, Payment Terms, Credit Limit, etc.

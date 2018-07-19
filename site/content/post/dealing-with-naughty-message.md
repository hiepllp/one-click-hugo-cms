---
title: Dealing with naughty message 'Margin(%)' cannot be empty.
date: 2018-07-19T07:09:12.053Z
description: Sales Order creation failed.
---
If you set up a new tenant with minimal settings available for it (not sufficient settings on all modules), you'll likely encounter such error when you pick a specific inventory item to add into your Sales Order.

The message is quite descriptive, which is telling us about the failure of inventory cost calculation (which is usually standard, average, or FIFO defined in the Stock Item screen as Valuation method.)

![Sales Order creation failed](/img/socreationfailed.jpg)

And if you trace it, it looks like:

![SO creation failed trace](/img/socreationfailed_trace.jpg)

So you can think about defining a standard cost for the item, in this case AM42FM item, but those attempts won't work:

\- Try changing the Last Cost field under _Stock Items > Price/Cost Info > Cost Statistics_ screen in to an expected value greater than 0

![Change the Last cost field](/img/lastcostchangedwontwork.jpg)

\- Try changing cost with Pending Cost and/or Last Cost field in Item Warehouse Details, then go to Process > Update Standard Costs to attempt to apply the change

![Pending cost to update](/img/pendingcosttoupdate.jpg)

![Process pending cost](/img/processupdatependingcost.jpg)

What the heck??!!!

So what will work? scratching my head.

Grab a cup of coffee or beer, and just type in Receipt in Search Bar, what you need to do is just receive the goods so that it will automagically do the rest for you, including the recalculation of the average/last cost

![Receipt to resolve](/img/receipt.jpg)

So, back to the Sales Order Entry screen, I was able to save the AM42FM with ordered qty 2, and create a drop shipment to other tenant.

Oh it works, I'm happy in 5 mins, but there are some paradox here:

* Since I intend to make the drop-shipment, so why do I need the stock available in my tenant in order to do so?

You might ask yourself this question, as I'm not alone: _https://www.timrodman.com/augforums/acumatica-distribution-modules/creating-a-drop-shipment/_

## Happy debugging!

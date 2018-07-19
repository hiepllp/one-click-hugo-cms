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

\- Try changing the Last Cost field under Stock Items > Price/Cost Info > Cost Statistics screen

\- Try

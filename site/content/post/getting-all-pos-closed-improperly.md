---
title: Getting all POs Closed Improperly
date: 2018-06-06T07:37:01.419Z
description: Pulling out all POs closed improperly from MAS
image: /img/poclosedimproperly.png
---
<pre><code>

use prod_app

\--GETTING ALL POs HAVING QTYTORCV=0 BUT TOTAL QTYREC < QTYORD

\---GETTING ALL POs CLOSED IMPROPERLY

select distinct tpo.TranID as PONo, tpo.POKey, tim.ItemID, tpl.Description, tpl.POLineKey, tpl.CloseDate, tpr.TranID as ReceiverNo, tpr.BillOfLadingNo, tpr.UpdateDate, tpd.QtyOrd, tpd.QtyInvcd, tpd.QtyRcvd, tpd.QtyOpenToRcv,tpo.Status as POStatus, tpd.Status as LineStatus --2 Closed, 1 Open

from tpoPurchOrder tpo

inner join tpoPOLine tpl

	on tpl.POKey=tpo.POKey

		inner join tpoPOLineDist tpd

		on tpd.POLineKey=tpl.POLineKey

			inner join timItem tim

			on tim.ItemKey=tpl.ItemKey

				inner join tpoReceiver tpr

				on tpr.POKey=tpo.POKey

					inner join tpoRcvrLine trl

					on trl.RcvrKey=tpr.RcvrKey

						inner join tpoRcvrLineDist rld

						on rld.RcvrLineKey=trl.RcvrLineKey	

	where tpo.Status=4 -- PO already CLOSED

	and tpd.QtyOrd > tpd.QtyRcvd

	and tpl.ItemKey=trl.ItemRcvdKey

	and year(tpo.CreateDate)>=2017



\----------------------------------------------------------	

select distinct tpo.TranID as PONo, tpo.POKey, tim.ItemID, tpl.Description, tpl.POLineKey, tpl.CloseDate, tpr.TranID as ReceiverNo, tpr.BillOfLadingNo, tpr.UpdateDate, tpd.QtyOrd, tpd.QtyInvcd, tpd.QtyRcvd, tpd.QtyOpenToRcv, tpo.Status as POStatus, tpd.Status as LineStatus --2 Closed, 1 Open

	from tpoPurchOrder tpo

	inner join tpoPOLine tpl

	on tpl.POKey=tpo.POKey

		inner join tpoPOLineDist tpd

		on tpd.POLineKey=tpl.POLineKey

			inner join timItem tim

			on tim.ItemKey=tpl.ItemKey

				inner join tpoReceiver tpr

				on tpr.POKey=tpo.POKey

					inner join tpoRcvrLine trl

					on trl.RcvrKey=tpr.RcvrKey

						inner join tpoRcvrLineDist rld

						on rld.RcvrLineKey=trl.RcvrLineKey	

	where tpd.QtyOpenToRcv=0 -- wrong OpenQtyToReceive

	and tpd.QtyOrd > tpd.QtyRcvd

	and tpl.ItemKey=trl.ItemRcvdKey

	and year(tpo.CreateDate)>=2017

</code></pre>
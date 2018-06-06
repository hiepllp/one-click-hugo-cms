---
title: Getting all POs Closed Improperly
date: 2018-06-06T07:37:01.419Z
description: Pulling out all POs closed improperly from MAS
image: /img/poclosedimproperly.png
---
<pre><code class="code" id="code-sql" data-clipboard-action="copy" data-clipboard-target="#code-sql">

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

## Solution

PO auto closed while not received in full, fixed for Artem Bannikov

<pre><code class="code" id="code-sql2" data-clipboard-action="copy" data-clipboard-target="#code-sql2">

\--Case 6

select Status, ClosedForInvc, ClosedForRcvg,* from tpoPurchOrder where TranNo like '%6552%'

select Status, ClosedForInvc, ClosedForRcvg,* from tpoPOLine where POKey=6745

select Status, ClosedForInvc, ClosedForRcvg,* from tpoPOLineDist where POLineKey in (select POLineKey from tpoPOLine where POKey=6745)



\--20 should be avail to receive

update tpoPurchOrder set Status=1, ClosedForRcvg=0, ClosedForInvc=0 where POKey=6745

update tpoPOLine set Status=1, ClosedForRcvg=0, ClosedForInvc=0 where POLineKey=12709

update tpoPOLineDist set Status=1, ClosedForRcvg=0, ClosedForInvc=0, QtyOpenToRcv=20 where POLineDistKey=12706

</code></pre>

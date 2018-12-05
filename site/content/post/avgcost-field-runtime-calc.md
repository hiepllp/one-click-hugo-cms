---
title: AvgCost field runtime calc
date: 2018-12-05T10:13:44.399Z
description: AvgCost field runtime calc
---
```
    SELECT CASE 
```

```
		WHEN SUM(INItemStats.QtyOnHand) = 0 
```

```
		THEN SUM(QtyOnHand)
```

```
		ELSE SUM(INItemStats.TotalCost) / SUM(INItemStats.QtyOnHand)
```

```
		END AS AvgCost
```

```
                --SUM(QtyOnHand) AS QtyOnHand,
```

```
                --SUM(LastCost)AS LastCost ,
```

```
                --MinCost ,
```

```
                --MaxCost
```

```
	FROM    dbo.INItemStats
```

```
    INNER JOIN dbo.InventoryItem ON InventoryItem.CompanyID = INItemStats.CompanyID
```

```
    AND InventoryItem.InventoryID = INItemStats.InventoryID
```

```
    WHERE InventoryCD = 'DB60'
```

```
    AND INItemStats.CompanyID = 2;
```

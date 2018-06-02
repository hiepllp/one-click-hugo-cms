---
title: Acumatica note 1
date: 2018-06-02T08:19:57.466Z
description: 'ScreenID, UserID and CompanyID'
---
```
ScreenID (char(8)): PX.Common.PXContext.GetScreenID()
```

```
UserID (guid): PX.Data.PXAccess.GetUserID()
```

```
CompanyID (int): PX.Data.Update.PXInstanceHelper.CurrentCompany
```

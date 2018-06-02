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
<pre><code id="code-js" data-clipboard-target="#code-js">
//js code
function onWindowResized() {
  if (animationAPI) {
    anim.resize();
    animationAPI.recalculateSize();
  }
}

function findCode(pre) {
    var node = pre.firstChild;
    return (node.nodeName == 'CODE' && node.className);
}
 
addEventListener('load', function () {
    Array.prototype.map.call(document.getElementsByTagName('pre');
    filter(Boolean).
    forEach(function (code) {
       hljs.highlightBlock(code);
    });
}, false);
        
</code></pre>
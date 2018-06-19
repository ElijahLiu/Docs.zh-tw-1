---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: 資料繫結滑桿控制項 (VB) |Microsoft 文件
author: wenz
description: 滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 這是可以繫結目前 positio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879136"
---
<a name="databinding-the-slider-control-vb"></a>資料繫結滑桿控制項 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> 滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 您可將繫結至其他 ASP.NET 控制項的滑桿的目前位置。


## <a name="overview"></a>總覽

滑桿控制項在 AJAX Control Toolkit 提供圖形化的滑桿，可以使用滑鼠來控制。 您可將繫結至其他 ASP.NET 控制項的滑桿的目前位置。

## <a name="steps"></a>步驟

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

接下來，加入兩個`TextBox`至頁面的控制項。 其中一個會轉換成圖形化的滑桿，按住不放，另一個將滑桿的位置。

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

下一個步驟已是最後一個步驟。 `SliderExtender`控制項從 ASP.NET AJAX Control Toolkit 滑桿從第一個文字方塊並滑桿位置變更時，自動更新第二個文字方塊。 為了讓，才能執行，`SliderExtender`的`TargetControlID`屬性必須設為第一個文字方塊中; 識別碼`BoundControlID`屬性必須設為第二個文字方塊中的識別碼。

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

您可以看到在瀏覽器中，資料繫結的雙向運作方式： 在文字方塊中輸入新值更新滑桿的位置。 如果您進行第二個唯讀的文字方塊中，您可以加入弱式保護的文字欄位使其具備要手動更新中有值的使用者不容易。


[![滑桿和文字方塊會同步](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

滑桿和文字方塊為同步 ([按一下以檢視完整大小的影像](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](using-the-slider-control-with-auto-postback-vb.md)

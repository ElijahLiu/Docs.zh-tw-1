---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: 建立自訂路由 (VB) |Microsoft 文件
author: microsoft
description: 了解如何將自訂的路由新增至 ASP.NET MVC 應用程式。 在本教學課程中，您會學習如何修改預設的路由表，在 Global.asax 檔案中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872321"
---
<a name="creating-custom-routes-vb"></a>建立自訂路由 (VB)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何將自訂的路由新增至 ASP.NET MVC 應用程式。 在本教學課程中，您會學習如何修改預設的路由表，在 Global.asax 檔案中。


在本教學課程中，您會學習如何將自訂的路由加入至 ASP.NET MVC 應用程式。 您了解如何修改預設的路由表，以自訂路由的 Global.asax 檔中。

在 ASP.NET MVC 應用程式中，預設路由表會正常運作。 不過，您可能會發現您專用的路由需求。 在此情況下，您可以建立自訂的路由。

例如，假設您要建置的部落格應用程式。 您可能想要處理傳入的要求看起來像這樣：

/ 封存/12-25-2009

當使用者輸入這個要求時，您想要的日期傳回對應的部落格項目 2009 年 12 月 25 日。 若要處理這種類型的要求，您需要建立自訂的路由。

Global.asax 檔案中列出的 1 會包含新的自訂路由，名為部落格，看起來像 /Archive/ 哪些控點要求*輸入日期*。

**列出 1-Global.asax （與自訂路由）**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

您將新增至路由表的路由順序很重要。 現有的預設路由之前已加入我們新的自訂部落格路由。 如果您依相反順序排列，然後預設路由一律會呼叫而不是自訂的路由。

自訂的部落格路由會符合開頭/封存/任何要求。 因此，它符合所有下列 Url:

- / 封存/12-25-2009

- / 封存/10-6-2004

- / 封存/apple

自訂路由將內送的要求對應至名為 Archive 的控制站，並叫用 Entry() 動作。 Entry() 方法呼叫時，輸入日期做為參數命名為 entryDate 傳遞。

您可以使用列表 2 中的控制站的部落格的自訂路由。

**列出 2-ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

請注意清單 2 Entry() 方法接受 DateTime 類型的參數。 MVC 架構是聰明，可以從 URL 中將日期轉換成日期時間值時，自動。 如果從 URL 的項目日期參數無法轉換成 DateTime，就會引發錯誤 （請參閱圖 1）。

**圖 1： 將參數轉換的錯誤**


[![新增專案 對話方塊](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**圖 01**： 將參數轉換的錯誤 ([按一下以檢視完整大小的影像](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>總結

本教學課程的目標是為了示範如何建立自訂的路由。 您已學習如何將自訂的路由加入至 Global.asax 檔案代表部落格文章中的路由表。 我們將討論如何將要求的部落格項目對應至名為 ArchiveController 控制器和名為 Entry() 控制器動作。

> [!div class="step-by-step"]
> [上一頁](asp-net-mvc-controller-overview-vb.md)
> [下一頁](creating-a-route-constraint-vb.md)

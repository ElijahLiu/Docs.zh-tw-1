---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: 檢查詳細資料與刪除方法 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 00f7e5d6679f1bd8875931e601c8151049f785ac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868772"
---
<a name="examining-the-details-and-delete-methods"></a>檢查的詳細資料和 Delete 方法
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > 本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。 它更安全、 容易遵循，及示範更多的功能。


在本教學課程的這個部分，您將會檢查自動產生`Details`和`Delete`方法。

## <a name="examining-the-details-and-delete-methods"></a>檢查的詳細資料和 Delete 方法

開啟`Movie`控制站，並檢查`Details`方法。

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

MVC scaffolding 引擎，來建立此動作方法中加入註解顯示叫用方法的 HTTP 要求。 在此情況下它是`GET`具有三個的 URL 區段的要求`Movies`控制站，`Details`方法和`ID`值。

程式碼第一次輕鬆地搜尋資料使用`Find`方法。 建置至方法的重要安全性功能是，程式碼驗證`Find`方法發現電影，這個程式碼嘗試執行與它的任何項目之前。 比方說，駭客可能會導致發生錯誤的站台變更建立者 」 從連結的 URL`http://localhost:xxxx/Movies/Details/1`為類似`http://localhost:xxxx/Movies/Details/12345`（或其他值並不代表實際的影片）。 如果您不會檢查 null 的影片，null 電影會導致資料庫錯誤。

檢查 `Delete` 和 `DeleteConfirmed` 方法。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

請注意，`HTTP Get``Delete`方法並不會刪除指定的影片，它會傳回電影的檢視，您可以提交 (`HttpPost`) 刪除... 如果您執行刪除作業以回應 GET 要求 (或是執行相關編輯作業、建立作業或任何會變更資料的其他作業)，則會造成安全性漏洞。 如需詳細資訊，請參閱 Stephen Walther 部落格文章： [ASP.NET MVC 秘訣 #46-請勿使用刪除連結，因為它們會產生安全性漏洞](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)。

我們將可刪除資料的 `HttpPost` 方法命名為 `DeleteConfirmed`，以提供 HTTP POST 方法的唯一簽章或名稱。 這兩個方法簽章如下所示：

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

通用語言執行平台 (CLR) 需要多載方法，以提供唯一的參數簽章 (方法名稱相同但參數清單不同)。 不過，您需要在這裡兩種刪除方法-一個用於 GET-，另一個用於 POST 都具有相同的參數簽章。 (它們都需要接受單一整數作為參數)。

若要排序時，您可以執行兩件事。 其中一個是讓方法不同的名稱。 這是 scaffolding 機制在上述範例採取的方法。 不過，這麼做會導致一個小問題：ASP.NET 會依名稱將 URL 區段與動作方法對應，一旦您重新命名方法，路由通常就會找不到這個方法。 解決辦法正如您看到的這個範例：將 `ActionName("Delete")` 屬性新增至 `DeleteConfirmed` 方法。 這會有效地執行對應的路由系統，讓 URL 包含<em>/Delete/</em>POST 要求會發現`DeleteConfirmed`方法。

若要避免具有相同名稱和簽章的方法有問題的另一個常見方法是人工方式變更為包含未使用的參數 POST 方法的簽章。 比方說，有些開發人員將參數類型`FormCollection`傳遞至 POST 方法，然後只要不使用參數：

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>總結

現在，您會有完整的 ASP.NET MVC 應用程式將資料儲存在本機的 DB 資料庫中。 您可以建立、 讀取、 更新、 刪除及搜尋電影。

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>後續步驟

您已經建置及測試 web 應用程式之後下, 一個步驟是將它提供給其他人透過網際網路使用。 若要這樣做，您必須將它部署至 web 主控提供者。 Microsoft 提供的免費 web 裝載中的最多 10 個 web sites[免費試用版的 Windows Azure 帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 我建議您接著執行我的教學課程[將成員資格、 OAuth、 與 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 絕佳的教學課程是 Tom Dykstra 中繼層級[建立 ASP.NET MVC 應用程式的 Entity Framework 資料模型](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 [Stackoverflow](http://stackoverflow.com/help)和[ASP.NET MVC 論壇](https://forums.asp.net/1146.aspx)會很大的位置，以詢問的問題。 請遵循[我](https://twitter.com/RickAndMSFT)因此我最新的教學課程中的更新可能會發生在 twitter 上。

意見反應是 「 歡迎畫面。

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [上一步](adding-validation-to-the-model.md)

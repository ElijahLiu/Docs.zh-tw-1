---
title: 新增搜尋
author: rick-anderson
description: 示範如何將搜尋新增至簡易的 ASP.NET Core MVC 應用程式
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 71e6074035e7c66fed40673d19c241bfcc585c18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

注意：SQLlite 有區分大小寫，因此您必須搜尋 "Ghost" 而不是 "ghost"。

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

變更 *Views\movie\Index.cshtml* Razor 檢視中的 `<form>` 標記，以指定 `method="get"`：

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [上一步 - 控制器方法和檢視](controller-methods-views.md)
> [下一步 - 新增欄位](new-field.md)  

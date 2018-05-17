---
title: 開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 了解建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。 對於 ASP.NET Core 中的 Web 工作負載，建議使用 Razor 頁面。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: caf4376c0a02931eeec85e5067a082b37ef9da68
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>開始使用 ASP.NET Core 中的 Razor Pages

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。 Razor Pages 是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。

本教學課程有 3 個版本：

* Windows：本教學課程
* macOS：[Razor 頁面與 Visual Studio for Mac 使用者入門](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS、Linux 和 Windows：[Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門](xref:tutorials/razor-pages-vsc/razor-pages-start)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **RazorPagesMovie**。 請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。
  ![新增 ASP.NET Core Web 應用程式](../../mvc/razor-pages/index/_static/np.png)
* 在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

Visual Studio 範本會建立入門專案：

![底下提供說明，包括方案總管](razor-pages-start/_static/se.png)

按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。

![Home 或 Index 頁面](razor-pages-start/_static/home.png)

* Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 在上述影像中，連接埠編號為 5000。 當您執行應用程式時，會看到不同的連接埠編號。
* 使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [下一步：新增模型](xref:tutorials/razor-pages/model)

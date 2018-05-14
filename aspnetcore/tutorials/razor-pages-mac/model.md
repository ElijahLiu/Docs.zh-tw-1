---
title: 使用 Visual Studio for Mac 將模型新增至 ASP.NET Core Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Visual Studio for Mac 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 97bc9f14b8d6da958a7f587e54a37d2d0e0aabd4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>使用 Visual Studio for Mac 將模型新增至 ASP.NET Core Razor 頁面應用程式

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>新增資料模型

* 在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。 將資料夾命名為 *Models*。
* 以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案]。
* 在 [新增檔案] 對話方塊中：

  * 在左窗格中選取 [一般]。
  * 在中央窗格中選取 [空類別]。
  * 將類別命名為 **Movie**，並選取 [新增]。

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

以滑鼠右鍵按一下紅色書寫行，例如 `services.AddDbContext<MovieContext>(options =>` 這行中的 `MovieContext`。 選取 [快速檢修] > [using RazorPagesMovie.Models;]。 Visual Studio 會新增至 using 陳述式。

請建置專案，以確認您沒有任何錯誤。

![建立頁面](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>用於移轉的 Entity Framework Core NuGet 套件

[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。 按一下 [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 連結以取得要使用的版本號碼。 若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合。 **注意：**您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。

若要編輯 *.csproj* 檔案：

* 選取 [檔案] > [開啟]，然後選取 *.csproj* 檔案。
* 選取 [選項]。
* 將 [開啟方式] 變更為 [原始程式碼編輯器]。

![編輯 csproj 檔案](model/csproj.png)

將 `Microsoft.EntityFrameworkCore.Tools.DotNet` 工具參考新增到第二個 **\<ItemGroup >**：

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

顯示於下方程式碼中的版本號碼，在寫入時為正確。

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>將 Pages/Movies 檔案新增至專案

* 在 Visual Studio 中，以滑鼠右鍵按一下 *Pages* 資料夾，然後選取 [新增] > [新增現有資料夾]。
* 選取 *Movies* 資料夾。
* 在 [選擇要內含在此專案中的檔案] 對話方塊中，選取 [全部包含]。

下一個教學課程說明 Scaffolding 所建立的檔案。

> [!div class="step-by-step"]
> [上一步：開始使用](xref:tutorials/razor-pages-mac/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages-mac/page)

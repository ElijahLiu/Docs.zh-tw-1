---
title: 裝載在 Windows 服務的 ASP.NET Core
author: rick-anderson
description: 了解如何裝載在 Windows 服務的 ASP.NET Core 應用程式。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>裝載在 Windows 服務的 ASP.NET Core

作者：[Tom Dykstra](https://github.com/tdykstra)

若要在 Windows 上裝載 ASP.NET Core 應用程式而不使用 IIS， 建議在 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications) 中執行應用程式。 當裝載為 Windows 服務，應用程式可以在重新開機及當機之後自動啟動，而不需要人工介入。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([如何下載](xref:tutorials/index#how-to-download-a-sample))。 如需如何執行範例應用程式的指示，請參閱 「 範例的*README.md*檔案。

## <a name="prerequisites"></a>必要條件

* 應用程式必須執行.NET Framework 執行階段。 在 *.csproj*檔案中，指定適當的值[TargetFramework](/nuget/schema/target-frameworks)和[RuntimeIdentifier](/dotnet/articles/core/rid-catalog)。 以下為範例：

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  使用 Visual Studio 中建立專案時**ASP.NET Core 應用程式 (.NET Framework)** 範本。

* 如果應用程式需要從網際網路 （不只是從內部網路）接收要求，則必須使用[HTTP.sys](xref:fundamentals/servers/httpsys)網頁伺服器 (在 ASP.NET Core 1.x 應用程式稱為[WebListener](xref:fundamentals/servers/weblistener)) 而不是[Kestrel](xref:fundamentals/servers/kestrel)。 IIS 建議是作為反向 proxy 伺服器使用 Kestrel 邊緣部署。 如需詳細資訊，請參閱[何時搭配使用 Kestrel 與反向 Proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)。

## <a name="get-started"></a>開始使用

本節說明將現有的 ASP.NET Core 專案設定為以服務執行所需的最小變更。

1. 安裝 NuGet 套件[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)。

2. 在變更下列`Program.Main`:

   * 呼叫`host.RunAsService`而不是`host.Run`。

   * 如果程式碼會呼叫`UseContentRoot`，發行位置，而不是使用路徑`Directory.GetCurrentDirectory()`。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. 發行應用程式的資料夾。 使用[dotnet 發行](/dotnet/articles/core/tools/dotnet-publish)或[Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)所發行的資料夾。

4. 測試建立和啟動服務。

   若要使用的系統管理權限開啟命令殼層[sc.exe](https://technet.microsoft.com/library/bb490995)命令列工具來建立及啟動服務。 如果服務為 MyService，發行至`c:\svc`，及名為 AspNetCoreService，命令：

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   `binPath`值是應用程式的可執行檔，其中包含可執行檔名稱的路徑。

   ![主控台視窗建立及啟動範例](windows-service/_static/create-start.png)

   當命令完成時，瀏覽至與相同的路徑做為主控台應用程式執行時 (根據預設， `http://localhost:5000`):

   ![在服務中執行](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>提供服務外部執行的方式

更輕鬆地測試和偵錯時執行外部服務，所以可將呼叫的程式碼加入`RunAsService`只在某些情況下。 例如，應用程式可以執行為主控台應用程式與`--console`命令列引數，或是如果附加偵錯工具：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>停止和啟動的事件處理

若要處理`OnStarting`， `OnStarted`，和`OnStopping`事件，進行下列的其他變更：

1. 建立衍生自類別`WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. 建立擴充方法給`IWebHost`傳遞之任何自訂`WebHostService`至`ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. 在`Program.Main`，呼叫新的擴充方法， `RunAsCustomService`，而不是`RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

如果自訂`WebHostService`程式碼需要從相依性插入 （例如記錄器） 服務，取得從`Services`屬性`IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

從網際網路或公司網路的要求和位於 proxy 後方或與其互動的負載平衡器的服務可能需要其他設定。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

## <a name="acknowledgments"></a>感謝

撰寫本文時的協助的已發行的來源：

* [為 Windows 服務裝載的 ASP.NET Core](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [如何在裝載您的 ASP.NET 核心 Windows 服務中](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)

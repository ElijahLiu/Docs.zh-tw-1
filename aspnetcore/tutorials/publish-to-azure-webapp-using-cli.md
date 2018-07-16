---
title: 使用命令列工具將 ASP.NET Core 應用程式發行到 Azure
author: camsoper
description: 了解如何使用 Git 命令列用戶端將 ASP.NET Core 應用程式發行到 Azure App Service。
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126956"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>使用命令列工具將 ASP.NET Core 應用程式發行到 Azure

作者 [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

本教學課程將說明如何使用命令列工具來建置 ASP.NET Core 應用程式，並將其部署至 Microsoft Azure App Service。 完成之後，您將會擁有建置在 ASP.NET Core 中且裝載為 Azure App Service Web 應用程式的 Razor Pages Web 應用程式。 本教學課程是使用 Windows 命令列工具來撰寫，但也適用於 macOS 和 Linux 環境。

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 使用 Azure CLI 來建立 Azure App Service 網站
> * 使用 Git 命令列工具將 ASP.NET Core 應用程式部署至 Azure App Service

## <a name="prerequisites"></a>必要條件

若要完成此課程，您會需要：

* [Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Git](https://www.git-scm.com/) 命令列用戶端

## <a name="create-a-web-app"></a>建立 Web 應用程式

為 Web 應用程式建立新的目錄、建立新的 ASP.NET Core Razor Pages 應用程式，然後在本機執行該網站。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[其他](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![命令列輸出](publish-to-azure-webapp-using-cli/_static/new_prj.png)

瀏覽至 `http://localhost:5000` 來測試應用程式。

![網站在本機上執行](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a>建立 Azure App Service 執行個體

使用 [Azure Cloud Shell](/azure/cloud-shell/quickstart)，建立資源群組、App Service 方案，以及 App Service Web 應用程式。

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

進行部署之前，使用以下命令來設定帳戶層級的部署認證：

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

使用 Git 部署應用程式需要部署 URL。 擷取如下的 URL。

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

記下所顯示結尾是 `.git` 的 URL。 在下一步中會使用它。

## <a name="deploy-the-app-using-git"></a>使用 Git 部署應用程式

您現在可以使用 Git 從本機電腦進行部署。

> [!NOTE]
> 您可以放心地忽略來自 Git 有關行尾結束符號的任何警告。

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[其他](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

Git 會提示您輸入先前設定的部署認證。 驗證之後，應用程式就會被推送到遠端位置，然後進行建置及部署。

![Git 部署輸出](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a>測試應用程式

瀏覽至 `https://<web app name>.azurewebsites.net` 來測試應用程式。 若要顯示在 Cloud Shell (或 Azure CLI) 中的位址，請使用：

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![應用程式在 Azure 中執行](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>清除

完成測試應用程式及檢查程式碼和資源之後，請刪除資源群組，藉以刪除 Web 應用程式和方案。

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 使用 Azure CLI 來建立 Azure App Service 網站
> * 使用 Git 命令列工具將 ASP.NET Core 應用程式部署至 Azure App Service

接下來，學習使用命令列來部署使用 CosmosDB 的現有 Web 應用程式。

> [!div class="nextstepaction"]
> [使用 .NET Core 從命令列部署至 Azure](/dotnet/azure/dotnet-quickstart-xplat)

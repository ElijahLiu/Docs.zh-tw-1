---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: 疑難排解封裝程序 |Microsoft 文件
author: jrjlee
description: 本主題描述如何使用 EnablePackageProcessLoggingAndAssert 屬性 M 中可以收集在封裝程序的詳細的資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892682"
---
<a name="troubleshooting-the-packaging-process"></a>疑難排解封裝程序
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何收集封裝程序的詳細的資訊，以及在使用**EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) 中的屬性。
> 
> 當您將**EnablePackageProcessLoggingAndAssert**屬性**true**，MSBuild 將會：
> 
> - 將封裝程序的其他資訊加入至組建記錄檔。
> - 記錄錯誤，某些狀況下，比方說，如果封裝清單中找到重複的檔案。
> - 建立的記錄檔目錄*ProjectName*\_封裝的資料夾，並使用它來記錄您要封裝之檔案的相關資訊。
> 
> 如果無法在封裝程序，或您的 web 部署封裝不包含您預期的檔案，您可以使用此資訊來疑難排解程序，而且 pinpoint 將要錯誤項目。
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert**屬性只有在您建置的專案使用**偵錯**組態。 在其他組態中會忽略此屬性。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>了解 EnablePackageProcessLoggingAndAssert 屬性

[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)描述 Web 發行管線 (WPP) 如何提供一組擴充功能的 MSBuild 並啟用整合與網際網路資訊服務 (IIS) Web MSBuild 目標部署工具 (Web Deploy)。 當您封裝的 web 應用程式專案時，您要叫用 WPP 目標。

許多這些 WPP 目標加入額外的資訊記錄的條件式邏輯時**EnablePackageProcessLoggingAndAssert**屬性設定為**true**。 例如，如果您檢閱**封裝**目標，您可以看到它會建立額外的記錄檔目錄，以及如果寫入文字檔案的檔案清單**EnablePackageProcessLoggingAndAssert**等於**true**。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> 中所定義的 WPP 目標*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。 您可以開啟這個檔案，並檢閱 Visual Studio 2010 或任何 XML 編輯器中的目標。 請注意不要修改檔案的內容。


## <a name="enabling-the-additional-logging"></a>啟用額外的記錄功能

您可以提供的值**EnablePackageProcessLoggingAndAssert**以各種方式，根據您如何建置您的專案屬性。

如果您建置您的專案，從命令列，您可以提供的值**EnablePackageProcessLoggingAndAssert**做為命令列引數的屬性：


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


如果您使用自訂的專案檔案以建置專案，您可以包含**EnablePackageProcessLoggingAndAssert**值**屬性**屬性**MSBuild**工作：


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


如果您使用 Team Foundation Server (TFS) 組建定義來建置您的專案，您可以提供的值**EnablePackageProcessLoggingAndAssert**屬性**MSBuild 引數**資料列：![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> 如需有關建立及設定組建定義的詳細資訊，請參閱[建立組建定義，支援部署](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md)。


或者，如果您想要在每次建置中包含的封裝，您可以修改專案檔來設定 web 應用程式專案**EnablePackageProcessLoggingAndAssert**屬性**true**。 您應該將屬性加入至第一個**PropertyGroup**在您的.csproj 或.vbproj 檔案中的項目。


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>檢閱記錄檔

當您建置並封裝使用的 web 應用程式專案**EnablePackageProcessLoggingAndAssert**設**true**，MSBuild 會建立名為記錄檔中的其他資料夾*ProjectName*\_套件資料夾。 記錄檔資料夾包含各種不同的檔案：

![](troubleshooting-the-packaging-process/_static/image2.png)

您看到的檔案清單會根據您的專案和您的建置流程中的項目而有所不同。 不過，這些檔案通常用來記錄的封裝，在程序的各個階段 WPP 正在收集的檔案清單：

- *PreExcludePipelineCollectFilesPhaseFileList.txt*檔案列出檔，MSBuild 會收集封裝之前指定排除的任何檔案會遭到移除。
- *AfterExcludeFilesFilesList.txt*檔案包含已修改的檔案清單之後指定排除的任何檔案會遭到移除。

    > [!NOTE]
    > 如需有關檔案和資料夾排除在封裝程序的詳細資訊，請參閱[排除檔案和資料夾從部署](excluding-files-and-folders-from-deployment.md)。
- *AfterTransformWebConfig.txt*檔會列出針對封裝之後收集的檔案*Web.config*已執行轉換。 在此清單中，任何特定組態*Web.config*轉換檔案，例如*Web.Debug.config*和*Web.Release.config*，會排除的檔案清單封裝。 轉換單一*Web.config*隨附於其位置。
- *PostAutoParameterizationWebConfigConnectionStrings.txt*檔案包含的檔案清單中的連接字串之後*Web.config*已參數化檔案。 這是程序，可讓您部署封裝時將連接字串取代您的目標環境的正確設定。
- *Prepackage.txt*檔案包含最終的建置前要包含在封裝中的檔案清單。

> [!NOTE]
> 額外的記錄檔的名稱通常會對應到 WPP 目標。 您可以檢閱這些目標，藉由檢查*Microsoft.Web.Publishing.targets* %programfiles (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web 資料夾中的檔案。


如果 web 封裝的內容不是您所預期，請檢閱這些檔案可以是實用的方式，來識別在何時處理程序中的發生錯誤的原因。

## <a name="conclusion"></a>結論

本主題描述如何使用**EnablePackageProcessLoggingAndAssert** MSBuild 疑難排解封裝程序中的屬性。 它說明不同的方式，您可以在其中提供要建置處理序的屬性值和它所述的其他資訊，您將屬性設定為時，會記錄**true**。

## <a name="further-reading"></a>進一步閱讀

如需使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。 如需有關 WPP 及如何管理封裝程序的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。 如需如何從 web 部署套件中排除特定檔案和資料夾的指引，請參閱[排除檔案和資料夾從部署](excluding-files-and-folders-from-deployment.md)。

> [!div class="step-by-step"]
> [上一步](running-windows-powershell-scripts-from-msbuild-project-files.md)

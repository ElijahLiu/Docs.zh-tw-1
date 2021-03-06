---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: "使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 12 個 6-設定資料夾權限 |Microsoft 文件"
author: tdykstra
description: "這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 42085fff5f1aed1440f49e1e2ceee0cf0e751e2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 12 個 6-設定資料夾權限
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>概觀

在本教學課程中，您可以設定的資料夾權限*Elmah*資料夾中已部署的 web 站台，好讓應用程式可以在該資料夾中建立記錄檔。

當您在 Visual Studio 中使用 Visual Studio 程式開發伺服器 (Cassini) 測試 web 應用程式時，您的身分執行應用程式。 您最有可能是開發電腦上系統管理員，並具有完整權限執行的任何資料夾中的任何檔案的任何動作。 但在 IIS 底下執行應用程式，它會在下執行應用程式集區，以指派的站台所定義的識別。 這通常是有限的權限的系統定義的帳戶。 依預設它具有讀取和執行權限，您的 web 應用程式檔案和資料夾，但沒有寫入權限。

如果您的應用程式會建立或更新檔案是一般需要 web 應用程式中，這會造成問題。 Contoso 大學應用程式、 Elmah 建立 XML 檔案中的*Elmah*資料夾，才能儲存有關錯誤的詳細資料。 即使您不使用 Elmah 類似，您的網站可能讓使用者將檔案上傳，或執行其他工作的資料寫入網站中的資料夾。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="testing-error-logging-and-reporting"></a>測試記錄和報告的錯誤

若要查看如何在應用程式無法正常運作在 IIS 中 （雖然您在 Visual Studio 中進行測試時一樣），您可能會造成錯誤，通常會記錄的 Elmah，，然後開啟 Elmah 錯誤記錄檔，以查看詳細資料。 如果 Elmah 無法建立 XML 檔案和存放區錯誤詳細資料，您會看到空白的錯誤報告。

開啟瀏覽器並移至`http://localhost/ContosoUniversity`，然後要求無效的 URL，例如*Studentsxxx.aspx*。 您會看到而不是系統產生的錯誤頁面*GenericErrorPage.aspx*頁面上，因為`customErrors`Web.config 檔案中的設定為"RemoteOnly"，而您在本機執行 IIS:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

現在執行*Elmah.axd*以查看錯誤報告。 因為 Elmah 無法建立 XML 檔案中的，您會看到空白錯誤記錄檔頁面*Elmah*資料夾：

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>設定 Elmah 資料夾的寫入權限

您可以手動設定資料夾權限，或者您可以將它自動在部署程序。 進行自動需要複雜 MSBuild 的程式碼，而且由於您只需要執行這個動作在第一次部署，本教學課程只會顯示如何以手動方式執行操作。 (如需如何讓此組件部署程序的資訊，請參閱[設定資料夾的權限上 Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi 部落格上。)

在**Windows 檔案總管**，瀏覽至*C:\inetpub\wwwroot\ContosoUniversity*。 以滑鼠右鍵按一下*Elmah*資料夾中，選取**屬性**，然後選取**安全性** 索引標籤。

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(如果您沒有看到**DefaultAppPool**中**群組或使用者名稱** 清單中，您可能用於本教學課程中所指定的其他方法來在您的電腦上設定 IIS 和 ASP.NET 4。 在此情況下，找出應用程式集區指派給 Contoso 大學應用程式和授與寫入權限給該識別使用何種身分識別。 請參閱有關應用程式集區識別連結在本教學課程結尾處）。

按一下 [編輯] 。 在**Elmah 的權限**對話方塊中，選取**DefaultAppPool**，然後選取**寫入**中核取方塊**允許**資料行。

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

按一下**確定**在兩個對話方塊。

## <a name="retesting-error-logging-and-reporting"></a>測試記錄和報告的錯誤

導致錯誤發生一次相同的方式 （不正確的 URL 要求） 來測試及執行**錯誤記錄檔**頁面。 目前頁面上，會出現錯誤。

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

您也需要寫入權限*應用程式\_資料*資料夾，因為您在該資料夾中，SQL Server Compact 資料庫檔案，而且您想要能夠在這些資料庫中更新資料。 在此情況下，不過，您不需要執行任何額外的動作，因為部署程序會自動設定寫入權限上*應用程式\_資料*資料夾。

您現在已完成的所有工作所需 Contoso 大學在本機電腦上 IIS 中運作正常。 在下一個教學課程中，您就會將它部署至主控提供者的公開可用的站台。

## <a name="more-information"></a>更多資訊

在此範例中，為什麼 Elmah 無法儲存記錄檔的原因，是很明顯。 您可以使用 IIS 追蹤問題的原因不那麼顯而易見。請參閱[疑難排解失敗的要求使用的追蹤在 IIS 7 中](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net 網站上。

如需如何授與應用程式集區識別的權限的詳細資訊，請參閱[應用程式集區識別](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)和[安全的內容在檔案系統 Acl 透過 IIS](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.net 網站上。

>[!div class="step-by-step"]
[上一頁](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
[下一頁](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)

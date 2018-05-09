---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: 開發與 Azure Active Directory 的 ASP.NET 應用程式 |Microsoft 文件
author: Rick-Anderson
description: Azure Active Directory 的 Microsoft ASP.NET tools 可讓您更簡單，好讓在 Azure 上裝載的 web 應用程式的驗證。 您可以使用 Azure 驗證...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 9b2dc05089126fd5f4c1b0a0bd85b8a39f3041dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>使用 Azure Active Directory 開發的 ASP.NET 應用程式
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

Microsoft ASP.NET 工具的 Azure Active Directory 可簡化上裝載的 web 應用程式啟用驗證[Azure](https://www.windowsazure.com/home/features/web-sites/)。 您可以使用 Azure 驗證來驗證 Office 365 使用者，從您的組織，從您在內部部署 Active Directory 同步處理的公司帳戶或建立您自己自訂的 Azure Active Directory 網域中的使用者。 啟用 Windows Azure 驗證設定您的應用程式驗證使用者使用單一[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)租用戶。

本教學課程會示範如何建立 ASP.NET 應用程式設定為使用登入[Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD)。 您也將學習如何呼叫 Graph API 來取得目前登入使用者的相關資訊及如何部署 Azure 應用程式。

## <a name="prerequisites"></a>必要條件

1. [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)或[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -3 或更高的更新為必要項。
3. Azure 帳戶。 [按一下這裡](https://azure.microsoft.com/pricing/free-trial/)免費試用，如果您沒有帳戶。

## <a name="add-a-global-administrator-to-your-active-directory"></a>加入您的 Active Directory 的全域管理員

1. 登入[Azure 管理入口網站](https://manage.windowsazure.com/)。
2. 包含所有的 Azure 帳戶**預設目錄**-按一下它，然後按一下 [**使用者**頂端的頁面] 索引標籤 （請參閱下面的影像）。
3. 按一下 新增使用者。  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. 建立新的使用者與**全域管理員**角色。 按一下**使用者**從頂端功能表中，然後按一下**新增使用者**命令列上的按鈕。
5. 在**新增使用者** 對話方塊中，輸入新使用者的名稱，然後按一下向右箭號。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. 輸入使用者名稱和角色設定為**全域管理員**。 全域管理員需要替代電子郵件地址進行復原密碼。 在完成之後，按一下向右箭號。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. 在對話方塊的 下一步 頁面上，按一下 **建立**。 將建立新的使用者暫時密碼，並將其顯示在對話方塊中。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   儲存密碼，您必須變更密碼之後的第一個記錄檔中。 下圖顯示新的系統管理員帳戶。 您必須使用 Azure Active Directory 登入您的應用程式，而不是 Microsoft 帳戶也會顯示此頁面上。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>建立 ASP.NET 應用程式

下列步驟會使用[Visual Studio Express 2013 for Web](https://www.microsoft.com/download/details.aspx?id=40747)，而且需要[Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721)。

1. 在 Visual Studio 中，按一下 **檔案**然後**新專案**。 在**新專案**對話方塊中，選取 Visual C# Web 專案從左窗格中，按一下**確定**。 您也可以取消核取**加入 Application Insights 加入專案**如果您不想為您的應用程式的功能。
2. 在**新增 ASP.NET 專案**對話方塊中，選取**MVC**，然後按一下 **變更驗證**。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. 在**變更驗證**對話方塊中，選取**組織帳戶**。 這些選項可用來自動向 Azure AD 中註冊您的應用程式，以及自動設定您的應用程式與 Azure AD 整合。 您不必使用**變更驗證**對話方塊來註冊及設定您的應用程式，但它可以更容易。 如果您使用 Visual Studio 2012 比方說，您就可以仍然可以手動在 Azure 管理入口網站中註冊應用程式，並更新其設定以使用 Azure AD 整合。  
   在下拉式功能表中，選取**雲端-單一組織**和**單一登入，讀取目錄資料**。 為您的 Azure AD 目錄，例如 （在下列映像） 輸入的網域*aricka0yahoo.onmicrosoft.com*，然後按一下 **確定**。 您可以從 [網域] 索引標籤預設目錄，在 azure 入口網站來取得網域名稱 （請參閱下圖中，向下）。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   下圖顯示從 Azure 入口網站的網域名稱。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > 您可以選擇性地設定會依序按一下 Azure AD 中註冊應用程式識別碼 URI**更多選項**。 應用程式識別碼 URI 是應用程式，其 Azure AD 中註冊，並由應用程式用來與 Azure AD 進行通訊時識別本身的唯一識別碼。 如需應用程式識別碼 URI 和已註冊的應用程式的其他屬性的詳細資訊，請參閱[本主題](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering)。 按一下 [應用程式識別碼 URI] 欄位下方的核取方塊，您也可以選擇使用相同的應用程式識別碼 URI 的 Azure AD 中覆寫現有的註冊。
4. 按一下後**確定**、 登入 對話方塊隨即出現，以及您要使用的全域管理員帳戶 （不與您訂用帳戶相關聯的 Microsoft 帳戶） 登入。 如果您先前建立新的系統管理員帳戶，您將必須變更密碼，然後登入一次使用新的密碼。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. 您已成功通過驗證之後，**新增 ASP.NET 專案**對話方塊會顯示您的驗證選項 (**組織**) 並將新的應用程式的目錄註冊 (*aricka0yahoo.onmicrosoft.com*在下圖)。 以下這項資訊，請選取標示的核取方塊**雲端中的主機**。 如果選取此核取方塊，則專案會佈建為 Azure web 應用程式，並將簡單發行更新版本的啟用。 按一下 [確定 **Deploying Office Solutions**]。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **設定 Azure 網站**對話方塊隨即出現，使用系統自動產生站台名稱和區域。 也請注意您目前登入對話方塊中的帳戶。 您想要確定此帳戶是一個 Azure 訂用帳戶附加到，通常是 Microsoft 帳戶。

    > [!NOTE]
    > 這個專案需要資料庫。 您必須選取其中一個現有的資料庫，或是另外新建一個。 需要資料庫，因為專案已經使用本機資料庫檔案來儲存少量驗證組態資料。 當您部署至 Azure 網站應用程式時，此資料庫未隨附部署，因此您必須選擇一個可存取位於雲端。 按一下 [確定 **Deploying Office Solutions**]。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. 將建立專案，和您的驗證選項和 web 應用程式選項將會自動設定與專案。 此程序完成後，請執行專案在本機按 **^ F5**。 您必須使用您的組織帳戶登入。 提供您稍早建立的帳戶使用者名稱和密碼，然後按一下**登入**。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. 之後成功登入，ASP.NET 網站會顯示您藉由顯示在頁面的右上角的使用者名稱驗證。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   如果您收到錯誤：  
   值不可為 null 或空白。 參數名稱： Externallink>   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   請參閱[偵錯](#dbg)教學課程的最後一節。

## <a name="basics-of-the-graph-api"></a>Graph API 的基本概念

[Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx)是用來執行 CRUD 和其他物件上的作業，您的 Azure AD 目錄中的程式設計介面。 如果您選取驗證的組織帳戶選項，在 Visual Studio 2013 中建立新的專案時，您的應用程式將已設定為呼叫 Graph API。 本節簡短說明 Graph API 的運作方式。

1. 在執行的應用程式中按一下頂端的登入的使用者名稱頁面的權限。 這會帶您前往 使用者設定檔頁面上，這可能會在首頁控制器的動作。 您會發現資料表包含您稍早建立的系統管理員帳戶相關的使用者資訊。 這項資訊會儲存在您的目錄，並會呼叫 Graph API 來載入頁面時，擷取這項資訊。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. 返回 Visual Studio，並展開**控制器**資料夾，然後開啟**HomeController.cs**檔案。 您會看到**UserProfile()** 包含擷取權杖，然後再呼叫 Graph API 的程式碼的動作。 此程式碼會複製如下： 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    若要呼叫 Graph API，您需要擷取語彙基元。 擷取語彙基元時，它的字串值必須附加的 Graph API 的所有後續要求的 Authorization 標頭。 上方的程式碼的大部分處理向 Azure AD 取得權杖，使用權杖來呼叫 Graph API，以與然後轉換回應，讓它可以顯示在 檢視詳細的資料。

    討論的最相關的部分會反白顯示的下行： `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`。 這一行表示已經還原序列化來自 JSON 回應，並顯示在檢視中的使用者名稱。

    您可以呼叫 Graph API 使用 HttpClient 和處理未經處理資料，但更簡單的方法是使用[圖形用戶端程式庫可透過 NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)。 用戶端程式庫會處理未經處理的 HTTP 要求和所傳回資料的轉換，並讓您更容易使用 Graph API 在.NET 環境中運作。 請參閱上相關的 Graph API 的程式碼範例[GitHub。](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>部署至 Azure 應用程式

下列步驟將說明如何部署 Azure 應用程式。 在先前步驟中，您新的專案以連接 web 應用程式在 Azure 上，讓它準備好可以發佈幾個步驟中。

1. 在 Visual Studio 中，以滑鼠右鍵按一下專案，然後選取**發行**。 **發行 Web**對話方塊會顯示每個設定。 按一下**下一步**按鈕移至**設定**頁面。 您可能會提示您進行驗證。請確定您使用您的 Azure 訂用帳戶 （通常是 Microsoft 帳戶） 和不您稍早建立的組織帳戶進行驗證。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. 請檢查**啟用組織驗證**選項。 在**網域**欄位中，輸入您的目錄的網域。 從**存取層級**下拉式清單中，選取**單一登入，讀取目錄資料**。 您會注意到您所使用的先前資料庫中便已自行填入**資料庫**> 一節。 按一下 [發行] 。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio 會開始部署您的網站，則會出現新的瀏覽器視窗。 您可能會提示您再次向您的目錄。 您已驗證之後，您將會重新導向至新發行的網站在 Azure 上。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>偵錯應用程式

如果您收到下列錯誤：   
 值不可為 null 或空白。 參數名稱： Externallink>   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

中的程式碼取代 *_layout.cshtml\\_LoginPartial.cshtml*以下列檔案：

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

之後執行應用程式時，如果登入的使用者顯示 「 Null 使用者 」，請先登出，並以登入您稍早建立的 Active Directory 帳戶。

絕佳教學課程，請遵循是 Rick Rainey[深入探討： Azure 網站和組織使用 Azure AD 的驗證](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)。

## <a name="more-information"></a>更多資訊

- [深入探討： Azure 網站和組織使用 Azure AD 的驗證](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Azure AD Graph API 概觀](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [在 Azure AD 中的驗證案例](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [GitHub 上的 azure AD 程式碼範例](https://github.com/AzureADSamples)

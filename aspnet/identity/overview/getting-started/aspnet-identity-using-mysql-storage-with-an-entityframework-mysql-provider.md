---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: EntityFramework MySQL 提供者 (C#) 搭配使用 MySQL 的儲存體 |Microsoft 文件'
author: maumar
description: 本教學課程會示範如何以取代 MySQL 能 EntityFramework （SQL 用戶端提供者） 與 ASP.NET 識別的預設資料儲存機制...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873387"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET 識別： 使用 MySQL 儲存體使用 EntityFramework MySQL 提供者 (C#)
====================
由[Maurycy Markowski](https://github.com/maumar)， [Raquel Soares De Almeida](https://github.com/raquelsa)， [Robert McMurray](https://github.com/rmcmurray)

> 本教學課程會示範如何取代預設資料儲存機制的[ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) EntityFramework （SQL 用戶端提供者） 與 MySQL 提供者使用。


下列主題將涵蓋在本教學課程：

- 在 Azure 上建立 MySQL 資料庫
- 建立 MVC 應用程式使用 Visual Studio 2013 MVC 範本
- 設定 EntityFramework 為處理 MySQL 資料庫提供者
- 執行應用程式來驗證結果

在本教學課程結束時，您必須儲存 ASP.NET Identity 的 MVC 應用程式正在使用裝載於 Azure 的 MySQL 資料庫。

## <a name="creating-a-mysql-database-instance-on-azure"></a>在 Azure 上建立的 MySQL 資料庫執行個體

1. 登入[Azure 入口網站](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409)。
2. 按一下**新增**底部的頁面上，，然後選取**存放區**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. 在**選擇和附加元件**精靈中，選取**ClearDB 的 MySQL 資料庫**，然後按一下 **下一步**框架底部的箭號：  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. 保留預設值**免費**，計劃變更**名稱**至**IdentityMySQLDatabase**，選取 是，最接近的區域，然後按一下**下一步**框架底部的箭號：  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. 按一下**購買**核取記號，完成資料庫的建立。  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. 建立資料庫之後，您可以管理從**附加元件**管理入口網站中的索引標籤。 若要擷取之資料庫的連接資訊，請按一下**連接資訊**底部的頁面：  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. 複製連接字串上的 [複製] 按鈕即可**CONNECTIONSTRING** ; 欄位，並將它儲存為 MVC 應用程式將這項資訊稍後在本教學課程：  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>建立 MVC 應用程式專案

若要完成本教學課程的這一節中的步驟，您首先需要安裝[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。 一旦安裝 Visual Studio 之後，請建立新的 MVC 應用程式專案中使用下列步驟：

1. 開啟 Visual Studio 2103。
2. 按一下**新專案**從**啟動**] 頁面上，或者您可以按一下 [**檔案**功能表，然後**新專案**:  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. 當**新專案**] 對話方塊隨即顯示，接著展開**Visual C#** 在範本清單，然後按一下 [ **Web**，並選取**ASP.NET Web 應用程式**. 命名您的專案**IdentityMySQLDemo** ，然後按一下 **確定**:  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. 在**新增 ASP.NET 專案**對話方塊中，選取**MVC** templatewith 預設選項; 這將會設定**個別使用者帳戶**做為驗證方法。 按一下**確定**:  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>設定要使用的 MySQL 資料庫 EntityFramework

### <a name="update-the-entity-framework-assembly-for-your-project"></a>更新您的專案的 Entity Framework 組件

從 Visual Studio 2013 範本建立的 MVC 應用程式包含的參考[EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework)封裝，但必須已包含該組件，因為其版本更新的重要效能改善。 若要在應用程式中使用這些最新的更新，請使用下列步驟。

1. Visual Studio 2013 中開啟 MVC 專案。
2. 按一下**工具**，然後按一下 **程式庫套件管理員**，然後按一下  **Package Manager Console**:  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Package Manager Console**會出現在 Visual Studio 的下一節。 型別&quot;**更新套件 EntityFramework** &quot;按下 Enter:  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>安裝 EntityFramework 的 MySQL 提供者

為了讓 EntityFramework 連接到 MySQL 資料庫，您要安裝 MySQL 提供者。 若要這樣做，請開啟**Package Manager Console**和型別&quot; **Install-package MySql.Data.Entity 前**&quot;，然後按 Enter 鍵。

> [!NOTE]
> 這是發行前版本的組件，而且在這種情況可能包含 bug。 您不應該在生產環境中使用的提供者的發行前版本。


[按一下以展開圖]。  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>對您的應用程式的 Web.config 檔案中的專案組態變更

在本節中，您將設定 Entity Framework 使用您剛才安裝的 MySQL 提供者，註冊 MySQL 提供者處理站，並加入您的連接字串從 Azure。

> [!NOTE]
> 下列範例包含 MySql.Data.dll 特定組件版本。 如果組件版本變更，您必須修改適當的組態設定，以正確的版本。


1. Visual Studio 2013 中開啟您專案的 Web.config 檔。
2. 找出 Entity Framework 定義的預設資料庫提供者和 factory 的下列組態設定：

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. 取代下列程式碼，將設定為使用 MySQL 提供者的 Entity Framework 中的這些組態設定： 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. 找出&lt;connectionStrings&gt;區段並取代為下列程式碼，將會定義在 Azure 裝載您的 MySQL 資料庫的連接字串 (請注意 providerName 值從也有所變更原始）：

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>加入自訂 MigrationHistory 內容

使用 entity Framework Code First **MigrationHistory**資料表追蹤的模型變更，並確保資料庫結構描述與概念結構描述之間的一致性。 不過，這個表格不適用於 MySQL 預設因為太大的主索引鍵。 若要補救這種情況下，您必須針對該資料表索引鍵的大小縮減。 若要這樣做，請使用下列步驟：

1. 此資料表的結構描述資訊中擷取**HistoryContext**，可能會修改任何其他**DbContext**。 若要這樣做，請將加入新的類別檔案命名為**MySqlHistoryContext.cs**至專案，並將其內容取代下列程式碼：

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. 接下來您將需要設定 Entity Framework 使用修改後**HistoryContext**，而不是預設值。 利用程式碼為基礎的組態功能可以完成此動作。 若要這樣做，請將加入新的類別檔案命名為**MySqlConfiguration.cs**至您的專案，並取代其內容：

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>建立 ApplicationDbContext 自訂 EntityFramework 初始設定式

在本教學課程顯示為精選 MySQL 提供者目前不支援 Entity Framework 移轉，因此您必須使用模型初始設定式，才能連接到資料庫。 因為本教學課程在 Azure 上使用的 MySQL 執行個體，您必須建立自訂的 Entity Framework 初始設定式。

> [!NOTE]
> 如果您要連接到 Azure，或如果您使用內部部署裝載的資料庫上的 SQL Server 執行個體，就不需要此步驟。


若要建立 MySQL 自訂 Entity Framework 初始設定式，請使用下列步驟：

1. 加入新的類別檔案命名為**MySqlInitializer.cs**很至專案，並取代為下列程式碼的內容： 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. 開啟**IdentityModels.cs**檔案位於您專案**模型**目錄，並以下列內容取代它的內容： 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>執行應用程式，而且正在驗證資料庫

當您完成前面幾節中的步驟時，您應該測試您的資料庫。 若要這樣做，請使用下列步驟：

1. 按**Ctrl + F5**建置並執行 web 應用程式。
2. 按一下**註冊**的頁面頂端的索引標籤：  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. 輸入新的使用者名稱和密碼，然後再按一下**註冊**:  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. 此時將 ASP.NET Identity 資料表建立的 MySQL 資料庫，並註冊並登入應用程式使用者：  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>安裝 MySQL Workbench 工具來確認資料

1. 安裝**MySQL Workbench**工具[MySQL 下載頁面](http://dev.mysql.com/downloads/windows/installer/)
2. 在安裝精靈：**特徵選取**索引標籤上，選取**MySQL Workbench**下**應用程式**> 一節。
3. 啟動應用程式，並加入新的連接使用連接字串中的資料在本教學課程的供您建立 Azure 的 MySQL 資料庫。
4. 建立連接後，檢查**ASP.NET Identity**資料表上建立**IdentityMySQLDatabase。**
5. 您會看到所有 ASP.NET 識別所都需資料表會建立在下列影像所示：  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. 檢查**aspnetusers**資料表執行個體若要檢查的項目，當您註冊新的使用者。  
  
   [按一下以展開圖。 ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)

---
title: 在 ASP.NET Core 中設定身分識別主索引鍵資料類型
author: AdrienTorris
description: 深入瞭解設定用於 ASP.NET Core 識別主索引鍵的所需的資料類型的步驟。
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: ce654492dc7bab6c031c9f82555f877f642171ce
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>在 ASP.NET Core 中設定身分識別主索引鍵資料類型

ASP.NET Core 身分識別可讓您設定用來表示主索引鍵的資料類型。 識別使用`string`預設的資料型別。 您可以覆寫此行為。

## <a name="customize-the-primary-key-data-type"></a>自訂主索引鍵資料類型

1. 建立的自訂實作[IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1)類別。 它代表要用來建立使用者物件的類型。 在下列範例中，預設值`string`類型取代`Guid`。

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. 建立的自訂實作[IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1)類別。 它代表要用來建立角色物件的類型。 在下列範例中，預設值`string`類型取代`Guid`。

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. 建立自訂的資料庫內容類別。 它會繼承自 Entity Framework 資料庫內容類別用於身分識別。 `TUser`和`TRole`引數會參考在前一個步驟中，分別建立的自訂使用者和角色類別。 `Guid`資料型別定義的主索引鍵。

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. 新增身分識別服務應用程式的啟動類別中時，請註冊自訂的資料庫內容類別。

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
    `AddEntityFrameworkStores`方法不會接受`TKey`引數，因為它未在 ASP.NET Core 1.x。 主索引鍵資料類型藉由分析推斷`DbContext`物件。

    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
    `AddEntityFrameworkStores`方法會接受`TKey`指出主索引鍵的資料型別引數。

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   * * *
## <a name="test-the-changes"></a>測試變更

完成時的組態變更，表示主索引鍵的屬性會反映新的資料類型。 下列範例示範如何存取此 MVC 控制器中的屬性。

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]

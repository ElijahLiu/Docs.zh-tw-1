---
uid: signalr/overview/security/hub-authorization
title: SignalR 中樞的驗證和授權 |Microsoft 文件
author: pfletcher
description: 本主題描述如何限制哪些使用者或角色可以存取中樞方法。 本主題中使用的軟體版本 Visual Studio 2013.NET 4.5 SignalR ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8e3bc8889efb1be80c57084fb04dc8030b386601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872750"
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>SignalR 中樞的驗證和授權
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主題描述如何限制哪些使用者或角色可以存取中樞方法。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主題的先前版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

此主題包括下列章節：

- [授權屬性](#authorizeattribute)
- [需要之所有驗證](#requireauth)
- [自訂的授權](#custom)
- [將驗證資訊傳遞給用戶端](#passauth)
- [.NET 用戶端的驗證選項](#authoptions)

    - [使用表單驗證 cookie](#cookie)
    - [Windows 驗證](#windows)
    - [連接標頭](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>授權屬性

提供 SignalR[授權](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)屬性來指定哪些使用者或角色有存取權的集線器或方法。 這個屬性位於`Microsoft.AspNet.SignalR`命名空間。 您套用`Authorize`屬性到集線器或中樞中的特定方法。 當您將套用`Authorize`屬性套用至 hub 類別，指定的授權需求套用至所有中樞中的方法。 本主題提供您可以將套用的授權需求的不同類型的範例。 不含`Authorize`屬性，已連線用戶端可以存取中樞上的任何公用方法。

如果您已經定義名為"Admin"，web 應用程式中的角色，您可以指定只有該角色中的使用者可以存取的集線器以下列程式碼。

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

或者，您可以指定一種方法，可供所有使用者和第二個方法，才可以使用已驗證的使用者，如下所示，中樞會包含。

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

下列範例會處理不同的授權案例：

- `[Authorize]` – 僅限驗證的使用者
- `[Authorize(Roles = "Admin,Manager")]` – 僅限驗證的使用者指定的角色
- `[Authorize(Users = "user1,user2")]` – 僅限驗證的使用者指定的使用者名稱
- `[Authorize(RequireOutgoing=false)]` – 只有經過驗證的使用者可以叫用中樞，但從伺服器傳回給用戶端的呼叫不受限制的授權，例如，只有特定使用者可以傳送訊息，但其他所有項目可以接收訊息時。 RequireOutgoing 屬性只能套用至整個中樞，不是在中樞內的個人版方法。 RequireOutgoing 未設定為 false，會從伺服器呼叫符合授權需求的使用者。

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>需要之所有驗證

您可以對所有中樞和中樞方法進行驗證，應用程式中，藉由呼叫[RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)應用程式啟動時的方法。 當您有多個中樞，並想要強制所有的驗證需求，您可以使用這個方法。 使用此方法，您無法指定角色、 使用者或外寄的授權的需求。 您只可以指定存取中樞的方法是限制為已驗證的使用者。 不過，您仍然可以套用 Authorize 屬性中心或方法，以指定其他需求。 基本驗證的需求會加入您指定在屬性中的任何需求。

下列範例會示範已驗證的使用者限制所有中樞方法的啟動檔案。

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

如果您呼叫`RequireAuthentication()`方法 SignalR 要求已處理之後，SignalR 會擲回`InvalidOperationException`例外狀況。 SignalR 擲回這個例外狀況，因為您無法將模組加入 HubPipeline，叫用管線之後。 上述範例示範呼叫`RequireAuthentication`方法中的`Configuration`之前處理第一個要求一次執行此方法。

<a id="custom"></a>

## <a name="customized-authorization"></a>自訂的授權

如果您需要自訂如何決定授權，您可以建立衍生自類別`AuthorizeAttribute`並覆寫[UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)方法。 針對每個要求，SignalR 會叫用這個方法，以判斷是否授權使用者完成要求。 在覆寫方法中，您會為授權案例提供必要的邏輯。 下列範例會示範如何強制執行宣告式身分識別授權。

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>將驗證資訊傳遞給用戶端

您可能需要在用戶端執行的程式碼中使用的驗證資訊。 在用戶端上呼叫方法時，您會傳遞所需的資訊。 例如，交談的應用程式方法無法當做參數傳遞張貼郵件的人員的使用者名稱如下所示。

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

或者，您可以建立的物件來代表驗證資訊，並將該物件傳遞為參數，如下所示。

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

您永遠不會應該傳遞給其他用戶端，一部用戶端連接識別碼，因為惡意使用者可以用它來模擬該用戶端的要求。

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>.NET 用戶端的驗證選項

當您有.NET 用戶端，例如主控台應用程式，與為集線器，僅限於驗證的使用者互動時，您可以傳遞在 cookie 中，連接標頭或憑證的驗證認證。 本節中的範例示範如何使用這些不同的方法來驗證使用者。 它們不是完整功能的 SignalR 應用程式。 如需與 SignalR 的.NET 用戶端的詳細資訊，請參閱[集線器 API 指南-.NET 用戶端](../guide-to-the-api/hubs-api-guide-net-client.md)。

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

當.NET 用戶端會使用 ASP.NET 表單驗證的集線器與互動時，您必須以手動方式在連接上設定的驗證 cookie。 新增 cookie`CookieContainer`屬性[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)物件。 下列範例主控台應用程式會從網頁擷取驗證 cookie 並將該 cookie 新增到連線。

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

主控台應用程式將張貼到認證<strong>www.contoso.com/RemoteLogin</strong>其無法參考包含下列程式碼後置檔案的空白頁面。

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows 驗證

使用 Windows 驗證時，您可以使用傳遞目前使用者的認證[DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)屬性。 您可以設定連線認證 DefaultCredentials 的值。

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>連接標頭

如果您的應用程式不使用 cookie，您可以在連接標頭中傳遞使用者資訊。 例如，您可以連接標頭中傳遞權杖。

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

然後，在中樞，您會確認使用者的權杖。

<a id="certificate"></a>

### <a name="certificate"></a>憑證

您可以傳遞用戶端憑證，以驗證使用者。 建立連接時，您可以加入憑證。 下列範例顯示如何只將用戶端憑證新增至連線;它不會顯示完整的主控台應用程式。 它會使用[X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)類別提供幾種不同的方式建立憑證。

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]

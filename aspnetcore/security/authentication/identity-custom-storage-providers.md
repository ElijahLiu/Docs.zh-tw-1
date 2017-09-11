---
title: "自訂的儲存提供者，ASP.NET Core 識別 |Microsoft 文件"
author: ardalis
description: "如何設定自訂儲存 ASP.NET Core 身分識別提供者。"
keywords: "ASP.NET Core，身分識別，自訂的存放裝置提供者"
ms.author: riande
manager: wpickett
ms.date: 05/24/2017
ms.topic: article
ms.assetid: b2ace545-ecf6-4664-b31e-b65bd4a6b025
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 33d02f595ff34a297d3046894edc8a6f5bd42275
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a><span data-ttu-id="83200-104">ASP.NET Core 身分識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="83200-104">Custom storage providers for ASP.NET Core Identity</span></span>

<span data-ttu-id="83200-105">由[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="83200-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="83200-106">ASP.NET Core 身分識別是可擴充的系統可讓您建立自訂的儲存提供者，然後連接到您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="83200-106">ASP.NET Core Identity is an extensible system which enables you to create a custom storage provider and connect it to your app.</span></span> <span data-ttu-id="83200-107">本主題描述如何建立 ASP.NET Core 身分識別的自訂儲存體提供者。</span><span class="sxs-lookup"><span data-stu-id="83200-107">This topic describes how to create a customized storage provider for ASP.NET Core Identity.</span></span> <span data-ttu-id="83200-108">它涵蓋建立自己的儲存體提供者的重要概念，但不是會逐步解說。</span><span class="sxs-lookup"><span data-stu-id="83200-108">It covers the important concepts for creating your own storage provider, but is not a step-by-step walkthrough.</span></span>

<span data-ttu-id="83200-109">[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample)。</span><span class="sxs-lookup"><span data-stu-id="83200-109">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).</span></span>

## <a name="introduction"></a><span data-ttu-id="83200-110">簡介</span><span class="sxs-lookup"><span data-stu-id="83200-110">Introduction</span></span>

<span data-ttu-id="83200-111">根據預設，ASP.NET Core 識別系統會將使用者資訊儲存在 SQL Server 資料庫中使用 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="83200-111">By default, the ASP.NET Core Identity system stores user information in a SQL Server database using Entity Framework Core.</span></span> <span data-ttu-id="83200-112">這個方法對於許多應用程式，非常適合。</span><span class="sxs-lookup"><span data-stu-id="83200-112">For many apps, this approach works well.</span></span> <span data-ttu-id="83200-113">不過，您可能想要使用不同的持續性機制或資料結構描述。</span><span class="sxs-lookup"><span data-stu-id="83200-113">However, you may prefer to use a different persistence mechanism or data schema.</span></span> <span data-ttu-id="83200-114">例如: </span><span class="sxs-lookup"><span data-stu-id="83200-114">For example:</span></span>

* <span data-ttu-id="83200-115">您使用[Azure 資料表儲存體](https://docs.microsoft.com/azure/storage/)或另一個資料存放區。</span><span class="sxs-lookup"><span data-stu-id="83200-115">You use [Azure Table Storage](https://docs.microsoft.com/azure/storage/) or another data store.</span></span>
* <span data-ttu-id="83200-116">資料庫資料表有不同的結構。</span><span class="sxs-lookup"><span data-stu-id="83200-116">Your database tables have a different structure.</span></span> 
* <span data-ttu-id="83200-117">您可能想要使用不同的資料存取方法，例如[Dapper](https://github.com/StackExchange/Dapper)。</span><span class="sxs-lookup"><span data-stu-id="83200-117">You may wish to use a different data access approach, such as [Dapper](https://github.com/StackExchange/Dapper).</span></span> 

<span data-ttu-id="83200-118">在每個這種情況下，您可以撰寫自訂提供者的儲存機制，並插入您的應用程式中的該提供者。</span><span class="sxs-lookup"><span data-stu-id="83200-118">In each of these cases, you can write a customized provider for your storage mechanism and plug that provider into your app.</span></span>

<span data-ttu-id="83200-119">ASP.NET Core 識別隨附於 Visual Studio 中的專案範本與 「 個別使用者帳戶 」 選項。</span><span class="sxs-lookup"><span data-stu-id="83200-119">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="83200-120">當使用.NET 核心 CLI，新增`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="83200-120">When using the .NET Core CLI, add `-au Individual`:</span></span>

```
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a><span data-ttu-id="83200-121">ASP.NET Core 識別架構</span><span class="sxs-lookup"><span data-stu-id="83200-121">The ASP.NET Core Identity architecture</span></span>

<span data-ttu-id="83200-122">ASP.NET Core 識別類別，稱為管理員和存放區所組成。</span><span class="sxs-lookup"><span data-stu-id="83200-122">ASP.NET Core Identity consists of classes called managers and stores.</span></span> <span data-ttu-id="83200-123">*管理員*是高階應用程式開發人員用來執行作業，例如建立身分識別使用者的類別。</span><span class="sxs-lookup"><span data-stu-id="83200-123">*Managers* are high-level classes which an app developer uses to perform operations, such as creating an Identity user.</span></span> <span data-ttu-id="83200-124">*存放區*是較低層級的類別，指定如何保存實體，例如使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="83200-124">*Stores* are lower-level classes that specify how entities, such as users and roles, are persisted.</span></span> <span data-ttu-id="83200-125">請遵循存放區[儲存機制模式](http://deviq.com/repository-pattern/)和緊密結合的持續性機制。</span><span class="sxs-lookup"><span data-stu-id="83200-125">Stores follow the [repository pattern](http://deviq.com/repository-pattern/) and are closely coupled with the persistence mechanism.</span></span> <span data-ttu-id="83200-126">管理員會分開存放區，這表示您可以取代持續性機制，而不需要變更您的應用程式程式碼 （除了組態）。</span><span class="sxs-lookup"><span data-stu-id="83200-126">Managers are decoupled from stores, which means you can replace the persistence mechanism without changing your application code (except for configuration).</span></span>

<span data-ttu-id="83200-127">下圖顯示 web 應用程式的互動方式經理，而與資料存取層的存放區互動。</span><span class="sxs-lookup"><span data-stu-id="83200-127">The following diagram shows how a web app interacts with the managers, while stores interact with the data access layer.</span></span>

![ASP.NET Core 應用程式使用 （例如，'UserManager'、 'RoleManager'） 的管理員。](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

<span data-ttu-id="83200-130">若要建立自訂的儲存提供者，建立資料來源、 資料存取層級和與此資料存取層 （綠色和灰色方塊上圖所示） 互動的存放區類別。</span><span class="sxs-lookup"><span data-stu-id="83200-130">To create a custom storage provider, create the data source, the data access layer, and the store classes that interact with this data access layer (the green and grey boxes in the diagram above).</span></span> <span data-ttu-id="83200-131">您不需要自訂的管理員或應用程式程式碼互動 （藍色方塊上面）。</span><span class="sxs-lookup"><span data-stu-id="83200-131">You don't need to customize the managers or your app code that interacts with them (the blue boxes above).</span></span>

<span data-ttu-id="83200-132">建立的新執行個體時`UserManager`或`RoleManager`您提供使用者類別的型別，並將存放區類別的執行個體傳遞做為引數。</span><span class="sxs-lookup"><span data-stu-id="83200-132">When creating a new instance of `UserManager` or `RoleManager` you provide the type of the user class and pass an instance of the store class as an argument.</span></span> <span data-ttu-id="83200-133">這個方法可讓您將 ASP.NET Core 插入您的自訂的類別。</span><span class="sxs-lookup"><span data-stu-id="83200-133">This approach enables you to plug your customized classes into ASP.NET Core.</span></span> 

<span data-ttu-id="83200-134">[重新設定應用程式使用新的存放裝置提供者](#reconfigure-app-to-use-new-storage-provider)示範如何具現化`UserManager`和`RoleManager`與自訂存放區。</span><span class="sxs-lookup"><span data-stu-id="83200-134">[Reconfigure app to use new storage provider](#reconfigure-app-to-use-new-storage-provider) shows how to instantiate `UserManager` and `RoleManager` with a customized store.</span></span>

## <a name="aspnet-core-identity-stores-data-types"></a><span data-ttu-id="83200-135">ASP.NET Core 身分識別儲存的資料類型</span><span class="sxs-lookup"><span data-stu-id="83200-135">ASP.NET Core Identity stores data types</span></span>

<span data-ttu-id="83200-136">[ASP.NET Core 識別](https://github.com/aspnet/identity)資料類型詳述於以下各節：</span><span class="sxs-lookup"><span data-stu-id="83200-136">[ASP.NET Core Identity](https://github.com/aspnet/identity) data types are detailed in the following sections:</span></span>

### <a name="users"></a><span data-ttu-id="83200-137">使用者</span><span class="sxs-lookup"><span data-stu-id="83200-137">Users</span></span>

<span data-ttu-id="83200-138">註冊您的網站的使用者。</span><span class="sxs-lookup"><span data-stu-id="83200-138">Registered users of your web site.</span></span> <span data-ttu-id="83200-139">[IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)類型可能會擴充，或使用您自己的自訂類型的範例。</span><span class="sxs-lookup"><span data-stu-id="83200-139">The [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) type may be extended or used as an example for your own custom type.</span></span> <span data-ttu-id="83200-140">您不需要繼承自特定的型別，以實作您自己的自訂身分識別儲存體解決方案。</span><span class="sxs-lookup"><span data-stu-id="83200-140">You do not need to inherit from a particular type to implement your own custom identity storage solution.</span></span>

### <a name="user-claims"></a><span data-ttu-id="83200-141">使用者宣告</span><span class="sxs-lookup"><span data-stu-id="83200-141">User Claims</span></span>

<span data-ttu-id="83200-142">一組陳述式 (或[宣告](https://msdn.microsoft.com/library/system.security.claims.claim(v=vs.110).aspx)) 代表使用者的身分識別之使用者的相關。</span><span class="sxs-lookup"><span data-stu-id="83200-142">A set of statements (or [claims](https://msdn.microsoft.com/library/system.security.claims.claim(v=vs.110).aspx)) about the user that represent the user's identity.</span></span> <span data-ttu-id="83200-143">可以啟用更高的使用者身分識別與透過角色能達到之效果的運算式。</span><span class="sxs-lookup"><span data-stu-id="83200-143">Can enable greater expression of the user's identity than can be achieved through roles.</span></span>

### <a name="user-logins"></a><span data-ttu-id="83200-144">使用者登入</span><span class="sxs-lookup"><span data-stu-id="83200-144">User Logins</span></span>

<span data-ttu-id="83200-145">外部驗證提供者 （例如 Facebook 或 Microsoft 帳戶） 的相關資訊時登入使用者使用。</span><span class="sxs-lookup"><span data-stu-id="83200-145">Information about the external authentication provider (like Facebook or a Microsoft account) to use when logging in a user.</span></span> [<span data-ttu-id="83200-146">範例</span><span class="sxs-lookup"><span data-stu-id="83200-146">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a><span data-ttu-id="83200-147">角色</span><span class="sxs-lookup"><span data-stu-id="83200-147">Roles</span></span>

<span data-ttu-id="83200-148">您的網站的授權群組。</span><span class="sxs-lookup"><span data-stu-id="83200-148">Authorization groups for your site.</span></span> <span data-ttu-id="83200-149">包含的角色識別碼和角色名稱 （例如"Admin"或"Employee"）。</span><span class="sxs-lookup"><span data-stu-id="83200-149">Includes the role Id and role name (like "Admin" or "Employee").</span></span> [<span data-ttu-id="83200-150">範例</span><span class="sxs-lookup"><span data-stu-id="83200-150">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a><span data-ttu-id="83200-151">資料存取層</span><span class="sxs-lookup"><span data-stu-id="83200-151">The data access layer</span></span>

<span data-ttu-id="83200-152">本主題假設您熟悉您要使用的持續性機制，以及如何建立實體，該機制。</span><span class="sxs-lookup"><span data-stu-id="83200-152">This topic assumes you are familiar with the persistence mechanism that you are going to use and how to create entities for that mechanism.</span></span> <span data-ttu-id="83200-153">本主題不提供如何建立儲存機制或資料存取類別; 詳細資料使用 ASP.NET Core 識別時，它會提供有關設計決策的一些建議。</span><span class="sxs-lookup"><span data-stu-id="83200-153">This topic does not provide details about how to create the repositories or data access classes; it provides some suggestions about design decisions when working with ASP.NET Core Identity.</span></span>

<span data-ttu-id="83200-154">設計自訂存放區提供者的資料存取層時，您會有很多的自由。</span><span class="sxs-lookup"><span data-stu-id="83200-154">You have a lot of freedom when designing the data access layer for a customized store provider.</span></span> <span data-ttu-id="83200-155">您只需要建立持續性機制，可用於您想要使用您的應用程式中的功能。</span><span class="sxs-lookup"><span data-stu-id="83200-155">You only need to create persistence mechanisms for features that you intend to use in your app.</span></span> <span data-ttu-id="83200-156">例如，如果您不會在您的應用程式中使用角色，您不需要建立的角色或使用者角色關聯的儲存體。</span><span class="sxs-lookup"><span data-stu-id="83200-156">For example, if you are not using roles in your app, you do not need to create storage for roles or user role associations.</span></span> <span data-ttu-id="83200-157">您的技術和現有的基礎結構，可能需要與 ASP.NET Core 身分識別的預設實作非常不同的結構。</span><span class="sxs-lookup"><span data-stu-id="83200-157">Your technology and existing infrastructure may require a structure that is very different from the default implementation of ASP.NET Core Identity.</span></span> <span data-ttu-id="83200-158">資料存取層，您提供的邏輯來處理您的儲存體實作的結構。</span><span class="sxs-lookup"><span data-stu-id="83200-158">In your data access layer, you provide the logic to work with the structure of your storage implementation.</span></span>

<span data-ttu-id="83200-159">資料存取層提供邏輯以從 ASP.NET Core 身分識別的資料儲存至資料來源。</span><span class="sxs-lookup"><span data-stu-id="83200-159">The data access layer provides the logic to save the data from ASP.NET Core Identity to a data source.</span></span> <span data-ttu-id="83200-160">資料存取層，您自訂的存放裝置提供者可能會包含下列類別，以儲存使用者和角色的資訊。</span><span class="sxs-lookup"><span data-stu-id="83200-160">The data access layer for your customized storage provider might include the following classes to store user and role information.</span></span>

### <a name="context-class"></a><span data-ttu-id="83200-161">Context 類別</span><span class="sxs-lookup"><span data-stu-id="83200-161">Context class</span></span>

<span data-ttu-id="83200-162">封裝連接到您的持續性機制，並執行查詢的資訊。</span><span class="sxs-lookup"><span data-stu-id="83200-162">Encapsulates the information to connect to your persistence mechanism and execute queries.</span></span> <span data-ttu-id="83200-163">數個資料類別需要這個類別，通常透過相依性插入所提供的執行個體。</span><span class="sxs-lookup"><span data-stu-id="83200-163">Several data classes require an instance of this class, typically provided through dependency injection.</span></span> <span data-ttu-id="83200-164">[範例](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1)。</span><span class="sxs-lookup"><span data-stu-id="83200-164">[Example](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).</span></span>

### <a name="user-storage"></a><span data-ttu-id="83200-165">使用者的儲存體</span><span class="sxs-lookup"><span data-stu-id="83200-165">User Storage</span></span>

<span data-ttu-id="83200-166">儲存和擷取使用者資訊 （例如使用者名稱和密碼雜湊）。</span><span class="sxs-lookup"><span data-stu-id="83200-166">Stores and retrieves user information (such as user name and password hash).</span></span> [<span data-ttu-id="83200-167">範例</span><span class="sxs-lookup"><span data-stu-id="83200-167">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a><span data-ttu-id="83200-168">角色存放裝置</span><span class="sxs-lookup"><span data-stu-id="83200-168">Role Storage</span></span>

<span data-ttu-id="83200-169">儲存和擷取角色資訊 （例如角色名稱）。</span><span class="sxs-lookup"><span data-stu-id="83200-169">Stores and retrieves role information (such as the role name).</span></span> [<span data-ttu-id="83200-170">範例</span><span class="sxs-lookup"><span data-stu-id="83200-170">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a><span data-ttu-id="83200-171">UserClaims 的儲存體</span><span class="sxs-lookup"><span data-stu-id="83200-171">UserClaims Storage</span></span>

<span data-ttu-id="83200-172">儲存和擷取使用者宣告資訊 （例如宣告類型和值）。</span><span class="sxs-lookup"><span data-stu-id="83200-172">Stores and retrieves user claim information (such as the claim type and value).</span></span> [<span data-ttu-id="83200-173">範例</span><span class="sxs-lookup"><span data-stu-id="83200-173">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a><span data-ttu-id="83200-174">Serlogins 的儲存體</span><span class="sxs-lookup"><span data-stu-id="83200-174">UserLogins Storage</span></span>

<span data-ttu-id="83200-175">儲存和擷取使用者登入資訊 （例如外部驗證提供者）。</span><span class="sxs-lookup"><span data-stu-id="83200-175">Stores and retrieves user login information (such as an external authentication provider).</span></span> [<span data-ttu-id="83200-176">範例</span><span class="sxs-lookup"><span data-stu-id="83200-176">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a><span data-ttu-id="83200-177">使用者角色存放裝置</span><span class="sxs-lookup"><span data-stu-id="83200-177">UserRole Storage</span></span>

<span data-ttu-id="83200-178">儲存和擷取哪些角色指派給使用者。</span><span class="sxs-lookup"><span data-stu-id="83200-178">Stores and retrieves which roles are assigned to which users.</span></span> [<span data-ttu-id="83200-179">範例</span><span class="sxs-lookup"><span data-stu-id="83200-179">Example</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

<span data-ttu-id="83200-180">**提示：**只實作您想要使用您的應用程式中的類別。</span><span class="sxs-lookup"><span data-stu-id="83200-180">**TIP:** Only implement the classes you intend to use in your app.</span></span>

<span data-ttu-id="83200-181">在資料存取類別中，提供程式碼以執行資料作業的持續性機制。</span><span class="sxs-lookup"><span data-stu-id="83200-181">In the data access classes, provide code to perform data operations for your persistence mechanism.</span></span> <span data-ttu-id="83200-182">比方說，在自訂的提供者，您可能必須建立新的使用者，在下列程式碼*儲存*類別：</span><span class="sxs-lookup"><span data-stu-id="83200-182">For example, within a custom provider, you might have the following code to create a new user in the *store* class:</span></span>

<span data-ttu-id="83200-183">[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="83200-183">[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]</span></span>

<span data-ttu-id="83200-184">建立使用者的實作邏輯處於``_usersTable.CreateAsync``方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="83200-184">The implementation logic for creating the user is in the ``_usersTable.CreateAsync`` method, shown below.</span></span>

## <a name="customize-the-user-class"></a><span data-ttu-id="83200-185">自訂使用者類別</span><span class="sxs-lookup"><span data-stu-id="83200-185">Customize the user class</span></span>

<span data-ttu-id="83200-186">當實作的儲存提供者，建立使用者類別，其相當於[`IdentityUser`類別](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser)。</span><span class="sxs-lookup"><span data-stu-id="83200-186">When implementing a storage provider, create a user class which is equivalent to the [`IdentityUser` class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).</span></span>

<span data-ttu-id="83200-187">至少必須包含在使用者類別`Id`和`UserName`屬性。</span><span class="sxs-lookup"><span data-stu-id="83200-187">At a minimum, your user class must include an `Id` and a `UserName` property.</span></span>

<span data-ttu-id="83200-188">`IdentityUser`類別定義的內容，``UserManager``呼叫時執行要求的作業。</span><span class="sxs-lookup"><span data-stu-id="83200-188">The `IdentityUser` class defines the properties that the ``UserManager`` calls when performing requested operations.</span></span> <span data-ttu-id="83200-189">預設類型`Id`屬性是一個字串，但您可以繼承自`IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>`並指定不同的類型。</span><span class="sxs-lookup"><span data-stu-id="83200-189">The default type of the `Id` property is a string, but you can inherit from `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` and specify a different type.</span></span> <span data-ttu-id="83200-190">架構會預期要處理的資料類型轉換的儲存體實作。</span><span class="sxs-lookup"><span data-stu-id="83200-190">The framework expects the storage implementation to handle data type conversions.</span></span>

## <a name="customize-the-user-store"></a><span data-ttu-id="83200-191">自訂使用者存放區</span><span class="sxs-lookup"><span data-stu-id="83200-191">Customize the user store</span></span>

<span data-ttu-id="83200-192">建立`UserStore`提供的所有使用者資料作業方式的類別。</span><span class="sxs-lookup"><span data-stu-id="83200-192">Create a `UserStore` class that provides the methods for all data operations on the user.</span></span> <span data-ttu-id="83200-193">這個類別就相當於[UserStore<TUser> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="83200-193">This class is equivalent to the [UserStore<TUser>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) class.</span></span> <span data-ttu-id="83200-194">在您`UserStore`類別，實作`IUserStore<TUser>`和所需的選用介面。</span><span class="sxs-lookup"><span data-stu-id="83200-194">In your `UserStore` class, implement `IUserStore<TUser>` and the optional interfaces required.</span></span> <span data-ttu-id="83200-195">您選取要實作的選擇性介面上根據您的應用程式中提供的功能。</span><span class="sxs-lookup"><span data-stu-id="83200-195">You select which optional interfaces to implement based on on the functionality provided in your app.</span></span>

### <a name="optional-interfaces"></a><span data-ttu-id="83200-196">選擇性介面</span><span class="sxs-lookup"><span data-stu-id="83200-196">Optional interfaces</span></span>

- <span data-ttu-id="83200-197">IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1</span><span class="sxs-lookup"><span data-stu-id="83200-197">IUserRoleStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserrolestore-1</span></span>
- <span data-ttu-id="83200-198">IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span><span class="sxs-lookup"><span data-stu-id="83200-198">IUserClaimStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserclaimstore-1</span></span>
- <span data-ttu-id="83200-199">IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span><span class="sxs-lookup"><span data-stu-id="83200-199">IUserPasswordStore https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.iuserpasswordstore-1</span></span>
- <span data-ttu-id="83200-200">IUserSecurityStampStore<!-- make these all links and remove / --></span><span class="sxs-lookup"><span data-stu-id="83200-200">IUserSecurityStampStore <!-- make these all links and remove / --></span></span>
- <span data-ttu-id="83200-201">IUserEmailStore</span><span class="sxs-lookup"><span data-stu-id="83200-201">IUserEmailStore</span></span>
- <span data-ttu-id="83200-202">IPhoneNumberStore</span><span class="sxs-lookup"><span data-stu-id="83200-202">IPhoneNumberStore</span></span>
- <span data-ttu-id="83200-203">IQueryableUserStore</span><span class="sxs-lookup"><span data-stu-id="83200-203">IQueryableUserStore</span></span>
- <span data-ttu-id="83200-204">IUserLoginStore</span><span class="sxs-lookup"><span data-stu-id="83200-204">IUserLoginStore</span></span>
- <span data-ttu-id="83200-205">IUserTwoFactorStore</span><span class="sxs-lookup"><span data-stu-id="83200-205">IUserTwoFactorStore</span></span>
- <span data-ttu-id="83200-206">IUserLockoutStore</span><span class="sxs-lookup"><span data-stu-id="83200-206">IUserLockoutStore</span></span>

<span data-ttu-id="83200-207">選擇性的介面繼承自`IUserStore`。</span><span class="sxs-lookup"><span data-stu-id="83200-207">The optional interfaces inherit from `IUserStore`.</span></span> <span data-ttu-id="83200-208">您可以查看儲存部分實作的範例使用者[這裡](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs)。</span><span class="sxs-lookup"><span data-stu-id="83200-208">You can see a partially implemented sample user store [here](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).</span></span>

<span data-ttu-id="83200-209">內`UserStore`類別，使用您建立用來執行作業的資料存取類別。</span><span class="sxs-lookup"><span data-stu-id="83200-209">Within the `UserStore` class, you use the data access classes that you created to perform operations.</span></span> <span data-ttu-id="83200-210">這些會在使用相依性插入傳遞。</span><span class="sxs-lookup"><span data-stu-id="83200-210">These are passed in using dependency injection.</span></span> <span data-ttu-id="83200-211">例如，在 SQL Server Dapper 實作，`UserStore`類別具有`CreateAsync`方法使用的執行個體`DapperUsersTable`來插入新的記錄：</span><span class="sxs-lookup"><span data-stu-id="83200-211">For example, in the SQL Server with Dapper implementation, the `UserStore` class has the `CreateAsync` method which uses an instance of `DapperUsersTable` to insert a new record:</span></span>

<span data-ttu-id="83200-212">[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="83200-212">[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]</span></span>

### <a name="interfaces-to-implement-when-customizing-user-store"></a><span data-ttu-id="83200-213">若要實作自訂使用者存放區時的介面</span><span class="sxs-lookup"><span data-stu-id="83200-213">Interfaces to implement when customizing user store</span></span>

- <span data-ttu-id="83200-214">**IUserStore**</span><span class="sxs-lookup"><span data-stu-id="83200-214">**IUserStore**</span></span>  
 <span data-ttu-id="83200-215">[IUserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx)介面是只有在使用者存放區中，您必須實作的介面。</span><span class="sxs-lookup"><span data-stu-id="83200-215">The [IUserStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) interface is the only interface you must implement in the user store.</span></span> <span data-ttu-id="83200-216">它會定義方法來建立、 更新、 刪除和擷取使用者。</span><span class="sxs-lookup"><span data-stu-id="83200-216">It defines methods for creating, updating, deleting, and retrieving users.</span></span>
- <span data-ttu-id="83200-217">**IUserClaimStore**</span><span class="sxs-lookup"><span data-stu-id="83200-217">**IUserClaimStore**</span></span>  
 <span data-ttu-id="83200-218">[IUserClaimStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx)介面會定義您要啟用使用者宣告實作的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-218">The [IUserClaimStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) interface defines the methods you implement to enable user claims.</span></span> <span data-ttu-id="83200-219">它包含用於加入、 移除和擷取使用者宣告的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-219">It contains methods for adding, removing and retrieving user claims.</span></span>
- <span data-ttu-id="83200-220">**IUserLoginStore**</span><span class="sxs-lookup"><span data-stu-id="83200-220">**IUserLoginStore**</span></span>  
 <span data-ttu-id="83200-221">[IUserLoginStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx)定義您要啟用外部驗證提供者實作的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-221">The [IUserLoginStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) defines the methods you implement to enable external authentication providers.</span></span> <span data-ttu-id="83200-222">它包含加入、 移除和擷取使用者登入和擷取使用者為基礎的登入資訊方法的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-222">It contains methods for adding, removing and retrieving user logins, and a method for retrieving a user based on the login information.</span></span>
- <span data-ttu-id="83200-223">**IUserRoleStore**</span><span class="sxs-lookup"><span data-stu-id="83200-223">**IUserRoleStore**</span></span>  
 <span data-ttu-id="83200-224">[IUserRoleStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)介面會定義您將使用者對應至角色實作的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-224">The [IUserRoleStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) interface defines the methods you implement to map a user to a role.</span></span> <span data-ttu-id="83200-225">它包含方法，以新增、 移除和擷取使用者的角色，並檢查使用者是否指派給角色的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-225">It contains methods to add, remove, and retrieve a user's roles, and a method to check if a user is assigned to a role.</span></span>
- <span data-ttu-id="83200-226">**IUserPasswordStore**</span><span class="sxs-lookup"><span data-stu-id="83200-226">**IUserPasswordStore**</span></span>  
 <span data-ttu-id="83200-227">[IUserPasswordStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx)介面會定義您實作來保存雜湊的密碼的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-227">The [IUserPasswordStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) interface defines the methods you implement to persist hashed passwords.</span></span> <span data-ttu-id="83200-228">它包含方法來取得和設定雜湊的密碼，並指出使用者是否已設定密碼的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-228">It contains methods for getting and setting the hashed password, and a method that indicates whether the user has set a password.</span></span>
- <span data-ttu-id="83200-229">**IUserSecurityStampStore**</span><span class="sxs-lookup"><span data-stu-id="83200-229">**IUserSecurityStampStore**</span></span>  
 <span data-ttu-id="83200-230">[IUserSecurityStampStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx)介面會定義您實作要用於安全性戳記，指出是否已變更的使用者帳戶資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-230">The [IUserSecurityStampStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) interface defines the methods you implement to use a security stamp for indicating whether the user's account information has changed.</span></span> <span data-ttu-id="83200-231">當使用者變更密碼，或加入或移除登入，則會更新這個戳記。</span><span class="sxs-lookup"><span data-stu-id="83200-231">This stamp is updated when a user changes the password, or adds or removes logins.</span></span> <span data-ttu-id="83200-232">它包含方法來取得和設定的安全性戳記。</span><span class="sxs-lookup"><span data-stu-id="83200-232">It contains methods for getting and setting the security stamp.</span></span>
- <span data-ttu-id="83200-233">**IUserTwoFactorStore**</span><span class="sxs-lookup"><span data-stu-id="83200-233">**IUserTwoFactorStore**</span></span>  
 <span data-ttu-id="83200-234">[IUserTwoFactorStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx)介面會定義您實作以支援雙因素驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-234">The [IUserTwoFactorStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) interface defines the methods you implement to support two factor authentication.</span></span> <span data-ttu-id="83200-235">它包含對取得和設定是否針對使用者啟用雙因素驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-235">It contains methods for getting and setting whether two factor authentication is enabled for a user.</span></span>
- <span data-ttu-id="83200-236">**IUserPhoneNumberStore**</span><span class="sxs-lookup"><span data-stu-id="83200-236">**IUserPhoneNumberStore**</span></span>  
 <span data-ttu-id="83200-237">[IUserPhoneNumberStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx)介面會定義您實作以儲存使用者電話號碼的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-237">The [IUserPhoneNumberStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) interface defines the methods you implement to store user phone numbers.</span></span> <span data-ttu-id="83200-238">它包含對取得和設定的電話號碼和電話號碼是否已確認的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-238">It contains methods for getting and setting the phone number and whether the phone number is confirmed.</span></span>
- <span data-ttu-id="83200-239">**IUserEmailStore**</span><span class="sxs-lookup"><span data-stu-id="83200-239">**IUserEmailStore**</span></span>  
 <span data-ttu-id="83200-240">[IUserEmailStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx)介面會定義您實作以儲存使用者的電子郵件地址的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-240">The [IUserEmailStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) interface defines the methods you implement to store user email addresses.</span></span> <span data-ttu-id="83200-241">它包含對取得和設定電子郵件地址和電子郵件是否已確認的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-241">It contains methods for getting and setting the email address and whether the email is confirmed.</span></span>
- <span data-ttu-id="83200-242">**IUserLockoutStore**</span><span class="sxs-lookup"><span data-stu-id="83200-242">**IUserLockoutStore**</span></span>  
 <span data-ttu-id="83200-243">[IUserLockoutStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx)介面會定義您實作以儲存有關鎖定的帳戶資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-243">The [IUserLockoutStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) interface defines the methods you implement to store information about locking an account.</span></span> <span data-ttu-id="83200-244">它包含追蹤失敗的存取嘗試以及鎖定的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-244">It contains methods for tracking failed access attempts and lockouts.</span></span>
- <span data-ttu-id="83200-245">**IQueryableUserStore**</span><span class="sxs-lookup"><span data-stu-id="83200-245">**IQueryableUserStore**</span></span>  
 <span data-ttu-id="83200-246">[IQueryableUserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx)介面會定義提供可查詢使用者存放區的成員實作。</span><span class="sxs-lookup"><span data-stu-id="83200-246">The [IQueryableUserStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) interface defines the members implement to provide a queryable user store.</span></span>

<span data-ttu-id="83200-247">您在應用程式中實作所需的介面。</span><span class="sxs-lookup"><span data-stu-id="83200-247">You implement only the interfaces that are needed in your app.</span></span> <span data-ttu-id="83200-248">例如: </span><span class="sxs-lookup"><span data-stu-id="83200-248">For example:</span></span>

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a><span data-ttu-id="83200-249">IdentityUserClaim、 IdentityUserLogin 和 IdentityUserRole</span><span class="sxs-lookup"><span data-stu-id="83200-249">IdentityUserClaim, IdentityUserLogin, and IdentityUserRole</span></span>

<span data-ttu-id="83200-250">``Microsoft.AspNet.Identity.EntityFramework``命名空間包含的實作[IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx)， [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)，和[IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="83200-250">The ``Microsoft.AspNet.Identity.EntityFramework`` namespace contains implementations of the [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), and [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) classes.</span></span> <span data-ttu-id="83200-251">如果您使用這些功能，您可能想要建立您自己的版本，這些類別，以及定義您的應用程式的屬性。</span><span class="sxs-lookup"><span data-stu-id="83200-251">If you are using these features, you may want to create your own versions of these classes and define the properties for your app.</span></span> <span data-ttu-id="83200-252">不過，有時候很不載入這些實體記憶體時執行 （例如加入或移除使用者的宣告） 的基本作業更有效率。</span><span class="sxs-lookup"><span data-stu-id="83200-252">However, sometimes it is more efficient to not load these entities into memory when performing basic operations (such as adding or removing a user's claim).</span></span> <span data-ttu-id="83200-253">請改為後端存放區類別可以執行這些作業，直接針對資料來源。</span><span class="sxs-lookup"><span data-stu-id="83200-253">Instead, the backend store classes can execute these operations directly on the data source.</span></span> <span data-ttu-id="83200-254">例如，``UserStore.GetClaimsAsync``方法可以呼叫``userClaimTable.FindByUserId(user.Id)``方法上執行查詢，直接資料表，並傳回清單的宣告。</span><span class="sxs-lookup"><span data-stu-id="83200-254">For example, the ``UserStore.GetClaimsAsync`` method can call the ``userClaimTable.FindByUserId(user.Id)`` method to execute a query on that table directly and return a list of claims.</span></span>

## <a name="customize-the-role-class"></a><span data-ttu-id="83200-255">自訂角色類別</span><span class="sxs-lookup"><span data-stu-id="83200-255">Customize the role class</span></span>

<span data-ttu-id="83200-256">當實作角色存放裝置提供者，您可以建立自訂角色型別。</span><span class="sxs-lookup"><span data-stu-id="83200-256">When implementing a role storage provider, you can create a custom role type.</span></span> <span data-ttu-id="83200-257">它不需要實作特定介面，但是它必須有`Id`而且通常會有`Name`屬性。</span><span class="sxs-lookup"><span data-stu-id="83200-257">It need not implement a particular interface, but it must have an `Id` and typically it will have a `Name` property.</span></span>

<span data-ttu-id="83200-258">以下是範例角色類別：</span><span class="sxs-lookup"><span data-stu-id="83200-258">The following is an example role class:</span></span>

<span data-ttu-id="83200-259">[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]</span><span class="sxs-lookup"><span data-stu-id="83200-259">[!code-csharp[Main](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]</span></span>

## <a name="customize-the-role-store"></a><span data-ttu-id="83200-260">自訂角色存放區</span><span class="sxs-lookup"><span data-stu-id="83200-260">Customize the role store</span></span>

<span data-ttu-id="83200-261">您可以建立``RoleStore``提供的角色上的所有資料作業方式的類別。</span><span class="sxs-lookup"><span data-stu-id="83200-261">You can create a ``RoleStore`` class that provides the methods for all data operations on roles.</span></span> <span data-ttu-id="83200-262">這個類別就相當於[RoleStore<TRole> ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)類別。</span><span class="sxs-lookup"><span data-stu-id="83200-262">This class is equivalent to the [RoleStore<TRole>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) class.</span></span> <span data-ttu-id="83200-263">在`RoleStore`類別時，實作``IRoleStore<TRole>``並選擇性地``IQueryableRoleStore<TRole>``介面。</span><span class="sxs-lookup"><span data-stu-id="83200-263">In the `RoleStore` class, you implement the ``IRoleStore<TRole>`` and optionally the ``IQueryableRoleStore<TRole>`` interface.</span></span>

- <span data-ttu-id="83200-264">**IRoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="83200-264">**IRoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="83200-265">[IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx)介面會定義角色存放區類別中實作的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-265">The [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) interface defines the methods to implement in the role store class.</span></span> <span data-ttu-id="83200-266">它包含建立、 更新、 刪除及擷取角色的方法。</span><span class="sxs-lookup"><span data-stu-id="83200-266">It contains methods for creating, updating, deleting and retrieving roles.</span></span>
- <span data-ttu-id="83200-267">**RoleStore&lt;TRole&gt;**</span><span class="sxs-lookup"><span data-stu-id="83200-267">**RoleStore&lt;TRole&gt;**</span></span>  
 <span data-ttu-id="83200-268">若要自訂`RoleStore`，建立類別，實作`IRoleStore`介面。</span><span class="sxs-lookup"><span data-stu-id="83200-268">To customize `RoleStore`, create a class that implements the `IRoleStore` interface.</span></span> 

## <a name="reconfigure-app-to-use-new-storage-provider"></a><span data-ttu-id="83200-269">重新設定應用程式使用新的存放裝置提供者</span><span class="sxs-lookup"><span data-stu-id="83200-269">Reconfigure app to use new storage provider</span></span>

<span data-ttu-id="83200-270">一旦您已實作的儲存提供者，您會設定您的應用程式使用它。</span><span class="sxs-lookup"><span data-stu-id="83200-270">Once you have implemented a storage provider, you configure your app to use it.</span></span> <span data-ttu-id="83200-271">如果您的應用程式使用預設提供者，請將它取代您的自訂提供者。</span><span class="sxs-lookup"><span data-stu-id="83200-271">If your app used the default provider, replace it with your custom provider.</span></span>

1. <span data-ttu-id="83200-272">移除`Microsoft.AspNetCore.EntityFramework.Identity`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="83200-272">Remove the `Microsoft.AspNetCore.EntityFramework.Identity` NuGet package.</span></span>
1. <span data-ttu-id="83200-273">如果存放裝置提供者位於不同的專案或封裝中，加入的參考。</span><span class="sxs-lookup"><span data-stu-id="83200-273">If the storage provider resides in a separate project or package, add a reference to it.</span></span>
1. <span data-ttu-id="83200-274">取代所有參考`Microsoft.AspNetCore.EntityFramework.Identity`使用存放裝置提供者的命名空間陳述式。</span><span class="sxs-lookup"><span data-stu-id="83200-274">Replace all references to `Microsoft.AspNetCore.EntityFramework.Identity` with a using statement for the namespace of your storage provider.</span></span>
1. <span data-ttu-id="83200-275">在``ConfigureServices``方法，變更`AddIdentity`方法，以使用您的自訂型別。</span><span class="sxs-lookup"><span data-stu-id="83200-275">In the ``ConfigureServices`` method, change the `AddIdentity` method to use your custom types.</span></span> <span data-ttu-id="83200-276">您可以建立您自己的擴充方法，針對此目的。</span><span class="sxs-lookup"><span data-stu-id="83200-276">You can create your own extension methods for this purpose.</span></span> <span data-ttu-id="83200-277">請參閱[IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs)的範例。</span><span class="sxs-lookup"><span data-stu-id="83200-277">See [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) for an example.</span></span>
1. <span data-ttu-id="83200-278">如果您使用角色時，更新`RoleManager`使用您`RoleStore`類別。</span><span class="sxs-lookup"><span data-stu-id="83200-278">If you are using Roles, update the `RoleManager` to use your `RoleStore` class.</span></span>
1. <span data-ttu-id="83200-279">更新您的應用程式設定的連接字串和認證。</span><span class="sxs-lookup"><span data-stu-id="83200-279">Update the connection string and credentials to your app's configuration.</span></span>

<span data-ttu-id="83200-280">範例：</span><span class="sxs-lookup"><span data-stu-id="83200-280">Example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a><span data-ttu-id="83200-281">參考</span><span class="sxs-lookup"><span data-stu-id="83200-281">References</span></span>

- [<span data-ttu-id="83200-282">ASP.NET 識別的自訂儲存體提供者</span><span class="sxs-lookup"><span data-stu-id="83200-282">Custom Storage Providers for ASP.NET Identity</span></span>](https://docs.microsoft.com/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- <span data-ttu-id="83200-283">[ASP.NET Core 識別](https://github.com/aspnet/identity)-這個儲存機制包含社群維護存放區提供者的連結。</span><span class="sxs-lookup"><span data-stu-id="83200-283">[ASP.NET Core Identity](https://github.com/aspnet/identity) - This repository includes links to community maintained store providers.</span></span>
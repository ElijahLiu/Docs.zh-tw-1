---
title: "使用 ASP.NET Core 和 Visual Studio for Mac 建立 Web API"
author: rick-anderson
description: "使用 ASP.NET Core MVC 和 Visual Studio for Mac 建立 Web API"
keywords: "ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, 服務, HTTP 服務"
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4af5-ed14-1638-7734-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 08619d3b4ab2d6fdb04794dcbafac0b696dd8504
ms.sourcegitcommit: 3273675dad5ac3e1dc1c589938b73db3f7d6660a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="c1487-104">使用 ASP.NET Core MVC 和 Visual Studio for Mac 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="c1487-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="c1487-105">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="c1487-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="c1487-106">在本教學課程中，您將建置 Web API 來管理「待辦事項」項目清單，</span><span class="sxs-lookup"><span data-stu-id="c1487-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="c1487-107">而不會建置 UI。</span><span class="sxs-lookup"><span data-stu-id="c1487-107">You won’t build a UI.</span></span>

<span data-ttu-id="c1487-108">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="c1487-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="c1487-109">macOS：使用 Visual Studio for Mac 建立 Web API (本教學課程)</span><span class="sxs-lookup"><span data-stu-id="c1487-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="c1487-110">Windows：[使用 Visual Studio for Windows 建立 Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="c1487-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="c1487-111">macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="c1487-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="c1487-112">如需使用持續性資料庫的範例，請參閱 [Mac 或 Linux 上的 ASP.NET Core MVC 簡介](xref:tutorials/first-mvc-app-xplat/index)。</span><span class="sxs-lookup"><span data-stu-id="c1487-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1487-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="c1487-113">Prerequisites</span></span>

<span data-ttu-id="c1487-114">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="c1487-114">Install the following:</span></span>

- [<span data-ttu-id="c1487-115">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="c1487-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#macos)  
- [<span data-ttu-id="c1487-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c1487-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="c1487-117">建立專案</span><span class="sxs-lookup"><span data-stu-id="c1487-117">Create the project</span></span>

<span data-ttu-id="c1487-118">從 Visual Studio 中，選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="c1487-118">From Visual Studio, select **File > New Solution**.</span></span>

![macOS 新增方案](first-web-api-mac/_static/sln.png)

<span data-ttu-id="c1487-120">選取 [.NET Core 應用程式] > [ASP.NET Core Web API] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="c1487-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)

<span data-ttu-id="c1487-122">為 [專案名稱] 輸入 **TodoApi**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="c1487-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![設定對話方塊](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="c1487-124">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="c1487-124">Launch the app</span></span>

<span data-ttu-id="c1487-125">在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1487-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="c1487-126">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="c1487-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="c1487-127">您收到 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="c1487-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="c1487-128">將 URL 變更為 `http://localhost:port/api/values`。</span><span class="sxs-lookup"><span data-stu-id="c1487-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="c1487-129">將會顯示 `ValuesController` 資料：</span><span class="sxs-lookup"><span data-stu-id="c1487-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="c1487-130">新增 Entity Framework Core 的支援</span><span class="sxs-lookup"><span data-stu-id="c1487-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="c1487-131">安裝 [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) 資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="c1487-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="c1487-132">此資料庫提供者可讓 Entity Framework Core 搭配使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="c1487-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="c1487-133">從 [專案] 功能表中，選取 [新增 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="c1487-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="c1487-134">或者，您可以用滑鼠右鍵按一下 [相依性]，然後選取 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="c1487-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="c1487-135">在搜尋方塊中輸入 `EntityFrameworkCore.InMemory`。</span><span class="sxs-lookup"><span data-stu-id="c1487-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="c1487-136">選取 `Microsoft.EntityFrameworkCore.InMemory`，然後選取 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="c1487-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="c1487-137">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="c1487-137">Add a model class</span></span>

<span data-ttu-id="c1487-138">模型是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="c1487-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="c1487-139">在此情況下，唯一的模型是待辦事項。</span><span class="sxs-lookup"><span data-stu-id="c1487-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="c1487-140">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c1487-140">Add a folder named *Models*.</span></span> <span data-ttu-id="c1487-141">在方案總管中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="c1487-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="c1487-142">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="c1487-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="c1487-143">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="c1487-143">Name the folder *Models*.</span></span>

![新增資料夾](first-web-api-mac/_static/folder.png)

<span data-ttu-id="c1487-145">注意：您可以將模型類別放在專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c1487-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="c1487-146">新增 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="c1487-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="c1487-147">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。</span><span class="sxs-lookup"><span data-stu-id="c1487-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="c1487-148">將類別命名為 `TodoItem`，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="c1487-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="c1487-149">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="c1487-149">Replace the generated code with:</span></span>

<span data-ttu-id="c1487-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="c1487-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="c1487-151">此資料庫會在建立 `TodoItem` 時產生 `Id`。</span><span class="sxs-lookup"><span data-stu-id="c1487-151">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="c1487-152">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="c1487-152">Create the database context</span></span>

<span data-ttu-id="c1487-153">「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="c1487-153">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="c1487-154">您可以透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立此類別。</span><span class="sxs-lookup"><span data-stu-id="c1487-154">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="c1487-155">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c1487-155">Add a `TodoContext` class to the *Models* folder.</span></span>

<span data-ttu-id="c1487-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="c1487-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="c1487-157">新增控制器</span><span class="sxs-lookup"><span data-stu-id="c1487-157">Add a controller</span></span>

<span data-ttu-id="c1487-158">在方案總管的 *Controllers* 資料夾中新增 `TodoController` 類別。</span><span class="sxs-lookup"><span data-stu-id="c1487-158">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="c1487-159">使用下列程式碼取代產生的程式碼 (並新增右大括弧)：</span><span class="sxs-lookup"><span data-stu-id="c1487-159">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="c1487-160">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="c1487-160">Launch the app</span></span>

<span data-ttu-id="c1487-161">在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1487-161">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="c1487-162">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="c1487-162">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="c1487-163">您收到 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="c1487-163">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="c1487-164">將 URL 變更為 `http://localhost:port/api/values`。</span><span class="sxs-lookup"><span data-stu-id="c1487-164">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="c1487-165">將會顯示 `ValuesController` 資料：</span><span class="sxs-lookup"><span data-stu-id="c1487-165">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="c1487-166">巡覽至位於 `http://localhost:port/api/todo` 的 `Todo` 控制器：</span><span class="sxs-lookup"><span data-stu-id="c1487-166">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="c1487-167">實作其他的 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="c1487-167">Implement the other CRUD operations</span></span>

<span data-ttu-id="c1487-168">我們會將 `Create`、`Update` 和 `Delete` 方法新增至控制器。</span><span class="sxs-lookup"><span data-stu-id="c1487-168">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="c1487-169">這些是佈景主題的變化，因此我只會顯示程式碼，並強調顯示主要的差異。</span><span class="sxs-lookup"><span data-stu-id="c1487-169">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="c1487-170">請在新增或變更程式碼之後建置專案。</span><span class="sxs-lookup"><span data-stu-id="c1487-170">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="c1487-171">建立</span><span class="sxs-lookup"><span data-stu-id="c1487-171">Create</span></span>

<span data-ttu-id="c1487-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="c1487-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="c1487-173">這是 HTTP POST 方法，以 [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) 屬性表示。</span><span class="sxs-lookup"><span data-stu-id="c1487-173">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) attribute.</span></span> <span data-ttu-id="c1487-174">[`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="c1487-174">The [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="c1487-175">`CreatedAtRoute` 方法會傳回 201 回應，這是 HTTP POST 方法的標準回應，可在伺服器上建立新的資源。</span><span class="sxs-lookup"><span data-stu-id="c1487-175">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="c1487-176">`CreatedAtRoute` 也會將位置標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="c1487-176">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="c1487-177">位置標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="c1487-177">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="c1487-178">請參閱 [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。</span><span class="sxs-lookup"><span data-stu-id="c1487-178">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="c1487-179">使用 Postman 來傳送建立要求</span><span class="sxs-lookup"><span data-stu-id="c1487-179">Use Postman to send a Create request</span></span>

* <span data-ttu-id="c1487-180">啟動應用程式 ([執行] > [啟動並偵錯])。</span><span class="sxs-lookup"><span data-stu-id="c1487-180">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="c1487-181">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="c1487-181">Start Postman.</span></span>

![Postman 主控台](first-web-api/_static/pmc.png)

* <span data-ttu-id="c1487-183">將 HTTP 方法設定為 `POST`</span><span class="sxs-lookup"><span data-stu-id="c1487-183">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="c1487-184">選取 [主體] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="c1487-184">Select the **Body** radio button</span></span>
* <span data-ttu-id="c1487-185">選取 [原始] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="c1487-185">Select the **raw** radio button</span></span>
* <span data-ttu-id="c1487-186">將類型設定為 JSON</span><span class="sxs-lookup"><span data-stu-id="c1487-186">Set the type to JSON</span></span>
* <span data-ttu-id="c1487-187">在索引鍵-值編輯器中，輸入待辦事項，例如</span><span class="sxs-lookup"><span data-stu-id="c1487-187">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="c1487-188">選取 [傳送]</span><span class="sxs-lookup"><span data-stu-id="c1487-188">Select **Send**</span></span>

* <span data-ttu-id="c1487-189">選取下方窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭：</span><span class="sxs-lookup"><span data-stu-id="c1487-189">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmget.png)

<span data-ttu-id="c1487-191">您可以使用 [位置] 標頭 URI 來存取您剛才建立的資源。</span><span class="sxs-lookup"><span data-stu-id="c1487-191">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="c1487-192">請重新叫用建立 `"GetTodo"` 具名路由的 `GetById` 方法：</span><span class="sxs-lookup"><span data-stu-id="c1487-192">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="c1487-193">更新</span><span class="sxs-lookup"><span data-stu-id="c1487-193">Update</span></span>

<span data-ttu-id="c1487-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="c1487-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="c1487-195">`Update` 類似於 `Create`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="c1487-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="c1487-196">回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="c1487-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="c1487-197">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。</span><span class="sxs-lookup"><span data-stu-id="c1487-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="c1487-198">若要支援部分更新，請使用 HTTP PATCH。</span><span class="sxs-lookup"><span data-stu-id="c1487-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="c1487-200">刪除</span><span class="sxs-lookup"><span data-stu-id="c1487-200">Delete</span></span>

<span data-ttu-id="c1487-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="c1487-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="c1487-202">回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="c1487-202">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="c1487-204">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c1487-204">Next steps</span></span>

* [<span data-ttu-id="c1487-205">傳送至控制器動作</span><span class="sxs-lookup"><span data-stu-id="c1487-205">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="c1487-206">如需部署 API 的資訊，請參閱[發行和部署](../publishing/index.md)。</span><span class="sxs-lookup"><span data-stu-id="c1487-206">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* [<span data-ttu-id="c1487-207">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="c1487-207">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [<span data-ttu-id="c1487-208">Postman</span><span class="sxs-lookup"><span data-stu-id="c1487-208">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="c1487-209">Fiddler</span><span class="sxs-lookup"><span data-stu-id="c1487-209">Fiddler</span></span>](http://www.fiddler2.com/fiddler2/)
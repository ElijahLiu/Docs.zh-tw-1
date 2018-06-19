---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: 將驗證加入至模型 (VB) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 86058530aa00ecbc00aeebc6ed7b5cf019fdad72
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872919"
---
<a name="adding-validation-to-the-model-vb"></a>將驗證加入至模型 (VB)
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。 開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：
> 
> - [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）
> 
> 如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。
> 
> 使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。 [下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。 如果您偏好 C#，切換至[C# 版本](../cs/adding-validation-to-the-model.md)本教學課程。


這一節中，您會將驗證邏輯加入`Movie`模型，而您要確保驗證規則會強制執行任何使用者嘗試建立或編輯電影使用應用程式的時間。

## <a name="keeping-things-dry"></a>保留項目乾

其中一個核心設計原則，ASP.NET mvc 是乾 （「 不重複自行"）。 ASP.NET MVC 鼓勵您一次，指定的功能或行為，然後讓它 everywhere 反映應用程式中。 這可減少您需要撰寫程式碼數量，並讓您更容易撰寫維護的程式碼。

ASP.NET MVC 和 Entity Framework Code First 所提供的驗證支援是在動作中的乾主體的絕佳範例。 您可以以宣告方式指定在單一位置 （以模型類別） 的驗證規則，然後這些規則會強制執行所有應用程式中。

讓我們看看如何您可以利用這項驗證支援的電影應用程式中。

## <a name="adding-validation-rules-to-the-movie-model"></a>將驗證規則加入至電影模型

藉由新增一些驗證邏輯，以開始，您將`Movie`類別。

開啟*Movie.vb*檔案。 新增`Imports`在參考的檔案最上方的陳述式[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間：

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

命名空間是.NET Framework 的一部分。 它提供一組內建的驗證屬性，您可以以宣告方式套用至任何類別或屬性。

立即更新`Movie`類別，以充分利用內建[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，和[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)驗證屬性. 使用下列程式碼做為要套用屬性的範例。

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

驗證屬性會指定您想要強制執行模型屬性套用的行為。 `Required`屬性會指出屬性必須有一個值; 在此範例中，影片必須有值`Title`， `ReleaseDate`， `Genre`，和`Price`屬性才會生效。 `Range` 屬性會將值限制在指定的範圍內。 `StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。

程式碼第一次可確保您的模型類別指定的驗證規則會強制執行之前會先應用程式資料庫中儲存的變更。 例如，下列程式碼將會擲回例外狀況時`SaveChanges`呼叫方法，因為數個需要`Movie`屬性值為遺失，價格為零 （亦即超出有效範圍）。

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

需要.NET framework 自動強制執行的驗證規則可協助讓您的應用程式更穩固。 它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。

以下是完整的程式碼已更新*Movie.vb*檔案：

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>驗證錯誤 ASP.NET MVC 中的 UI

重新執行應用程式，並瀏覽至 */Movies* URL。

按一下**建立電影**連結加入新的電影。 填寫表單具有一些無效的值，然後按一下 [**建立**] 按鈕。

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

請注意如何表單已自動使用背景色彩來反白顯示包含無效的資料，以及發出適當的驗證錯誤訊息，每個旁邊的文字方塊。 錯誤訊息會比對您指定當您的註解錯誤字串`Movie`類別。 錯誤會強制執行 （使用 JavaScript） 用戶端和伺服器端 （如果使用者已停用 JavaScript）。

實際的好處是： 您不需要變更單一行中的程式碼`MoviesController`類別或*Create.vbhtml*才能啟用這項驗證 UI 的檢視。 控制站建立和檢視您稍早在本教學課程中會自動挑選驗證規則，您會指定使用屬性的總`Movie`模型類別。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>如何驗證會在中建立檢視，並建立動作方法

您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。 下一步的清單顯示什麼`Create`方法`MovieController`類別看起來像。 變更就會維持與您建立的方式加以稍早在本教學課程。

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

第一個動作方法會顯示在初始建立表單。 第二個處理表單 post。 第二個`Create`方法呼叫`ModelState.IsValid`，檢查電影是否有任何驗證錯誤。 呼叫此方法會評估已套用至物件的所有驗證屬性。 如果物件有驗證錯誤`Create`方法重新顯示表單。 如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。

以下是*Create.vbhtml*您稍早在本教學課程中建立結構的檢視範本。 上述動作方法會使用它來顯示初始表單，以及在發生錯誤時重新顯示它。

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

請注意程式碼如何使用`Html.EditorFor`輸出的協助專家`<input>`每個項目`Movie`屬性。 此協助程式旁是呼叫`Html.ValidationMessageFor`helper 方法。 這兩個 helper 方法會使用控制器的傳遞至檢視的模型物件 (在此情況下，`Movie`物件)。 它們會自動尋找適當的模型並顯示錯誤訊息中指定的驗證屬性。

這種方法的優點是，控制器和建立檢視表範本都不知道的任何項目有關強制執行的實際驗證規則或顯示的特定錯誤訊息。 驗證規則和錯誤字串只在 `Movie` 類別中指定。

如果您想要稍後變更驗證邏輯，您可以在一個地方。 您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。 這會讓程式碼非常整齊乾淨，容易維護及發展。 這表示您會完全接受 DRY 原則。

## <a name="adding-formatting-to-the-movie-model"></a>加入影片模型格式設定

開啟*Movie.vb*檔案。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間提供除了一組內建的驗證屬性的格式屬性。 您將套用[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性和[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)發行日期和價格欄位的列舉值。 下列程式碼會示範`ReleaseDate`和`Price`具有適當的屬性[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性。

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

或者，您也可以明確設定[ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)值。 下列程式碼將示範使用日期的格式字串的發行日期屬性 (也就是"d")。 您會使用這個指定不希望時間做為發行日期的一部分。

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

下列程式碼格式`Price`屬性做為貨幣。

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

完整`Movie`類別如下所示。

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

執行應用程式，並瀏覽至`Movies`控制站。

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

在數列的下一個部分中，我們會檢閱應用程式，並進行一些改良的自動產生`Details`和`Delete`方法...

> [!div class="step-by-step"]
> [上一頁](adding-a-new-field.md)
> [下一頁](improving-the-details-and-delete-methods.md)

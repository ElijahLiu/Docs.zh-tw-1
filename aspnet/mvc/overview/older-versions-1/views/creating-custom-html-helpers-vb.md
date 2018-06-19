---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: 建立自訂 HTML Helper (VB) |Microsoft 文件
author: microsoft
description: 本教學課程的目標是為了示範如何建立您可以使用 MVC 檢視中的自訂 HTML Helper。 利用 HTML Helper...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 6980026e2653eacb71697f9b34def9bc38638726
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871502"
---
<a name="creating-custom-html-helpers-vb"></a>建立自訂 HTML Helper (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> 本教學課程的目標是為了示範如何建立您可以使用 MVC 檢視中的自訂 HTML Helper。 利用 HTML Helper，您可以減少的冗長的輸入，您必須執行建立標準的 HTML 網頁的 HTML 標記。


本教學課程的目標是為了示範如何建立您可以使用 MVC 檢視中的自訂 HTML Helper。 利用 HTML Helper，您可以減少的冗長的輸入，您必須執行建立標準的 HTML 網頁的 HTML 標記。

在本教學課程的第一個部分中，我會說明一些現有的 HTML Helper 內含在 ASP.NET MVC framework。 我接下來，描述建立自訂的 HTML Helper 的兩個方法： 我說明如何建立自訂的 HTML Helper，藉由建立的共用的方法並建立擴充方法。

## <a name="understanding-html-helpers"></a>了解 HTML Helper

HTML Helper 是只會傳回字串的方法。 字串可以代表任何一種您想要的內容。 例如，您可以使用 HTML Helper 來呈現標準的 HTML 標記，例如 HTML`<input>`和`<img>`標記。 您也可以使用 HTML Helper 來呈現更複雜的內容，例如索引標籤區域或資料庫資料的 HTML 表格。

ASP.NET MVC 架構包括下列的標準 （這不是完整的清單） 的 HTML Helper 集：

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

例如，請考慮列出 1 中的表單。 此表單會呈現兩個標準的 HTML Helper （請參閱圖 1） 的協助。 這個表單用`Html.BeginForm()`和`Html.TextBox()`Helper 方法。


[![使用 HTML Helper 呈現頁面](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**圖 01**： 以 HTML Helper 呈現的頁面 ([按一下以檢視完整大小的影像](creating-custom-html-helpers-vb/_static/image3.png))


**列出 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

`Html.BeginForm()` Helper 方法用來建立的開頭和結尾 HTML`<form>`標記。 請注意，`Html.BeginForm()`方法呼叫內的 using 陳述式。 使用陳述式時，可確保`<form>`標記取得關閉結尾所使用的區塊。

如果想要的話，而不是建立在 using 區塊中，您可以呼叫 Html.EndForm() Helper 方法，以關閉`<form>`標記。 使用何種方法來建立開啟和關閉`<form>`看起來您最直覺的標記。

`Html.TextBox()` Helper 方法會在程式碼範例 1 中用來呈現 HTML`<input>`標記。 如果您選取檢視來源瀏覽器中您會看到列出 2 中的 HTML 原始檔。 請注意，來源就會包含標準的 HTML 標記。

> [!IMPORTANT]
> 請注意， `Html.TextBox()`HTML Helper 呈現與`<%= %>`標記，而不是`<% %>`標記。 如果您不想加入等號，然後執行任何動作取得瀏覽器中呈現。

ASP.NET MVC framework 會包含較少的協助程式。 最可能的原因，您必須擴充 MVC 架構，以自訂 HTML Helper。 在本教學課程的其餘部分，您會學習建立自訂的 HTML Helper 的兩個方法。

**列出 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>建立使用共用方法的 HTML Helper

若要建立新的 HTML Helper 的最簡單方式是建立共用的方法會傳回字串。 例如，假設，您決定要建立新的 HTML Helper 呈現 HTML`<label>`標記。 您可以列出 2 中使用類別來呈現`<label>`。

**列出 2 – `Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

沒有任何特殊有關列表 2 中的類別。 `Label()`方法只會傳回字串。

使用修改的索引檢視表中列出的 3`LabelHelper`來呈現 HTML`<label>`標記。 請注意，檢視包含`<%@ imports %>`匯入 Application1.Helpers 命名空間的指示詞。

**列出 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>建立具有擴充方法的 HTML Helper

如果您想要建立只使用的 HTML Helper，例如標準包含在 ASP.NET MVC 架構，則您需要建立擴充方法的 HTML Helper。 擴充方法可讓您將新方法加入至現有的類別。 建立時的 HTML Helper 方法，您將新方法新增至`HtmlHelper`檢視的 Html 屬性所代表的類別。

Visual Basic 模組中列出的 3 新增名為擴充方法`Label()`至`HtmlHelper`類別。 有幾個您應該注意到此模組的相關的東西。 首先，請注意此模組以裝飾`<Extension()>`屬性。 若要使用這個屬性，您必須匯入`System.Runtime.CompilerServices`命名空間

第二，請注意，第一個參數`Label()`方法代表`HtmlHelper`類別。 擴充方法的第一個參數會指出延伸模組方法所擴充的類別。

**列出 3 – `Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

您建立擴充方法，並建置您的應用程式成功後，擴充方法會出現在 Visual Studio Intellisense，如同所有其他類別的方法 （請參閱圖 2）。 唯一的差別是該方法會出現以特殊符號旁邊 （向下箭號圖示） 的擴充功能。


[![使用 Html.Label() 擴充方法](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**圖 02**: Html.Label() 擴充方法 ([按一下以檢視完整大小的影像](creating-custom-html-helpers-vb/_static/image6.png))


修改的索引檢視表中列出的 4 會使用 Html.Label() 擴充方法的所有呈現其&lt;標籤&gt;標記。

**列出 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>總結

在本教學課程中，您將學會建立自訂的 HTML Helper 的兩個方法。 首先，您學到如何建立自訂`Label()`HTML Helper 所建立的共用的方法傳回的字串。 接下來，您學到如何建立自訂`Label()`上建立擴充方法的 HTML Helper 方法`HtmlHelper`類別。

在本教學課程中，我會著重於建立非常簡單的 HTML Helper 方法。 請注意，HTML Helper 就是您想要一樣複雜。 您可以建置豐富的內容，例如樹狀檢視、 功能表、 或資料表的資料庫資料的呈現的 HTML Helper。

> [!div class="step-by-step"]
> [上一頁](asp-net-mvc-views-overview-vb.md)
> [下一頁](using-the-tagbuilder-class-to-build-html-helpers-vb.md)

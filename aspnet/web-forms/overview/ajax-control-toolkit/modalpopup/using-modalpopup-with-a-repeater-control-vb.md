---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: 使用 ModalPopup 與中繼器控制項 (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 此外，也可以使用這個 contr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 04e3b132d1de2f42ba5de113dfbc22c85c3b198e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870917"
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a>使用 ModalPopup 與中繼器控制項 (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)

> AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 它也可使用這個控制項內的重複項。


## <a name="overview"></a>總覽

AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 它也可使用這個控制項內的重複項。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。 此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。 如果您的設定不同，您必須調整資料庫的連接資訊。 為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

然後，將資料來源加入至頁面。 若要使用有限的資料，我們只選取前五個項目 Vendor 資料表的 AdventureWorks 資料庫中。 如果您使用 Visual Studio 小幫手來建立資料來源，請記住，請在目前版本中的 bug 不前置資料表名稱 (`Vendor`) 與`Purchasing`。 下列標記會顯示正確的語法：

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

接著，將會做為強制回應的快顯視窗的面板。 它包含`Button`再次關閉快顯視窗的控制項：

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

為了讓快顯視窗在中繼器工作`ModalPopupExtender`控制項必須置於`<ItemTemplate>`中繼器的區段。 因此面板超出中繼器，但 extender 內。 以下是中繼器的標記：

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

然後，會觸發強制回應的快顯的按鈕旁邊會顯示資料來源中的每個項目。


[![強制回應的快顯視窗可能觸發的每個資料來源項目](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)

強制回應的快顯視窗可能觸發的每個資料來源項目 ([按一下以檢視完整大小的影像](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](launching-a-modal-popup-window-from-server-code-vb.md)
> [下一頁](handling-postbacks-from-a-modalpopup-vb.md)

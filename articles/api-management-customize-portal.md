﻿<properties 
	pageTitle="在 Azure API 管理中自訂開發人員入口網站" 
	description="在 Azure API 管理中自訂開發人員入口網站。" 
	services="api-management" 
	documentationCenter="" 
	authors="steved0x" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="api-management" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/18/2014" 
	ms.author="sdanie"/>

# 在 Azure API 管理中自訂開發人員入口網站

本指南示範如何在 API 管理中修改開發人員入口網站的外觀及操作，以便與您的品牌一致。

## 本主題內容

-	[變更頁首的文字/標誌][]
-	[變更頁首的樣式][]
-	[編輯頁面的內容][]
-	[後續步驟][]


## <a name="change-page-headers"> </a>變更頁首的文字/標誌

入口網站自訂的一個主要層面，便是將所有頁面頂端的文字以您的公司名稱或標誌取代。

開發人員入口網站內的內容，是透過經 Azure 管理入口網站存取的發行者入口網站加以修改。若要連線至「API 管理」主控台，請在「Azure 入口網站」中按一下您「API 管理」服務的 [管理主控台]****。

![Management console][api-management-management-console]

開發人員入口網站是以「內容管理系統」(CMS) 為基礎。每個頁面上出現的頁首是特殊類型的內容，也稱為 Widget。若要編輯該 Widget 的內容，請從左側的 [開發人員入口網站]**** 功能表中按一下 [Widget]****，然後從清單中選取 [頁首]**** Widget。

![Widgets header][api-management-widgets-header]

頁首的內容可在 [本文]**** 欄位內進行編輯。將文字變更為 "Fabrikam Developer Portal"，然後按一下頁面底端的 [儲存]****。

現在，您應該會在開發人員入口網站內的每個頁面上看到新的頁首！

> 在管理主控台中時，若要開啟開發人員入口網站，請按一下頂端列中的 [開發人員入口網站]****。

## <a name="change-headers-styling"> </a>變更頁首的樣式

入口網站上任何頁面的色彩、字型、大小、空格和其他樣式相關的元素，會透過樣式規則加以定義。若要編輯樣式，請在發行者入口網站的 [開發人員入口網站]**** 功能表上按一下 [外觀]****。然後按一下 [開始自訂]**** 來啟用樣式編輯器。

您的瀏覽器將在開發人員入口網站內切換為隱藏頁面，其中包含了內容範例，以及可在網站任意處使用之所有樣式規則的範例。若要開啟樣式編輯器，請將游標移至頁面最左側的細長灰色垂直線上。應該會出現編輯器工具列。

![Customization toolbar][api-management-customization-toolbar]

編輯樣式規則有兩種主要模式 - [編輯所有規則]**** 會顯示在任何一處使用之所有樣式規則的清單，而 [挑選元素]**** 可讓您從所在頁面選取元素，並且只會顯示該元素的樣式。

在本節中，我們只想變更頁首的樣式。從樣式編輯器工具列中按一下 [挑選元素]****選項，然後按一下 [選取要自訂的元素]****。現在，當您將滑鼠移至元素上方，元素會變成反白顯示的狀態，以象徵在您按一下時會開始編輯的元素樣式。將滑鼠移至頁首中代表公司名稱的文字上 (如果您遵循上一節中的指示，即為 "Fabrikam Developer Portal")，然後按一下該文字。一組已具名且分類的樣式規則將出現在樣式編輯器中。

每個規則均代表所選取元素的樣式屬性。例如，針對以上選取的頁首文字，文字的大小在 @font-size-h1，而替代字型的名稱則在 @headings-font-family。

> 如果您熟悉[啟動程序][]，這些規則實際上就是開發人員入口網站所使用之啟動程序佈景主題內的 [LESS 變數][]。

讓我們變更頁首文字的色彩。選取 [@headings-color]**** 欄位中的項目，然後輸入 #000000。這是黑色的十六進位碼。執行這項作業，您會看見文字方塊末端出現方形色彩指示器。如果您按一下該指示器，將出現色彩選擇器，供您選擇色彩。

![Color picker][api-management-customization-toolbar-color-picker]

完成變更所選元素的樣式時，請按一下 [預覽變更]****，以在畫面上查看結果。目前只有管理員可看見這些結果。若要讓每個人都可看見這些變更，請在樣式編輯器中按一下 [發行]**** 按鈕並確認變更。

![Publish menu][api-management-customization-toolbar-publish-form]

> 若要變更套用至頁面上任何其他元素的樣式規則，請依照您對頁首所進行的相同程序 - 從樣式編輯器中按一下 [挑選元素]****，選取您感興趣的元素，然後開始修改畫面上顯示之樣式規則的值。

## <a name="edit-page-contents"> </a>編輯頁面的內容

開發人員入口網站是由自動產生的頁面組成，像是 API、產品、應用程式、問題和手動編寫的內容。由於它是以內容管理系統為基礎，所以您可以在必要時建立這類內容。

若要查看所有現有內容頁面的清單，請在管理主控台的 [開發人員入口網站]**** 功能表上按一下 [內容]****。

![Manage content][api-management-customization-manage-content]

按一下 [歡迎使用] 頁面，編輯要在開發人員入口網站的首頁上顯示的內容。進行您想要做的變更，視需要預覽，然後按一下 [立即發行]****，讓每個人都可看見這些變更。

> 首頁使用特殊的版面配置，使其得以在上方顯示橫幅。您無法透過 [內容] 區段編輯橫幅。若要編輯此橫幅，請從 [開發人員入口網站]**** 功能表中按一下 [Widget]****，然後從 [目前圖層]**** 下拉式清單中選取 [首頁]****，接著開啟 [特色] 區段下的 [橫幅]**** 項目。如同其他任何頁面一般，此 Widget 的內容可供編輯。

## <a name="next-steps"> </a>後續步驟

-	查看[開始使用進階 API 組態][]教學課程中的其他主題。

[變更頁首的文字/標誌]: #change-page-headers
[變更頁首的樣式]: #change-headers-styling
[編輯頁面的內容]: #edit-page-contents
[後續步驟]: #next-steps

[管理入口網站]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[開始使用進階 API 組態]: ../api-management-get-started-advanced
[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/

<!--HONumber=35.2-->

<!--HONumber=46--> 

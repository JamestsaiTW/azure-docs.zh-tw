---
title: 在 Azure 入口網站中建立 Azure 搜尋服務索引 - Azure 搜尋服務
description: 了解如何使用內建入口網站索引設計工具，為 Azure 搜尋服務建立索引。
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6a2bac71c37cc750eb24e3492ecdcdf0b2333cce
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338830"
---
# <a name="create-an-azure-search-index-in-the-portal"></a>在入口網站中建立 Azure 搜尋服務索引

Azure 搜尋服務在入口網站中包含適用於原型的內建索引設計工具，或可供建立在您的 Azure 搜尋服務上裝載的[搜尋索引](search-what-is-an-index.md)。 此工具使用於結構描述建構。 當您儲存定義時，空的索引就會變成完全以 Azure 搜尋服務表示。 您將它與可搜尋資料一起載入的方式，由您決定。

索引設計工具只是建立索引的一種方法。 以程式設計方式，您可使用 [.NET](search-create-index-dotnet.md) 或 [REST](search-create-index-rest-api.md) API 建立索引。

## <a name="start-index-designer"></a>啟動索引設計工具

1. 登入 [Azure 入口網站](https://portal.azure.com)並開啟服務儀表板。 您可以按一下導向列中的 [所有服務] 以搜尋目前訂用帳戶中的現有「搜尋服務」。 

2. 在頁面頂端的命令列中，按一下 [新增索引] 連結。

   ![命令列中的 [新增索引] 連結](media/search-create-index-portal/add-index.png "命令列中的 [新增索引] 連結")

3. 為 Azure 搜尋服務索引命名。 在編製索引和查詢作業中會參考索引名稱。 索引名稱會成為連至索引之連線上所使用端點 URL 的一部分，並用來在 Azure 搜尋服務 REST API 中傳送 HTTP 要求。

   * 以字母開頭。
   * 僅使用小寫字母、數字或連字號 ("-")。
   * 將名稱長度限制為 60 個字元。

## <a name="add-fields"></a>新增欄位

索引組合包含「欄位集合」，用以定義索引中可搜尋的資料。 總之，Fields 集合會指定您分別上傳的文件結構。 Fields 集合包含必要與選用欄位 (具名與具類型)，以及用來判斷如何使用欄位的索引屬性。

1. 新增欄位以完整指定您要上傳的文件，為每個都設定[資料類型](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) \(英文\)。 例如，如果文件包含 *hotel-id*、*hotel-name*、*address*、*city* 及 *region*，請為每一個在索引中建立對應的欄位。 請檢閱[下一節中的設計指導方針](#design)，以取得設定屬性的說明。

2. 指定 Edm.String 類型的 *key* 欄位。 此欄位的值必須唯一地識別每個文件。 根據預設，已將該欄位命名為 id，但您可以將它重新命名，只要字串符合[命名規則](https://docs.microsoft.com/rest/api/searchservice/Naming-rules)即可。 例如，如果您的欄位集合包含 *hotel-id*，您就會針對您的索引鍵選擇它。 對於每個 Azure 搜尋服務索引而言，索引鍵欄位是必要的且必須為字串。

3. 在每個欄位上設定屬性。 索引設計工具會排除對資料類型為無效的任何屬性，但不會建議要包含哪些。 請檢閱下一節中的指導方針，以了解有哪些屬性及其用途為何。

    Azure 搜尋服務 API 文件包含具有簡單 hotels 索引功能的程式碼範例。 在以下的螢幕擷取畫面中，您可以看到索引定義，包括在索引定義期間指定的法文語言分析器，而您可以在入口網站中重新建立以當作練習。

    ![旅館示範索引](media/search-create-index-portal/field-definitions.png "旅館示範索引")

4. 完成後，按一下 [建立]，以儲存並建立索引。

<a name="design"></a>

## <a name="set-attributes"></a>設定屬性

雖然您可以隨時新增欄位，但現有的欄位定義會在索引的存留期間加以鎖定。 基於這個理由，開發人員通常會使用入口網站來建立簡單的索引、測試概念，或使用管理入口網站頁面來查詢設定。 如果您遵循以程式碼為基礎的方法，則整個索引設計中頻繁的反覆動作就會更有效率，如此便能讓您輕鬆地重建索引。

系統會先將分析器與建議工具關聯至欄位，然後再儲存索引。 在建立索引定義時，請務必加入語言分析器或建議工具。

字串欄位通常會標示為 [可搜尋] 和 [可擷取]。 用來縮小搜尋結果的欄位包括 [可排序]、[可篩選] 及 [可 Facet]。

欄位屬性會決定欄位的使用方式，例如，是否將它用於全文檢索搜尋、多面向導覽、排序作業等。 下表描述每個屬性。

|屬性|說明|  
|---------------|-----------------|  
|**searchable**|可進行全文檢索搜尋，受限於語彙分析，例如，在編製索引期間執行斷字功能。 如果您為可搜尋的欄位設定像是「sunny day」的值，則系統會在內部將它分割為「sunny」和「day」這兩個個別的語彙基元。 如需詳細資訊，請參閱[全文檢索搜尋如何運作](search-lucene-query-architecture.md)。|  
|**filterable**|在 **$filter** 查詢中加以參考。 類型 `Edm.String` 或 `Collection(Edm.String)` 的可篩選欄位不會執行斷字功能，因此，只會針對完全相符的項目進行比較。 例如，如果您將欄位 f 之類的欄位設為「sunny day」，`$filter=f eq 'sunny'` 就會找不到相符項目，但 `$filter=f eq 'sunny day'` 可以。 |  
|**sortable**|根據預設，系統會依分數來排序結果，但您可根據文件中的欄位設定排序。 類型 `Collection(Edm.String)` 的欄位不能是 **sortable**。 |  
|**facetable**|通常用來呈現搜尋結果，其中包含依類別的叫用次數 (例如，特定城市中的旅館)。 此選項無法與類型 `Edm.GeographyPoint`的欄位搭配使用。 類型為 `Edm.String` 的欄位 (其為 **filterable**、**sortable** 或 **facetable**) 長度最多可達 32 KB。 如需詳細資訊，請參閱[建立索引 (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index)。|  
|**key**|索引內文件的唯一識別碼。 您必須只選擇一個欄位作為索引鍵欄位，而它的類型必須是 `Edm.String`。|  
|**retrievable**|判斷搜尋結果中是否會傳回該欄位。 當您想要使用某個欄位 (例如 profit margin) 作為篩選、排序或評分機制，但不想讓使用者看見該欄位時，這非常有用。 針對 `true` for `key` 。|  

## <a name="next-steps"></a>後續步驟

建立 Azure 搜尋服務索引之後，您可以移到下一個步驟：[將可搜尋的資料上傳至索引](search-what-is-data-import.md)。

或者，您可能也需要[深入探討索引](search-what-is-an-index.md)。 除了欄位集合之外，索引也會指定分析器、建議工具、評分設定檔和 CORS 設定。 入口網站提供索引標籤頁面來定義最常見的元素：欄位、分析器及建議工具。 若要建立或修改其他元素，您可以使用 REST API 或 .NET SDK。

## <a name="see-also"></a>另請參閱

 [全文檢索搜尋如何運作](search-lucene-query-architecture.md)  
 [搜尋服務 REST API](https://docs.microsoft.com/rest/api/searchservice/) [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)


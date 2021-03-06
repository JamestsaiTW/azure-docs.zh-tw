---
title: 為 Azure Blob 索引子的 JSON Blob 編製索引以用於全文檢索搜尋 - Azure 搜尋服務
description: 使用 Azure 搜尋服務 Blob 索引子，搜耙文字內容的 Azure JSON Blob。 索引子可為選取的資料來源 (例如 Azure Blob 儲存體) 將資料擷取自動化。
ms.date: 02/28/2019
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: d70ad65f5bbc4424b4224cf601d903ad7ec10691
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57405108"
---
# <a name="how-to-index-json-blobs-using-azure-search-blob-indexer"></a>如何使用 Azure 搜尋服務 Blob 索引子編製 JSON blob
本文說明如何設定 Azure 搜尋服務 blob [indexer](search-indexer-overview.md)擷取 Azure Blob 儲存體中的 JSON 文件中的結構化的內容，並使其可在 Azure 搜尋服務。 此工作流程建立 Azure 搜尋服務索引，並將其載入具有現有從 JSON blob 擷取的文字。 

您可以使用[入口網站](#json-indexer-portal)、[REST API](#json-indexer-rest) 或 [.NET SDK](#json-indexer-dotnet) 為 JSON 內容編製索引。 通用的方法都是 JSON 文件位於 Azure 儲存體帳戶中的 blob 容器。 如需如何從其他非 Azure 平台推送 JSON 文件的指引，請參閱 [Azure 搜尋服務中的資料匯入](search-what-is-data-import.md)。

Azure Blob 儲存體中的 JSON blob 通常是單一 JSON 文件或 JSON 實體的集合。 對於 JSON 的集合，blob 可能**陣列**的語式正確的 JSON 元素。 Blob 也無法組成多個個別的 JSON 實體以新行字元分隔。 在 Azure 搜尋服務 blob 索引子可以剖析這類結構，取決於您設定的方式**parsingMode**要求的參數。

> [!IMPORTANT]
> `json` 並`jsonArray`剖析模式已公開推出，但`jsonLines`剖析模式處於公開預覽狀態，和不應在生產環境中。 如需詳細資訊，請參閱 [REST api-version=2017-11-11-Preview](search-api-2017-11-11-preview.md)。 

> [!NOTE]
> 請依照下列中的索引子組態建議[-一對多編製索引](search-howto-index-one-to-many-blobs.md)輸出從一個 Azure blob 的多個搜尋文件。

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>使用入口網站

為 JSON 文件索引編製時，最簡單的方法是使用 [Azure 入口網站](https://portal.azure.com/)中的精靈。 藉由剖析 Azure Blob 容器中的中繼資料，[**匯入資料**](search-import-data-portal.md)精靈可以建立預設索引、將來源欄位對應至目標索引欄位，並在單一作業中載入索引。 根據來源資料的大小和複雜度，只需要幾分鐘的時間，您就可以擁有正常運作的全文檢索搜尋索引。

我們建議使用相同的 Azure 訂用帳戶的 Azure 搜尋服務和 Azure 儲存體，最好位於相同的區域。

### <a name="1---prepare-source-data"></a>1 - 準備來源資料

請準備 Azure 儲存體帳戶，此帳戶必須有 Blob 儲存體和 JSON 文件容器。 如果您不熟悉這些需求，檢閱 「 設定 Azure Blob 服務和載入範例資料 」 中[認知搜尋-快速入門](cognitive-search-quickstart-blob.md#set-up-azure-blob-service-and-load-sample-data)。

> [!Important]
> 在容器中，務必**公用存取層級**設為 「 容器 （匿名讀取權限適用於容器和 blob） 」。 Azure 儲存體和 Azure 搜尋服務應位於相同的訂用帳戶，且可能的話，請在相同的區域中。 

### <a name="2---start-import-data-wizard"></a>2 - 啟動匯入資料精靈

您可以從 Azure 搜尋服務頁面中的命令列[啟動精靈](search-import-data-portal.md)，也可以藉由在儲存體帳戶左側瀏覽窗格的 [Blob 服務] 區段中按一下 [新增 Azure 搜尋服務] 來啟動。

   ![在入口網站匯入資料命令](./media/search-import-data-portal/import-data-cmd2.png "啟動匯入資料精靈")

### <a name="3---set-the-data-source"></a>3 - 設定資料來源

在 [資料來源] 頁面中，來源必須是 **Azure Blob 儲存體**，並具有下列規格：

+ [要擷取的資料] 應該是 [內容和中繼資料]。 選擇此選項可讓精靈推斷索引結構描述，並對應要匯入的欄位。
   
+ **剖析模式**應該設定為*JSON*， *JSON 陣列*或是*JSON 行*。 

  [JSON] 會將每個 Blob 清楚表達為單一搜尋文件，而在搜尋結果中顯示為獨立項目。 

  *JSON 陣列*並對應至物件的陣列，包含語式正確的 JSON 資料集的語式正確的 JSON blob，或有一個屬性，也就是物件的陣列，而您想要獨立、 獨立的搜尋服務文件形式來聯結的每個項目。 如果 Blob 很複雜，而且您未選擇 [JSON 陣列]，則整個 Blob 會擷取為單一文件。

  *JSON 行*是您要用來做為獨立獨立搜尋文件來聯結的每個實體的 blob 是由多個新行，以分隔的 JSON 實體所組成。 如果 blob 很複雜，而且您未選擇*JSON 行*剖析模式，然後將整個 blob 資料會內嵌為單一文件。
   
+ [儲存體容器] 必須指定儲存體帳戶和容器，或是會解析為容器的連接字串。 您可以在 Blob 服務的入口網站頁面上取得連接字串。

   ![Blob 資料來源定義](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - 略過精靈的 [新增認知搜尋] 頁面

您並不需要新增認知技術就能匯入 JSON 文件。 除非您有特殊需要，而必須將[認知服務 API 和轉換納入](cognitive-search-concept-intro.md)編製索引管線，否則請略過此步驟。

若要略過此步驟，先移至下一個頁面。

   ![認知搜尋的下一頁按鈕](media/search-get-started-portal/next-button-add-cog-search.png)

從該頁面就可以跳至索引自訂。

   ![跳過認知技術步驟](media/search-get-started-portal/skip-cog-skill-step.png)

### <a name="5---set-index-attributes"></a>5 - 設定索引屬性

在 [索引] 頁面上，您應該會看到欄位清單，其中有資料類型以及一系列用於設定索引屬性的核取方塊。 精靈可以產生根據中繼資料，並藉由取樣來源資料的欄位 清單。 

您可以大量選取屬性，即可在屬性資料行頂端的核取方塊。 選擇**可擷取**並**Searchable**應該傳回至用戶端應用程式，並受限於全文檢索搜尋處理的每一個欄位。 您會注意到整數不是完整的文字或模糊搜尋 （數字會逐字評估及通常可用於篩選）。

檢閱其描述[屬性編製索引](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib)並[語言分析器](https://docs.microsoft.com/rest/api/searchservice/language-support)如需詳細資訊。 

請花一點時間檢閱您的選擇。 一旦執行精靈，就會建立實體的資料結構，而且除非您捨棄並重新建立所有物件，否則將無法編輯這些欄位。

   ![Blob 索引定義](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6 - 建立索引子

若完整指定，精靈會在搜尋服務中建立三個不同的物件。 資料來源物件和索引物件會在 Azure 搜尋服務中儲存為具名資源。 最後一個步驟則會建立索引子物件。 為索引子命名可讓索引子以獨立資源的形式存在，索引子可以獨立地排程及管理，而不會受制於索引和資料來源物件 (這兩個物件會在相同的精靈序列中建立)。

如果您不熟悉索引子，「索引子」是 Azure 搜尋服務中的資源，其會搜耙外部資料來源中的可搜尋內容。 **匯入資料**精靈的輸出是一個索引子，其會搜耙 JSON 資料來源、擷取可搜尋的內容，並將該內容匯入到 Azure 搜尋服務上的索引。

   ![Blob 索引子定義](media/search-howto-index-json/import-wizard-json-indexer.png)

按一下 [確定] 來執行精靈並建立所有物件。 編製索引的程序會立即開始。

您可以在入口網站的頁面中監視資料匯入過程。 進度通知會指出編製索引的狀態，以及所上傳的文件數量。 

編製索引的程序完成後，您可以使用[搜尋總管](search-explorer.md)來查詢索引。

> [!NOTE]
> 如果您沒有看到您預期的資料，您可能需要多個欄位上設定其他屬性。 刪除索引和索引子，您剛建立的並逐步執行精靈一次，修改您在步驟 5 中的索引屬性的選項。 

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>使用 REST API

您可以使用 REST API 來編製索引 JSON blob，Azure 搜尋服務中的所有索引子來遵循常見的三段式工作流程： 建立資料來源、 建立索引、 建立索引子。 當您建立索引子要求提交，就會發生資料擷取，從 blob 儲存體。 完成此要求之後，您必須可供查詢的索引。 

您可以檢閱[REST 範例程式碼](#rest-example)本節示範如何建立所有的三個物件的結尾。 本章節也包含詳細資料的相關[剖析模式的 JSON](#parsing-modes)，[單一 blob](#parsing-single-blobs)， [JSON 陣列](#parsing-arrays)，以及[巢狀陣列](#nested-json-arrays)。

針對程式碼為基礎 JSON 編製索引，使用[Postman](search-fiddler.md)和 REST API 來建立這些物件：

+ [index](https://docs.microsoft.com/rest/api/searchservice/create-index)
+ [資料來源](https://docs.microsoft.com/rest/api/searchservice/create-data-source)
+ [indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

作業的順序會要求您建立，並依此順序呼叫物件。 相較於入口網站工作流程中，程式碼的方法需要可用的索引，以接受透過傳送的 JSON 文件**建立索引子**要求。

Azure Blob 儲存體中的 JSON blob 通常是單一 JSON 文件或 「 陣列 」 的 JSON。 Azure 搜尋服務中的 Blob 索引子可以剖析其中一種結構，取決於您如何設定在要求上的 **parsingMode** 參數。

| JSON 文件 | parsingMode | 描述 | 可用性 |
|--------------|-------------|--------------|--------------|
| 一個 blob 一個 | `json` | 將 JSON blob 當作單一文字區塊來剖析。 每一個 JSON blob 會變成單一 Azure 搜尋服務文件。 | 在正式推出[其餘](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)API 並[.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK。 |
| 一個 blob 多個 | `jsonArray` | 剖析 blob 中的 JSON 陣列，陣列的每個元素會變成不同的 Azure 搜尋服務文件。  | 在同時提供預覽[其餘](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)API 並[.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK。 |
| 一個 blob 多個 | `jsonLines` | 剖析 blob，其中包含新行字元，其中每個實體會成為個別的 Azure 搜尋服務文件以分隔多個 JSON 實體 （「 陣列 」）。 | 在同時提供預覽[其餘](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)API 並[.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) SDK。 |

### <a name="1---assemble-inputs-for-the-request"></a>1-組合要求的輸入

針對每個要求中，您必須提供服務名稱和系統管理金鑰 （POST 標頭中），Azure 搜尋服務和儲存體帳戶名稱和 blob 儲存體金鑰。 您可以使用[Postman](search-fiddler.md)將 HTTP 要求傳送至 Azure 搜尋服務。

將下列四項值複製到 記事本，讓您可以將其貼到 要求：

+ Azure 搜尋服務名稱
+ Azure 搜尋服務管理金鑰
+ Azure 儲存體帳戶名稱
+ Azure 儲存體帳戶金鑰

您可以在入口網站中找到這些值：

1. 在 Azure 搜尋服務入口網站頁面中，請從 [概觀] 頁面中複製的搜尋服務 URL。

2. 在左側的導覽窗格中，按一下**金鑰**然後複製其中一個主要或次要金鑰 （它們是相等的）。

3. 切換至您的儲存體帳戶的入口網站頁面。 在左側的導覽窗格中，在**設定**，按一下**便捷鍵**。 此頁面會提供帳戶名稱和金鑰。 將儲存體帳戶名稱和其中一個金鑰複製到 [記事本]。

### <a name="2---create-a-data-source"></a>2-建立資料來源

此步驟中提供索引子所使用的資料來源連接資訊。 資料來源是保存連接資訊的 Azure 搜尋服務中的具名的物件。 資料來源類型， `azureblob`，決定哪些資料擷取行為叫用索引子。 

替換成有效的值，服務名稱、 系統管理金鑰、 儲存體帳戶和帳戶金鑰的預留位置。

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="3---create-a-target-search-index"></a>3-建立目標搜尋索引 

索引子要搭配索引結構描述。 如果您使用 API (而不是入口網站)，請事先準備好索引，以便在索引子作業中指定它。

索引會將可搜尋的內容儲存在 Azure 搜尋服務中。 若要建立索引，請提供結構描述來指定文件中的欄位、屬性和其他可形塑搜尋體驗的建構。 如果您建立的索引具有與來源相同的欄位名稱和資料類型，索引子便會比對來源和目的地的欄位，讓您不必明確地對應欄位。

下列範例說明[建立索引](https://docs.microsoft.com/rest/api/searchservice/create-index)要求。 索引會有可搜尋的 `content` 欄位，以供儲存從 Blob 擷取到的文字︰   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="4---configure-and-run-the-indexer"></a>4-設定及執行索引子

因為具有索引和資料來源和索引子也是具名物件，您會建立，並在 Azure 搜尋服務上重複使用。 建立索引子的完整指定的要求可能如下所示：

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

索引子組態是在要求主體中。 它需要的資料來源和空的目標索引已存在於 Azure 搜尋服務。 

排程和參數是選擇性的。 如果您省略它們，索引子會立即執行，使用`json`作為剖析模式。

不包含這個特定的索引子[欄位對應](#field-mappings)。 在索引子定義中，您可以省略**欄位對應**如果目標搜尋索引的欄位值的來源 JSON 文件屬性相符。 


### <a name="rest-example"></a>REST 範例

此區段是用來建立物件的所有要求的回顧。 如元件組件的討論，請參閱這篇文章中的前幾節。

### <a name="data-source-request"></a>資料來源的要求

所有索引子要求提供現有的資料連接資訊的資料來源物件。 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }  


### <a name="index-request"></a>索引要求

所有索引子需要接收資料的目標索引。 要求的主體會定義索引結構描述，其中包含屬於支援所需的行為，可搜尋的索引中的欄位。 當您執行索引子時，此索引應為空白。 

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="indexer-request"></a>索引子要求

此要求會顯示完整指定的索引子。 它包含[欄位對應](#field-mappings)，這已在前一個範例中省略。 還記得該 「 排程 」，「 參數 」，和 「 fieldMappings"是選擇性的只要沒有可用的預設值。 省略 「 排程 」，會導致立即執行索引子。 省略"parsingMode 」 會使用 「 json 」 預設的索引。

建立 Azure 搜尋服務索引子會觸發資料匯入。 它立即，之後會在執行排程提供了其中一個。

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key for Azure Search]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }


<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>使用 .NET SDK

.NET SDK 與 REST API 完全對等。 建議您檢閱先前的 REST API 章節，以了解其概念、工作流程和需求。 然後，您可以參閱下列 .NET API 參考文件，以在受控程式碼中實作 JSON 索引子。

+ [microsoft.azure.search.models.datasource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

<a name="parsing-modes"></a>

## <a name="parsing-modes"></a>剖析模式

JSON blob 可以假設多個表單。 **ParsingMode** JSON 索引子上的參數可決定如何剖析並在 Azure 搜尋服務索引中結構化 JSON blob 內容：

| parsingMode | 描述 |
|-------------|-------------|
| `json`  | 索引為單一文件的每個 blob。 這是預設值。 |
| `jsonArray` | 如果您的 blob 包含 JSON 陣列，而您需要成為 Azure 搜尋服務中的個別文件陣列的每個項目，請選擇這個模式。 |
|`jsonLines` | 如果您的 blob 包含多個 JSON 實體，以新行分隔，而您需要成為個別的文件，Azure 搜尋服務中的每個實體，請選擇這個模式。 |

您可以將文件視為搜尋結果中的單一項目。 如果您想要顯示在搜尋結果中為獨立項目陣列中的每個項目，然後使用`jsonArray`或`jsonLines`適當選項。

在索引子定義中，您可以選擇性地使用[欄位對應](search-indexer-field-mappings.md)，以選擇用來填入目標搜尋索引的來源 JSON 文件屬性。 針對`jsonArray`剖析模式，如果陣列為較低層級屬性時，您可以設定文件根目錄，指出陣列在 blob 中的放置位置。

> [!IMPORTANT]
> 當您使用`json`，`jsonArray`或`jsonLines`剖析模式時，Azure 搜尋服務會假設您的資料來源中的所有 blob 都包含 JSON。 如果您需要支援在相同的資料來源中混用 JSON 和非 JSON Blob，請在 [UserVoice 網站](https://feedback.azure.com/forums/263029-azure-search)上讓我們知道。


<a name="parsing-single-blobs"></a>

## <a name="parse-single-json-blobs"></a>剖析單一 JSON blob

根據預設， [Azure 搜尋服務 Blob 索引子](search-howto-indexing-azure-blob-storage.md) 會將 JSON Blob 剖析為單一的文字區塊。 通常，您會想要保留 JSON 文件的結構。 例如，假設您在 Azure Blob 儲存體中有下列 JSON 文件：

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Blob 索引子會將 JSON 文件剖析成單一的 Azure 搜尋服務文件。 索引子會比對來源中的 "text"、"datePublished"、"tags"，找出與目標索引欄位具有相同名稱和類型的索引，然後載入索引。

如前所述，不一定要使用欄位對應。 指定索引的 "text"、"datePublished"、"tags" 欄位，blob 索引子可以推斷正確的對應，故要求中不需要欄位對應。

<a name="parsing-arrays"></a>

## <a name="parse-json-arrays"></a>剖析 JSON 陣列

或者，您可以使用 JSON 陣列選項。 當 blob 包含這個選項非常有用*語式正確的 JSON 物件的陣列*，而且您想要成為個別的 Azure 搜尋服務文件的每個項目。 以下列 JSON blob 為例，您可以將 Azure 搜尋服務索引填入三個不同的文件，每個都具有 "id" 和 "text" 欄位。  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

若為 JSON 陣列，索引子定義看起來應該像下列範例。 請注意，parsingMode 參數會指定 `jsonArray` 剖析器。 指定正確的剖析器，並具有正確的資料輸入會編製索引 JSON blob 的只有兩個陣列特定需求。

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

同樣地，請注意您可以省略欄位對應。 採用 "id" 和 "text" 欄位名稱相同的索引，Blob 索引子便可以推斷正確的對應，而不需要明確的欄位對應清單。

<a name="nested-json-arrays"></a>

## <a name="parse-nested-arrays"></a>剖析巢狀的陣列
針對 JSON 陣列具有巢狀項目，您可以指定`documentRoot`表示多層級的結構。 例如，如果您的 Blob 看起來像這樣︰

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

使用這個設定來索引 `level2` 屬性中包含的陣列：

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="parse-blobs-separated-by-newlines"></a>剖析 blob 以換行符號分隔

如果您的 blob 包含以新行字元，分隔的多個 JSON 實體，而且您想要每個元素成為個別的 Azure 搜尋服務文件，您可以選擇 JSON 線條選項。 例如，假設下列 blob （如果有三個不同的 JSON 實體），您可以將 Azure 搜尋服務索引三個不同的文件，每個都具有 「 識別碼 」 和 「 文字 」 欄位填入。

    { "id" : "1", "text" : "example 1" }
    { "id" : "2", "text" : "example 2" }
    { "id" : "3", "text" : "example 3" }

JSON 線條索引子定義看起來應該類似下列的範例。 請注意，parsingMode 參數會指定 `jsonLines` 剖析器。 

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonLines" } }
    }

請注意，欄位對應可以省略，類似於`jsonArray`剖析模式。

## <a name="add-field-mappings"></a>新增欄位對應

當來源和目標的欄位不能完全對齊時，您可以在要求主體中定義欄位對應區段，明確指定欄位對欄位的關聯。

目前，Azure 搜尋服務無法編製索引任意 JSON 文件直接，因為它支援基本資料型別、 字串陣列和 GeoJSON 點。 不過，您可以使用 **欄位對應** 挑選 JSON 文件的幾個部分，並將它們「上移」到搜尋文件的最上層欄位。 如需了解欄位對應的基本知識，請參閱 [Azure 搜尋服務索引子中的欄位對應](search-indexer-field-mappings.md)。

回到我們的範例 JSON 文件︰

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

假設搜尋索引有下列欄位︰`Edm.String` 類型的 `text`、`Edm.DateTimeOffset` 類型的 `date`、`Collection(Edm.String)` 類型的 `tags`。 請注意來源中的 "datePublished" 與索引中的 `date` 欄位之間的差異。 若要將 JSON 對應到所需形狀，請使用下列欄位對應︰

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

對應中的來源欄位名稱是使用 [JSON 指標](https://tools.ietf.org/html/rfc6901) 標記法指定。 以正斜線開始參考 JSON 文件的根目錄，然後使用正斜線分隔的路徑挑選所需的屬性 (使用任意層級的巢狀結構)。

您也可以使用以零為起始的索引來參考個別陣列元素。 比方說，若要從上述範例挑選 "tags" 陣列的第一個元素，請如下所示使用欄位對應︰

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> 如果欄位對應路徑中的來源欄位名稱參考在 JSON 中不存在的屬性，則會略過該對應且不會產生錯誤。 這麼做是為了讓我們可支援具有不同結構描述 (這是常見的使用案例) 的文件。 因為沒有任何驗證，您必須小心避免在欄位對應規格中出現錯字。
>
>

## <a name="see-also"></a>請參閱

+ [Azure 搜尋服務中的索引子](search-indexer-overview.md)
+ [使用 Azure 搜尋服務編製 Azure Blob 儲存體的索引](search-howto-index-json-blobs.md)
+ [使用 Azure 搜尋服務 blob 索引子編製 CSV blob 的索引](search-howto-index-csv-blobs.md)
+ [教學課程：搜尋 Azure Blob 儲存體中的半結構化資料](search-semi-structured-data.md)

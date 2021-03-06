---
title: 語言和文字處理的分析器 - Azure 搜尋服務
description: 將分析器指派給索引中的可搜尋文字欄位，可將預設的標準 Lucene 取代為自訂、預先定義或特定語言的替代項目。
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 7306258b6a7eee66df0961b2b993d0bcc9de94b9
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343267"
---
# <a name="analyzers-for-text-processing-in-azure-search"></a>Azure 搜尋服務中用於文字處理的分析器

「分析器」是[全文搜尋引擎](search-lucene-query-architecture.md)的元件，負責查詢字串和已編製索引文件中的文字處理。 視案例而定，不同的分析器會以不同的方式處理文字。 語言分析器會使用語言規則來處理文字，以便改善搜尋品質，而其他分析器則會執行更基本的工作，例如將字元轉換為小寫。 

語言分析器最常用，而且有指派給 Azure 搜尋服務索引中每個可搜尋欄位的預設語言分析器。 在文字分析期間的典型語言轉換如下：

+ 會移除非必要字組 (停用字詞) 和標點符號。
+ 片語和有連字號的字組會被細分為組件。
+ 大寫字組會被改為小寫。
+ 字組會縮減為根表單，如此不論時態就可以找到相符項目。

語言分析器會將文字輸入轉換為基本表單或根表單，如此可以有效率地儲存和檢索資訊。 在編製索引期間、索引建立之時、以及讀取索引進行搜尋的時候，皆會進行轉換。 若兩方作業均使用相同的分析器，更有可能獲得想要的搜尋結果。

## <a name="default-analyzer"></a>預設分析器  

Azure 搜尋服務會使用 [Apache Lucene 標準分析器 (標準 Lucene)](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) 作為預設值，該分析器會依照[「Unicode 文字分割」](https://unicode.org/reports/tr29/)規則將文字分為數個元素。 此外，標準分析器會將所有字元轉換為它們的小寫形式。 已編製索引的文件和搜尋字詞在編製索引和查詢處理期間都會執行分析。  

它會自動用於每個可搜尋的欄位。 您可以逐欄覆寫預設值。 替代分析器可以是[語言分析器](index-add-language-analyzers.md)、[自訂分析器](index-add-custom-analyzers.md)，或[可用分析器清單](index-add-custom-analyzers.md#AnalyzerTable)中預先定義的分析器。


## <a name="types-of-analyzers"></a>分析器類型

下列清單說明 Azure 搜尋服務中可用的分析器。

| 類別 | 說明 |
|----------|-------------|
| [標準 Lucene 分析器](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | 預設值。 不需要任何規格或設定。 這個一般用途的分析器對於大部分的語言和情節都能順利執行。|
| 預先定義的分析器 | 作為預計要依現狀使用的成品提供。 <br/>共有兩種類型：特製化和語言。 使它們成為「預先定義」的條件是依名稱參考，無須設定或自訂。 <br/><br/>[特製化 (語言無從驗證) 分析器](index-add-custom-analyzers.md#AnalyzerTable)適用於文字輸入需要特殊處理或最少處理時。 非語言預先定義的分析器包含 **Asciifolding**、**金鑰**、**模式**、**簡單**、**停止**、**空白**。<br/><br/>[語言分析器](index-add-language-analyzers.md)適用於當您需要為個別語言提供豐富的語言支援時。 Azure 搜尋服務支援 35 個 Lucene 語言分析器和 50 個 Microsoft 自然語言處理分析器。 |
|[自訂分析器](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | 意指結合現有元素的使用者定義組態，包括一個權杖化工具 (必要) 和選擇性篩選條件 (Char 或權杖)。|

一些預先定義的分析器 (例如 **Pattern** 或 **Stop**) 可支援一組有限的設定選項。 若要設定這些選項，請實際上建立自訂分析器，其中包含預先定義的分析器和其中一個記載於[預先定義的分析器參考](index-add-custom-analyzers.md#AnalyzerTable)中的替代選項。 如同任何自訂組態，為您的新組態提供名稱，例如 myPatternAnalyzer，以便與 Lucene 分析器有所區別。

## <a name="how-to-specify-analyzers"></a>如何指定分析器

1. (僅限於自訂分析器) 在索引定義中建立具名的 **analyzer** 區段。 如需詳細資訊，請參閱[建立索引](https://docs.microsoft.com/rest/api/searchservice/create-index)以及[新增自訂分析器](index-add-custom-analyzers.md)。

2. 在索引的[欄位定義](https://docs.microsoft.com/rest/api/searchservice/create-index)上，將欄位的 **analyzer** 屬性設為目標分析器的名稱 (例如 `"analyzer" = "keyword"`)。 有效值包括預先定義的分析器名稱、語言分析器名稱，或者也會定義於索引結構描述中的自訂分析器名稱。 計劃在服務中建立索引之前，在索引定義階段指派分析器。

3. 您可以選擇性地使用 **indexAnalyzer** 和 **searchAnalyzer`** 欄位參數，而非一個 **analyzer** 屬性，設定不同的分析器來編製索引和查詢。 如果其中一個活動需要其他活動所不需的特定轉換，您會使用不同的分析器進行資料準備和擷取。

不允許將 **analyzer** 或 **indexAnalyzer** 指派給已實際建立的欄位。 如果上述任何一項不清楚，請檢閱下表中的明細，以了解哪些動作需要重建和原因。
 
 | 案例 | 影響 | 步驟 |
 |----------|--------|-------|
 | 新增欄位 | 最低限度 | 若在結構描述中尚未具有欄位，則由於在索引中尚未實際存有欄位，因此不會執行欄位修訂。 您可以使用[更新索引](https://docs.microsoft.com/rest/api/searchservice/update-index)將新欄位新增至現有索引，以及使用 [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) 來填入資料。|
 | 將 **analyzer** 或 **indexAnalyzer** 新增至現有已編製索引的欄位。 | [重建](search-howto-reindex.md) | 該欄位的反向索引必須從頭重新編製，且必須重新編製這些欄位的內容索引。 <br/> <br/>針對正在開發中的索引，[刪除](https://docs.microsoft.com/rest/api/searchservice/delete-index)和[建立](https://docs.microsoft.com/rest/api/searchservice/create-index)索引以挑選新的欄位定義。 <br/> <br/>對於生產環境中的索引，您可建立新欄位以提供修改過的定義，然後開始使用它取代舊的定義，藉此延遲重建。 使用[更新索引](https://docs.microsoft.com/rest/api/searchservice/update-index)來合併新欄位，以及使用 [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) 來填入資料。 稍後，您可以清除索引以移除過時的欄位，當作規劃索引服務的一部分。 |

## <a name="when-to-add-analyzers"></a>新增分析器的時機

新增並指派分析器的最佳時機是在有效開發期間內，當卸除並重建索引為例行工作時。

隨著索引定義強化起來，您可以將新的分析結構附加到索引，但如果您想要避免錯誤，則必須將 **allowIndexDowntime** 旗標傳遞至[更新索引](https://docs.microsoft.com/rest/api/searchservice/update-index)：

*「不允許更新索引，因為這會導致停機。若要將新的分析器、權杖化工具、權杖篩選器或字元篩選器新增至現有索引，請在索引更新要求中將 'allowIndexDowntime' 查詢參數設為 'true'。請注意，這項作業至少會讓您的索引離線幾秒鐘，而會導致您的索引編製和查詢要求失敗。在索引更新後，索引的效能和寫入可用性可能會降低數分鐘，如果是非常大的索引，則可能持續更久。」*

將分析器指派給欄位時也是如此。 分析器是欄位定義不可或缺的部分，因此您只可以在建立欄位時加以新增。 如果您想要將分析器新增到現有欄位，則必須[卸除並重建](search-howto-reindex.md)索引，或新增具有您所要分析器的新欄位。

如上所述，例外狀況是 **searchAnalyzer** 變體。 在指定分析器的三種方式 (**analyzer**、**indexAnalyzer**、**searchAnalyzer**) 中，只有 **searchAnalyzer** 屬性可在現有欄位上變更。

## <a name="recommendations-for-working-with-analyzers"></a>使用分析器的建議

本節提供如何使用分析器的建議。

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>一個用於讀寫的分析器，除非您有特定的需求

Azure 搜尋服務可讓您指定不同的分析器來編製索引，並透過其他 **indexAnalyzer** 和 **searchAnalyzer** 欄位參數進行搜尋。 如果未指定，以 **analyzer** 屬性設定的分析器可用於編製索引和搜尋。 若未指定 `analyzer`，就會使用標準 Lucene 分析器。

一般規則是使用索引和查詢的相同分析器，除非特定需求另有指示。 請務必徹底測試。 當搜尋和索引時的文字處理不同，會有查詢字詞和索引字詞不符的風險，因為搜尋和索引分析器的設定不一致。

### <a name="test-during-active-development"></a>在開發期間進行測試

覆寫標準分析器需要重建索引。 可能的話，請先決定要在開發期間使用的作用中分析器，然後才將索引實際執行。

### <a name="inspect-tokenized-terms"></a>檢查權杖化字詞

若搜尋作業無法傳回預期中的結果，則查詢的字詞輸入以及索引中的權杖化字詞之間，極有可能發生權杖不相符的狀況。 若權杖不相同，則比對作業無法實行。 若要檢查權杖化工具輸出，建議使用[分析 API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) 做為調查工具。 回應由權杖組成，一如特定分析器所產生的權杖。

### <a name="compare-english-analyzers"></a>比較英文分析器

[搜尋分析器示範](https://alice.unearth.ai/)是一種協力廠商示範應用程式，其會顯示標準 Lucene 分析器、Lucene 的英文語言分析器，以及 Microsoft 的英文版自然語言處理器之間的並排比較。 索引已固定；內含熱門故事中的文字。 您提供的每項搜尋輸入，會在相鄰窗格中顯示來自每個分析器的結果，協助您瞭解每個分析器處理相同字串的方式。 

## <a name="examples"></a>範例

下列範例會顯示幾個重要情節的分析器定義。

+ [自訂分析器範例](#Example1)
+ [將分析器指派給欄位範例](#Example2)
+ [混合編製索引和搜尋的分析器](#Example3)
+ [語言分析器範例](#Example4)

<a name="Example1"></a>

### <a name="custom-analyzer-example"></a>自訂分析器範例

此範例會使用自訂選項來說明分析器定義。 Char 篩選、安全性權杖和權杖篩選條件會個別指定為具名的建構，然後在分析器定義中加以參考。 預先定義的元素會依現狀使用，而且只依名稱加以參考。

逐步解說這個範例：

* 分析器是可搜尋欄位的欄位類別屬性。
* 自訂分析器是索引定義的一部分。 它可能是小幅自訂 (例如，在一個篩選條件中自訂單一選項) 或在多個位置中加以自訂。
* 在此情況下，自訂分析器是 "my_analyzer"，會依次使用自訂的標準權杖化工具 "my_standard_tokenizer" 和兩個權杖篩選條件：小寫和自訂的 asciifolding 篩選條件 "my_asciifolding"。
* 此外，其中也會定義 2 個自訂 char 篩選條件「map_dash」和「remove_whitespace」。 第一個會以底線取代所有的連字號，而第二個則會移除所有空格。 空間必須在對應規則中以 UTF-8 編碼。 權杖化之前會套用 Char 篩選條件，而且Char 篩選條件會影響所產生的權杖 (標準權杖化工具會在虛線和空格中斷，而不會在底線中斷)。

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
~~~~

<a name="Example2"></a>

### <a name="per-field-analyzer-assignment-example"></a>每個欄位的分析器指派範例

預設值為標準分析器。 假設您需要將預設值取代為不同的預先定義分析器，例如模式分析器。 如果您未設定自訂選項，就只需要在欄位定義中依名稱加以指定。

「分析器」元素會以各欄位方式來覆寫標準分析器。 沒有通用的覆寫。 在此範例中，`text1` 會使用模式分析器和 `text2` (其中並未指定分析器) 來使用預設值。

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
~~~~

<a name="Example3"></a>

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>混合編製索引和搜尋作業的分析器

API 包含其他的索引屬性，可針對索引和搜尋指定不同的分析器。 必須將 **searchAnalyzer** 和 **indexAnalyzer** 屬性指定為一組，從而取代單一 **analyzer** 屬性。


~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
~~~~

<a name="Example4"></a>

### <a name="language-analyzer-example"></a>語言分析器範例

包含不同語言之字串的欄位可以使用語言分析器，而其他欄位則是保留預設值 (或使用一些其他的預先定義或自訂分析器)。 如果您是使用語言分析器，就必須將它用於索引和搜尋作業。 使用語言分析器的欄位不能具有索引和搜尋的不同分析器。

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
~~~~

## <a name="next-steps"></a>後續步驟

+ 請檢閱[全文檢索搜尋如何在 Azure 搜尋服務中運作](search-lucene-query-architecture.md)的完整說明。 本文使用範例來說明表面上看似違反直覺的行為。

+ 請從[搜尋文件](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples)範例章節，或從入口網站的搜尋總管中[簡單查詢語法](query-simple-syntax.md)，嘗試其他查詢語法。

+ 了解如何套用[特定語言的語彙分析器](index-add-language-analyzers.md)。

+ [設定自訂分析器](index-add-custom-analyzers.md)以進行最少的處理，或是在個別欄位上進行特殊的處理。

+ 在這個示範網站上的相鄰窗格中[比較標準和英文分析器](https://alice.unearth.ai/)。 

## <a name="see-also"></a>另請參閱

 [搜尋文件 REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [簡單查詢語法](query-simple-syntax.md) 

 [完整的 Lucene 查詢語法](query-lucene-syntax.md) 
 
 [處理搜尋結果](search-pagination-page-layout.md)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png

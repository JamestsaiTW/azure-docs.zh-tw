---
title: 快速入門：使用 C# 呼叫您的 Bing 自訂搜尋端點 | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: 您可以使用本快速入門，開始透過 C# 要求 Bing 自訂搜尋執行個體所產生的搜尋結果。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: maheshb
ms.openlocfilehash: ec0b1d95f5151d4aef45d4e02926d33ee71c5fbd
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878318"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-c"></a>快速入門：使用 C# 呼叫您的 Bing 自訂搜尋端點 

您可以使用本快速入門，開始要求 Bing 自訂搜尋執行個體所產生的搜尋結果。 雖然此應用程式是以 C# 撰寫的，但 Bing 自訂搜尋 API 是一種與大多數程式設計語言都相容的 RESTful Web 服務。 此範例的原始程式碼可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingCustomSearchv7.cs) 中找到。

## <a name="prerequisites"></a>必要條件

- 「Bing 自訂搜尋」執行個體。 請參閱[快速入門：建立您的第一個 Bing 自訂搜尋執行個體](quick-start.md)，以取得詳細資訊。
- Microsoft [.Net Core](https://www.microsoft.com/net/download/core)
- 任何版本的 [Visual Studio 2017](https://www.visualstudio.com/downloads/)
- 如果您使用 Linux/MacOS，則可以使用 [Mono](http://www.mono-project.com/)來執行此應用程式。
- 已安裝 [NuGet 自訂搜尋](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)套件。 
    - 在 Visual Studio 的 [方案總管] 中，以滑鼠右鍵按一下專案，然後從功能表選取 `Manage NuGet Packages`。 安裝 `Microsoft.Azure.CognitiveServices.Search.CustomSearch` 套件。 安裝 NuGet 自訂搜尋套件也會一併安裝下列組件：
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>建立應用程式並將其初始化

1. 在 Visual Studio 中，建立新的 C# 主控台應用程式。 然後，將下列套件新增至您的專案。

    ```csharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    ```

2. 建立下列類別以儲存 Bing 自訂搜尋 API 所傳回的搜尋結果。

    ```csharp
    public class BingCustomSearchResponse {        
        public string _type{ get; set; }            
        public WebPages webPages { get; set; }
    }

    public class WebPages {
        public string webSearchUrl { get; set; }
        public int totalEstimatedMatches { get; set; }
        public WebPage[] value { get; set; }        
    }

    public class WebPage {
        public string name { get; set; }
        public string url { get; set; }
        public string displayUrl { get; set; }
        public string snippet { get; set; }
        public DateTime dateLastCrawled { get; set; }
        public string cachedPageUrl { get; set; }
    }
    ```

3. 在您專案的主要方法中，建立 Bing 自訂搜尋 API 訂用帳戶金鑰的變數、搜尋執行個體的自訂組態識別碼，以及搜尋詞彙。

    ```csharp
    var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
    var searchTerm = args.Length > 0 ? args[0]:"microsoft";
    ```

4. 將搜尋詞彙附加到 `q=` 查詢參數，並將搜尋執行個體的自訂組態識別碼附加到 `customconfig=`，以建構要求 URL。 使用 `&` 字元分隔參數。 

    ```csharp
    var url = "https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?" +
                "q=" + searchTerm + "&" +
                "customconfig=" + customConfigId;
    ```

## <a name="send-and-receive-a-search-request"></a>傳送及接收搜尋要求 

1. 建立要求用戶端，並將您的訂用帳戶金鑰新增至 `Ocp-Apim-Subscription-Key` 標頭。

    ```csharp
    var client = new HttpClient();
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    ```

2. 執行搜尋要求並取得 JSON 物件形式的回應。

    ```csharp
    var httpResponseMessage = client.GetAsync(url).Result;
    var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
    BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
    ```
## <a name="process-and-view-the-results"></a>處理並檢視結果

1. 逐一查看回應物件以顯示每個搜尋結果的相關資訊，包括其名稱、URL，以及上次搜耙網頁的日期。

    ```csharp
    for(int i = 0; i < response.webPages.value.Length; i++) {                
        var webPage = response.webPages.value[i];
        
        Console.WriteLine("name: " + webPage.name);
        Console.WriteLine("url: " + webPage.url);                
        Console.WriteLine("displayUrl: " + webPage.displayUrl);
        Console.WriteLine("snippet: " + webPage.snippet);
        Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
        Console.WriteLine();
    }
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
    ```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [建置自訂搜尋 Web 應用程式](./tutorials/custom-search-web-page.md)

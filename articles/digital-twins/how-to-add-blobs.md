---
title: 如何新增 Blob 到 Azure Digital Twins 中的物件 | Microsoft Docs
description: 了解如何新增 Blob 到 Azure Digital Twins 中的物件。
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: ffd7d71c33b569b396b9f8babf8105968ee525b9
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263062"
---
# <a name="add-blobs-to-objects-in-azure-digital-twins"></a>在 Azure Digital Twins 中將 Blob 新增到物件

Blob 是常見檔案類型 (例如圖片和記錄) 的非結構化表示法。 Blob 使用 MIME 類型 (例如："image/jpeg") 和中繼資料 (名稱、描述、類型等等)，追蹤它們所代表的資料類型。

Azure Digital Twins 支援將 Blob 連結到裝置、空間和使用者。 Blob 可以代表使用者的個人檔案圖片、裝置的相片、影片、地圖、韌體、韌體 zip、JSON 資料或記錄等等。

[!INCLUDE [Digital Twins Management API familiarity](../../includes/digital-twins-familiarity.md)]

## <a name="uploading-blobs-overview"></a>上傳 Blob 概觀

您可以使用多部分要求，將 Blob 上傳到特定端點及其各自的功能。

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

### <a name="blob-metadata"></a>Blob 中繼資料

除了 **Content-Type** 和 **Content-Disposition**，Azure Digital Twins Blob 多部分要求必須指定正確的 JSON 主體。 要提交的 JSON 主體取決於所執行 HTTP 要求作業的種類。

四個主要 JSON 結構描述是：

![JSON 結構描述][1]

JSON blob 中繼資料符合下列模型：

```JSON
{
    "parentId": "00000000-0000-0000-0000-000000000000",
    "name": "My First Blob",
    "type": "Map",
    "subtype": "GenericMap",
    "description": "A well chosen description",
    "sharing": "None"
  }
```

| 屬性 | 類型 | 說明 |
| --- | --- | --- |
| **parentId** | 字串 | 與 blob 相關聯的父實體 (空間、裝置或使用者) |
| **name** |字串 | blob 的人類易記名稱 |
| **type** | 字串 | blob 的類型 - 不能使用 *type* 與 *typeId*  |
| **typeId** | 整數  | blob 類型識別碼 - 不能使用 *type* 與 *typeId* |
| **subtype** | 字串 | blob 子類型 - 不能使用 *subtype* 與 *subtypeId* |
| **subtypeId** | 整數  | blob 的子類型識別碼 - 不能使用 *subtype* 與 *subtypeId* |
| **description** | 字串 | 自訂的 blob 描述 |
| **sharing** | 字串 | 是否可以共用 blob - enum [`None`, `Tree`, `Global`] |

一律提供 blob 中繼資料作為 **Content-Type** 為 `application/json` 的第一個區塊，或作為 `.json` 檔案。 檔案資料是在第二個區塊中提供的，可以是任何支援的 MIME 類型。

Swagger 文件會完整詳細說明這些模型結構描述。

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

若要了解如何使用參考文件，請參閱[如何使用 Swagger](./how-to-use-swagger.md)。

<div id="blobModel"></div>

### <a name="blobs-response-data"></a>Blob 回應資料

個別傳回的 Blob 符合下列的 JSON 結構描述：

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "parentId": "00000000-0000-0000-0000-000000000000",
  "type": "string",
  "subtype": "string",
  "typeId": 0,
  "subtypeId": 0,
  "sharing": "None",
  "description": "string",
  "contentInfos": [
    {
      "type": "string",
      "sizeBytes": 0,
      "mD5": "string",
      "version": "string",
      "lastModifiedUtc": "2019-01-12T00:58:08.689Z",
      "metadata": {
        "additionalProp1": "string",
        "additionalProp2": "string",
        "additionalProp3": "string"
      }
    }
  ],
  "fullName": "string",
  "spacePaths": [
    "string"
  ]
}
```

| 屬性 | 類型 | 說明 |
| --- | --- | --- |
| **id** | 字串 | Blob 的專屬識別碼 |
| **name** |字串 | Blob 的人類易記名稱 |
| **parentId** | 字串 | 與 Blob 相關聯的父實體 (空間、裝置或使用者) |
| **type** | 字串 | Blob 的類型 - 不能使用 *type* 與 *typeId*  |
| **typeId** | 整數  | Blob 類型識別碼 - 不能使用 *type* 與 *typeId* |
| **subtype** | 字串 | Blob 子類型 - 不能使用 *subtype* 與 *subtypeId* |
| **subtypeId** | 整數  | Blob 的子類型識別碼 - 不能使用 *subtype* 與 *subtypeId* |
| **sharing** | 字串 | 是否可以共用 blob - enum [`None`, `Tree`, `Global`] |
| **description** | 字串 | 自訂的 Blob 描述 |
| **contentInfos** | 陣列 | 指定非結構化的中繼資料資訊，包括版本 |
| **fullName** | 字串 | Blob 的完整名稱 |
| **spacePaths** | 字串 | 空間路徑 |

一律提供 Blob 中繼資料作為 **Content-Type** 為 `application/json` 的第一個區塊，或作為 `.json` 檔案。 檔案資料是在第二個區塊中提供的，可以是任何支援的 MIME 類型。

### <a name="blob-multipart-request-examples"></a>Blob 多部分要求範例

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

若要上傳文字檔作為 Blob，並將其與空間建立關聯，請發出已驗證的 HTTP POST 要求至：

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs
```

使用下列主體：

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "ParentId": "54213cf5-285f-e611-80c3-000d3a320e1e",
  "Name": "My First Blob",
  "Type": "Map",
  "SubType": "GenericMap",
  "Description": "A well chosen description",
  "Sharing": "None"
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="myblob.txt"
Content-Type: text/plain

This is my blob content. In this case, some text, but I could also be uploading a picture, an JSON file, a firmware zip, etc.

--USER_DEFINED_BOUNDARY--
```

| 值 | 更換為 |
| --- | --- |
| USER_DEFINED_BOUNDARY | 多部分內容界限名稱 |

以下程式碼是使用類別 [MultipartFormDataContent](https://docs.microsoft.com/dotnet/api/system.net.http.multipartformdatacontent) 的相同 Blob 上傳 .NET 實作：

```csharp
//Supply your metadata in a suitable format
var multipartContent = new MultipartFormDataContent("USER_DEFINED_BOUNDARY");

var metadataContent = new StringContent(JsonConvert.SerializeObject(metaData), Encoding.UTF8, "application/json");
metadataContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json; charset=utf-8");
multipartContent.Add(metadataContent, "metadata");

//MY_BLOB.txt is the String representation of your text file
var fileContents = new StringContent("MY_BLOB.txt");
fileContents.Headers.ContentType = MediaTypeHeaderValue.Parse("text/plain");
multipartContent.Add(fileContents, "contents");

var response = await httpClient.PostAsync("spaces/blobs", multipartContent);
```

最後，[cURL](https://curl.haxx.se/) 使用者可以相同的方式提出多部分表單要求：

![裝置 Blob][5]

```bash
curl
 -X POST "YOUR_MANAGEMENT_API_URL/spaces/blobs"
 -H "Authorization: Bearer YOUR_TOKEN"
 -H "Accept: application/json"
 -H "Content-Type: multipart/form-data"
 -F "meta={\"ParentId\": \"YOUR_SPACE_ID\",\"Name\":\"My CURL Blob",\"Type\":\"Map\",\"SubType\":\"GenericMap\",\"Description\": \"A well chosen description\", \"Sharing\": \"None\"};type=application/json"
 -F "text=PATH_TO_FILE;type=text/plain"
```

| 值 | 更換為 |
| --- | --- |
| YOUR_TOKEN | 有效的 OAuth 2.0 權杖 |
| YOUR_SPACE_ID | 要與 Blob 產生關聯的空間其識別碼 |
| PATH_TO_FILE | 文字檔案的路徑 |

成功 POST 傳回新 Blob 的識別碼 (先前以紅色醒目提示)。

## <a name="api-endpoints"></a>API 端點

下列各節說明核心 Blob 相關的 API 端點及其功能。

### <a name="devices"></a>裝置

您可以將 blob 連結到裝置。 下圖顯示管理 API 的 Swagger 參考文件。 其中指定裝置相關 API 端點以供 Blob 取用，以及任何要傳入的必要路徑參數。

![裝置 Blob][2]

例如，若要更新或建立 Blob 並將 Blob 連結到裝置，請發出已驗證的 HTTP PATCH 要求至：

```plaintext
YOUR_MANAGEMENT_API_URL/devices/blobs/YOUR_BLOB_ID
```

| 參數 | 更換為 |
| --- | --- |
| *YOUR_BLOB_ID* | 所需的 Blob 識別碼 |

成功的要求傳回 JSON 物件，如[先前所述](#blobModel)。

### <a name="spaces"></a>空格

您也可以江 Blob 連結道空間。 下圖列出負責處理 Blob 的所有空間 API 端點。 也會列出要傳入這些端點的任何路徑參數。

![空間 Blob][3]

例如，若要傳回連結到空間的 Blob，請發出已驗證的 HTTP GET 要求至：

```plaintext
YOUR_MANAGEMENT_API_URL/spaces/blobs/YOUR_BLOB_ID
```

| 參數 | 更換為 |
| --- | --- |
| *YOUR_BLOB_ID* | 所需的 Blob 識別碼 |

成功的要求傳回 JSON 物件，如[先前所述](#blobModel)。

相同端點的 PATCH 要求會更新中繼資料描述，並建立 Blob 的版本。 HTTP 要求是透過 PATCH 方法搭配任何必要的中繼與多部分表單資料來發出。

### <a name="users"></a>使用者

您可以將 Blob 連結到使用者模型 (例如，與個人檔案的圖片建立關聯)。 下圖顯示相關使用者 API 端點和任何必要的路徑參數，例如 `id`：

![使用者 Blob][4]

例如，若要擷取連結至使用者的 Blob，請發出已驗證的 HTTP GET 要求，並搭配任何必要的表單資料：

```plaintext
YOUR_MANAGEMENT_API_URL/users/blobs/YOUR_BLOB_ID
```

| 參數 | 更換為 |
| --- | --- |
| *YOUR_BLOB_ID* | 所需的 Blob 識別碼 |

成功的要求傳回 JSON 物件，如[先前所述](#blobModel)。

## <a name="common-errors"></a>常見錯誤

常見錯誤與未提供正確標頭資訊有關：

```JSON
{
    "error": {
        "code": "400.600.000.000",
        "message": "Invalid media type in first section."
    }
}
```

若要解決這個錯誤，請確認整體要求具有適當的 **Content-Type** 標頭：

* `multipart/mixed`
* `multipart/form-data`

此外，視需要確認每個多部分區塊都有對應的 **Content-Type**。

## <a name="next-steps"></a>後續步驟

- 若要深入了解 Azure Digital Twins 的 Swagger 參考文件，請參閱[使用 Azure Digital Twins Swagger](how-to-use-swagger.md)。

- 若要透過 Postman 上傳 Blob，請參閱[如何設定 Postman](./how-to-configure-postman.md)。

<!-- Images -->
[1]: media/how-to-add-blobs/blob-models.PNG
[2]: media/how-to-add-blobs/blobs-device-api.PNG
[3]: media/how-to-add-blobs/blobs-space-api.PNG
[4]: media/how-to-add-blobs/blobs-users-api.PNG
[5]: media/how-to-add-blobs/curl.PNG
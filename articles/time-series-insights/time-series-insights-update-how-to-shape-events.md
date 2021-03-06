---
title: 使用 Azure 時間序列深入解析為事件塑形 | Microsoft Docs
description: 了解如何使用 Azure 時間序列深入解析預覽版為事件塑形。
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: eb398ad621167ad9f9b245fb8aa98c6942b87668
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2018
ms.locfileid: "53557422"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>使用 Azure 時間序列深入解析為事件塑形

此文章可協助您為 JSON 檔案塑形以最佳化 Azure 時間序列深入解析預覽版查詢的效率。

## <a name="best-practices"></a>最佳作法

請務必考慮如何將事件傳送至時間序列深入解析預覽版。 也就是說，您應該一律：

* 盡可能有效率地透過網路傳送資料。
* 以可協助您針對您的案例更適當地彙總資料的方式存放資料。

為獲得最佳查詢效能，請執行下列動作：

* 請勿傳送不必要的屬性。 時間序列深入解析預覽版會根據您的使用量向您收費。 最好存放並存取您將查詢的資料。
* 為統計資料使用執行個體欄位。 此實務可協助您避免透過網路傳送統計資料。 執行個體欄位 (時間序列模型的元件) 的運作方式就像時間序列深入解析公開推出服務中參考資料的運作方式一樣。 若要深入了解執行個體欄位，請參閱[時間序列模型](./time-series-insights-update-tsm.md)。
* 在兩個或更多事件之間共用維度屬性。 此實務可協助您更有效率地透過網路傳送資料。
* 請勿使用深層陣列巢狀結構。 時間序列深入解析預覽版支援最多兩層包含物件的巢狀陣列。 時間序列深入解析會將訊息中的陣列壓平合併為具有屬性值組的多個事件。
* 如果所有或大部分的事件只存在少數的量值，最好將這些量值當作相同物件內的個別屬性來傳送。 個別傳送可減少事件數目，因為需要處理的事件數目較少，此實務可能會讓查詢效能比較好。

## <a name="example"></a>範例

下列範例是以兩部或多部裝置傳送度量或訊號的案例為基礎。 這些度量或訊號可以是流動率、引擎機油壓力、溫度與溼度。

在下列範例中，有一個 IoT 中樞訊息，其中外部陣列包含通用維度值的共用區段。 外部陣列使用時間序列執行個體資料來提高訊息的效率。 時間序列執行個體包含裝置中繼資料，這些中繼資料不會隨著每個事件而變更，但它確實提供可用於資料分析的實用屬性。 為減少透過網路傳送的大小並讓訊息更有效率，請考慮為通用維度值進行批次處理並採用時間序列執行個體中繼資料。

### <a name="example-json-payload"></a>範例 JSON 承載

```JSON
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate psi": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

### <a name="time-series-instance"></a>時間序列執行個體 
> [!NOTE]
> 時間序列識別碼是 *deviceId*。

```JSON
{
    "timeSeriesId": [
      "FXXX"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "REVOLT SIMULATOR",
      "L2": "Battery System",
    }
  },
  {
    "timeSeriesId": [
      "FYYY"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "COMMON SIMULATOR",
      "L2": "Battery System",
    }
  },
```

時間序列深入解析預覽版會在查詢期間聯結資料表 (在壓平之後)。 該資料表包括額外的資料行，例如**類型**。 下列範例示範如何為您的遙測資料[塑形](./time-series-insights-send-events.md#json)：

| deviceId  | 類型 | L1 | L2 | timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | REVOLT SIMULATOR | Battery System | 2018-01-17T01:17:00Z |    1.0172575712203979 |    34.7 |
| `FXXX` | LINE_DATA    REVOLT | SIMULATOR |    Battery System |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | LINE_DATA    COMMON | SIMULATOR |    Battery System |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

在上面的範例中，請注意下列各點：

* 統計屬性是存放在時間序列深入解析預覽版中，以最佳化透過網路傳素的資料。
* 系統會在查詢期間使用執行個體中定義的 *timeSeriesId* 來聯結時間序列深入解析預覽版資料。
* 使用兩層的巢狀結構，它們是時間序列深入解析預覽版支援的數目上限。 請務必避免巢狀結構很深的陣列。
* 因為有一些量值，它們會在相同的物件中以不同的屬性傳送。 在此範例中，**series.Flow Rate psi**、**series.Engine Oil Pressure psi** 與 **series.Flow Rate ft3/s** 是唯一資料行。

>[!IMPORTANT]
> 執行個體欄位不會隨著遙測存放。 它們會隨著中繼資料存放在**時間序列模型**中。
> 上面的資料表代表查詢檢視。

## <a name="next-steps"></a>後續步驟

若要實作這些指導方針，請參閱 [Azure 時間序列深入解析預覽版查詢語法](./time-series-insights-query-data-csharp.md)。 您將了解時間序列深入解析預覽版資料存取 REST API 的查詢語法。

若要了解支援的 JSON 塑形，請參閱[支援的 JSON 塑形](./time-series-insights-send-events.md#json)。

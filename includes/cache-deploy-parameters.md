---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: dd9700c9472e07daf294eca12b766e3dc4832955
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111941"
---
### <a name="cacheskuname"></a>cacheSKUName
新的 Azure Cache for Redis 的定價層。

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Cache for Redis."
      }
    },

此範本定義此參數允許的值 (基本或標準)，並指派未指定任何值時的預設值 (基本)。 「基本」提供單一節點，有多種大小可用，最大為 53 GB。
「標準」提供雙節點「主要/複本」，有多種大小可用，最大為 53 GB，還有高達 99.9% 的 SLA。

### <a name="cacheskufamily"></a>cacheSKUFamily
Sku 系列。

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
新 Azure Cache for Redis 執行個體的大小。 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Cache for Redis instance. "
      }
    }


範本會定義此參數允許使用的值 (0、1、2、3、4、5 或 6)，並指派未指定任何值時的預設值 (0)。 這些數字對應到下列快取大小：0 = 250 MB、1 = 1 GB、2 = 2.5 GB、3 = 6 GB、4 = 13 GB、5 = 26 GB、6 = 53 GB


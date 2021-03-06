---
title: Azure Cosmos DB 中的要求單位和輸送量
description: 了解如何在 Azure Cosmos DB 中指定和預估要求單位需求
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: 3801c19fbef70bf5d67670c4d9acc950291853e6
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990150"
---
# <a name="request-units-in-azure-cosmos-db"></a>Azure Cosmos DB 中的要求單位

使用 Azure Cosmos DB，您需要按小時支付所佈建的輸送量及所取用的儲存體。 您必須佈建輸送量，確保有足夠的系統資源隨時都可供您的 Azure Cosmos 資料庫使用。 您需要足夠的資源來滿足或超過 Azure Cosmos DB SLA。

Azure Cosmos DB 支援許多 API，例如 SQL、MongoDB、Cassandra、Gremlin 和資料表。 每個 API 都有它自己的一組資料庫作業。 這些作業的範圍可從簡單的點讀取和寫入到複雜查詢。 每個資料庫作業都會根據作業的複雜度來取用系統資源。 

所有資料庫作業的成本會由 Azure Cosmos DB 進行正規化，並以要求單位 (RU) 表示。 讀取 1-KB 項目的成本是 1 個要求單位 (RU)。 所有其他資料庫作業同樣會以 RU 來指派成本。 無論您使用哪種 API 與 Azure Cosmos 容器互動，成本始終由 RU 測量。 無論資料庫作業是寫入、讀取或查詢，成本一律以 RU 為單位進行測量。

您可以將每秒 RU 想像為輸送量的貨幣。 每秒 RU 是以速率為基礎的貨幣。 它會擷取執行 Azure Cosmos DB 所支援之資料庫作業所需的系統資源 (例如 CPU、IOPS 和記憶體)。 下圖顯示不同資料庫作業所取用的 RU：

![資料庫作業會取用要求單位](./media/request-units/request-units.png)

為了管理和規劃容量，Azure Cosmos DB 會確保指定資料集上指定資料庫作業的 RU 數目具有決定性。 檢查回應標頭來追蹤任何資料庫作業所取用的 RU 數目。 當您了解影響 RU 費用的因素，以及應用程式的輸送量需求之後，就可以最符合經濟效益的方式來執行應用程式。

您要按小時付費。 您還是會以每秒為單位，以遞增 100 個 RU/秒的方式來佈建應用程式的 RU 數目。 若要調整應用程式的佈建輸送量，您可以隨時增加或減少 RU 數目。 以 100 RU 的增量或減量進行變更。 您可以程式設計方式或使用 Azure 入口網站進行變更。

您可以在兩個不同的資料粒度上佈建輸送量： 

* **容器**。 如需詳細資訊，請參閱[在 Azure Cosmos 容器上佈建輸送量](how-to-provision-container-throughput.md)。
* **資料庫**。 如需詳細資訊，請參閱[在 Azure Cosmos 資料庫上佈建輸送量](how-to-provision-database-throughput.md)。

## <a name="request-unit-considerations"></a>要求單位的考量

雖然您預估每秒佈建的的 RU 數目，但請考慮下列因素：

* **項目大小**：當項目的大小增加時，取用來讀取或寫入該項目的 RU 數目也會增加。

* **編製索引**：預設會自動編製每個項目的索引。 如果您選擇不要在容器中編製某些項目的索引，即會取用較少的 RU。

* **項目屬性計數**：假設預設會在所有屬性上編製索引，則取用來寫入項目的 RU 數目會隨著項目屬性計數增加而增加。

* **已編製索引的屬性**：每個容器上的索引原則會判定預設要編製哪些索引的屬性。 若要減少寫入作業的要求單位取用量，請限制已編製索引的屬性數目。

* **資料一致性**：相較於其他寬鬆一致性層級的讀取作業，強式和限定過期一致性層級在執行讀取作業時所取用 RU 大約是 2 倍以上。

* **查詢模式**：查詢的複雜性會影響針對作業所耗用的要求單位數目。 影響查詢作業成本的因素包括： 
    
    - 查詢結果數目。
    - 述詞數目。
    - 述詞性質。
    - 使用者定義函式數目。
    - 來源資料的大小。
    - 結果集的大小。
    - 投影。

  Azure Cosmos DB 保證相同資料上相同查詢的成本，一律會與重複執行上的 RU 數目相同。

* **指令碼使用方式**：和查詢一樣，預存程序和觸發程序都會根據所執行作業的複雜度來取用 RU。 開發應用程式時，請檢查要求費用標頭，以便進一步了解每個作業要取用多少 RU 容量。

## <a name="next-steps"></a>後續步驟

* 深入了解如何[佈建 Azure Cosmos 容器和資料庫的輸送量](set-throughput.md)。
* 深入了解[邏輯分割區](partition-data.md)。
* 深入了解如何[全域調整佈建的輸送量](scaling-throughput.md)。
* 了解如何[在 Azure Cosmos 容器上佈建輸送量](how-to-provision-container-throughput.md)。
* 了解如何[在 Azure Cosmos 資料庫上佈建輸送量](how-to-provision-database-throughput.md)。

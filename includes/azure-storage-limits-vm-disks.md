---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 2936fd318f08c74675f7e8b382c861f4a28319fc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261326"
---
您可以將多個資料磁碟附加至 Azure 虛擬機器。 根據 VM 的資料磁碟的延展性和效能目標，您可以判斷您需要以符合您的效能和容量需求的磁碟類型與數量。

> [!IMPORTANT]
> 為達最佳效能，限制連結至虛擬機器的高度使用磁碟數目，以避免可能的節流。 如果所有連接的磁碟不會在同一時間高度使用的虛擬機器可以支援更大量的磁碟。

**適用於 Azure 受控磁碟：**

下表說明的預設值和最大限制數目的每個訂用帳戶區域的資源

> | 資源 | 預設限制  | 上限 |
> | --- | --- | --- |
> | 標準受控磁碟 | 25,000 | 50,000 |
> | 標準 SSD 受控磁碟 | 25,000 | 50,000 |
> | 進階受控磁碟 | 25,000 | 50,000 |
> | Standard_LRS 快照集 | 25,000 | 50,000 |
> | Standard_ZRS 快照集 | 25,000 | 50,000 |
> | 受控映像 | 25,000 | 50,000 |

* **標準儲存體帳戶：** 標準儲存體帳戶有總要求率上限為 20000 IOPS。 所有標準儲存體帳戶中虛擬機器磁碟的 IOPS 總數不得超過此限制。
  
    您可以大致計算單一標準儲存體帳戶，根據要求率限制所支援的高度使用磁碟數目。 例如，基本層 VM，高度使用的磁碟數目上限是大約 66，每一磁碟 20000/300 IOPS。 高度使用的標準層 VM 的磁碟數目上限約為 40，也就是每一磁碟 20000/500 IOPS。 

* **進階儲存體帳戶：** 進階儲存體帳戶有總輸送量速率上限為 50 Gbps。 所有 VM 磁碟的總輸送量不應該超過此限。


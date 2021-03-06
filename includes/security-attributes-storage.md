---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: d8e33113ca9f0886a4cef1c8f9acb855b32c2973
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2019
ms.locfileid: "58115582"
---
## <a name="preventative"></a>預防

| 安全性屬性 | 是/否 | 注意 |
|---|---|--|
| 待用加密：<ul><li>伺服器端加密</li><li>使用客戶管理的金鑰進行伺服器端加密</li><li>其他加密功能 (例如用戶端、一律加密等)</ul>| 是 |  |
| 傳輸中加密：<ul><li>Express Route 加密</li><li>VNet 中加密</li><li>VNet-VNet 加密</ul>| 是 | 支援標準的 HTTPS/TLS 機制。  它會傳送至服務之前，使用者也可以加密資料。 |
| 加密金鑰處理 (CMK、BYOK 等)| 是 | 請參閱[在 Azure Key Vault 中使用客戶管理金鑰的儲存體服務加密](../articles/storage/common/storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)。|
| 資料行層級加密 (Azure 資料服務)| N/A |  |
| API 呼叫加密| 是 |  |

## <a name="network-segmentation"></a>網路分割

| 安全性屬性 | 是/否 | 注意 |
|---|---|--|
| 服務端點支援| 是 |  |
| vNET 插入支援| N/A |  |
| 網路隔離/防火牆支援| 是 | |
| 支援強制通道 | N/A |  |

## <a name="detection"></a>偵測

| 安全性屬性 | 是/否 | 注意|
|---|---|--|
| Azure 監視支援 (Log Analytics、App Insights 等)| 是 | 現在，可用的 azure 監視器計量記錄的開始預覽 |

## <a name="iam-support"></a>IAM 支援

| 安全性屬性 | 是/否 | 注意|
|---|---|--|
| 存取管理 - 驗證| 是 | Azure Active Directory 中，共用金鑰、 共用存取權杖。 |
| 存取管理 - 授權| 是 | 支援透過 RBAC、 POSIX Acl 中和 SAS 權杖的授權 |


## <a name="audit-trail"></a>稽核線索

| 安全性屬性 | 是/否 | 注意|
|---|---|--|
| 控制/管理計劃記錄和稽核 | 是 | Azure Resource Manager 活動記錄檔 |
| 資料平面記錄和稽核| 是 | 服務診斷記錄和 Azure 監視器記錄開始預覽  |

## <a name="configuration-management"></a>組態管理

| 安全性屬性 | 是/否 | 注意|
|---|---|--|
| 組態管理支援 (組態的版本控制等)| 是 | 支援透過 Azure Resource Manager Api 的資源提供者版本控制 |
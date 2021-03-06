---
title: 包含檔案
description: 包含檔案
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 02/07/2019
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: c6c57390e0a2fba0c79d3198df0f5577eb813f88
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553399"
---
<a name="virtual-networking-limits-classic"></a>下列限制僅適用於透過每個訂用帳戶的傳統部署模型所管理的網路資源。 深入了解如何[根據您的訂用帳戶限制檢視目前資源使用量](../articles/networking/check-usage-against-limits.md)。

| 資源 | 預設限制 | 上限 |
| --- | --- | --- |
| 虛擬網路 |50 |100 |
| 區域網路網站 |20 |請連絡支援人員。 |
| 每個虛擬網路的 DNS 伺服器 |20 |20 |
| 每個虛擬網路的私人 IP 位址 |4,096 |4,096 |
| 虛擬機器或角色執行個體之每個 NIC 的並行 TCP 或 UDP 流程 |500000，最多兩個或多個 Nic 的 1,000,000。 |500000，最多兩個或多個 Nic 的 1,000,000。 |
| 網路安全性群組 (NSG) |100 |200 |
| 每个 NSG 的 NSG 规则数 |200 |1,000 |
| 使用者定義的路由表 |100 |200 |
| 使用者定義的路由，每個路由表 |100 |400 |
| 公用 IP 位址 (動態) |5 |請連絡支援人員。 |
| 保留的公用 IP 位址 |20 |請連絡支援人員。 |
| 每個部署的公用 VIP |5 |請連絡支援人員。 |
| 每個部署私人 VIP （內部負載平衡） |1 |1 |
| 端點存取控制清單 (Acl) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>網路限制-Azure Resource Manager
下列限制僅適用於透過每個訂用帳戶每一區域的 Azure Resource Manager 所管理的網路資源。 深入了解如何[根據您的訂用帳戶限制檢視目前資源使用量](../articles/networking/check-usage-against-limits.md)。

> [!NOTE]
> 我們最近已將所有預設限制提升至其最大限制。 如果沒有最大限制的資料行，該資源不會有可調整的限制。 如果您在過去支援增加這些限制，看不到更新的限制，在下列資料表中，[開啟線上客戶支援要求，不另外加收費用](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| 資源 | 預設限制 | 
| --- | --- |
| 虛擬網路 |1,000 |
| 每一虛擬網路的子網路 |3,000 |
| 每個虛擬網路的虛擬網路對等互連 |100 |
| 每個虛擬網路的 DNS 伺服器 |20 |
| 每個虛擬網路的私人 IP 位址 |65,536 |
| 每個網路介面的私人 IP 位址 |256 |
| 每個虛擬機器的私人 IP 位址 |256 |
| 虛擬機器或角色執行個體之每個 NIC 的並行 TCP 或 UDP 流程 |500,000 |
| 網路介面卡 |65,536 |
| 網路安全性群組 |5,000 |
| 每一 NSG 的 NSG 規則 |1,000 |
| 針對安全性群組中的來源或目的地所指定的 IP 位址和範圍 |4,000 |
| 應用程式安全性群組 |3,000 |
| 每個 NIC、每個 IP 組態的應用程式安全性群組 |20 |
| 每個應用程式安全性群組的 IP 組態 |4,000 |
| 可在網路安全性群組的所有安全性規則內指定的應用程式安全性群組 |100 |
| 使用者定義的路由表 |200 |
| 使用者定義的路由，每個路由表 |400 |
| 每個 Azure VPN 閘道的點對站根憑證 |20 |
| 虛擬網路 TAP |100 |
| 每個虛擬網路 TAP 的網路介面 TAP 設定 |100 |

#### <a name="publicip-address"></a>公用 IP 位址限制
| 資源 | 預設限制 | 上限 |
| --- | --- | --- |
| 公用 IP 位址 - 動態 | 適用於 Basic 1,000。 |請連絡支援人員。 |
| 公用 IP 位址 - 靜態 | 適用於 Basic 200。 |請連絡支援人員。 |
| 公用 IP 位址 - 靜態 | 標準的 200。|請連絡支援人員。 |
| 公用 IP 前置詞大小 （預覽） | /28 | /28 |

#### <a name="load-balancer"></a>負載平衡器限制
下列限制僅適用於透過每個訂用帳戶每一區域的 Azure Resource Manager 所管理的網路資源。 深入了解如何[根據您的訂用帳戶限制檢視目前資源使用量](../articles/networking/check-usage-against-limits.md)。

| 資源 | 預設限制 |
| --- | --- |
| 負載平衡器 | 1,000 | 
| 每個資源的規則，基本 | 250 |
| 每個資源的規則，標準 | 1,500 | 
| 每個 IP 設定的規則 | 299 |
| 每個 NIC 的規則 | 500 |
| 前端 IP 設定基本 | 200 |
| 前端 IP 設定標準 | 600 |
| 後端集區基本 | 100，單一可用性設定組 |
| 後端集區標準 | 1000，單一虛擬網路 |
| 每個負載平衡器，標準的後端資源<sup>1</sup> | 150 |
| 高可用性連接埠標準 | 每 1 個內部前端 |

<sup>1</sup>的限制是中獨立虛擬機器資源的任何組合的 150 部資源、 資源和虛擬機器擴展集資源的可用性設定組。


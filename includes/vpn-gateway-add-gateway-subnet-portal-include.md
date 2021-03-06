---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9132cf438cab518e20e6c2ddfdb7d0928753bd19
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444022"
---
1. 在入口網站中，瀏覽至要建立虛擬網路閘道的虛擬網路。
2. 在 VNet 頁面的 [設定] 中，按一下 [子網路] 以展開 [子網路] 頁面。
3. 在 [子網路] 頁面上，按一下頂端的 [+閘道子網路] 以開啟 [新增子網路] 頁面。

   ![新增閘道子網路](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "新增閘道子網路")
  
4. 子網路的 [名稱] 會自動填入 'GatewaySubnet' 這個值。 為了讓 Azure 將此子網路視為閘道子網路，需要有 GatewaySubnet 值。 調整自動填入的 [位址範圍] 值，以符合您的組態需求。

   ![新增閘道子網路](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet.png "新增閘道子網路")
  
5. 若要建立子網路，請按一下頁面底部的 [確定]。

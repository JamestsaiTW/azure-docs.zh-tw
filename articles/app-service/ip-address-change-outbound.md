---
title: 為連出 IP 位址變更做準備 - Azure App Service
description: 如果您的輸出 IP 位址即將變更，請了解該怎麼做，以讓您的應用程式可在變更後繼續運作。
services: app-service\web
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: ac62217af096653d61a79ff29ae352c8e950f8af
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2018
ms.locfileid: "53719297"
---
# <a name="how-to-prepare-for-an-outbound-ip-address-change"></a>如何為輸出 IP 位址變更進行準備

如果您收到通知 Azure App Service 應用程式的輸出 IP 位址即將變更，請遵循本文中的指示。

## <a name="determine-if-you-have-to-do-anything"></a>判斷您是否需要執行任何動作

* 選項 1：如果您的 App Service 應用程式並未使用 IP 篩選、包含絕對路徑清單或連出流量的特殊處理 (例如路由或防火牆)，則不需執行任何動作。

* 選項 2：如果您的應用程式確實具有連出 IP 位址的特殊處理 (請參閱以下範例)，請在現有位址出現的所有位置新增新的連出 IP 位址。 請勿取代現有的 IP 位址。 遵循下一節中的指示，以找出新的輸出 IP 位址。

  比方說，輸出 IP 位址可能會明確包含在您應用程式外部的防火牆中，或者外部付款服務可能具有包含應用程式輸出 IP 位址的允許清單。 如果您的輸出位址是在應用程式以外的清單中進行設定，則需要變更。

## <a name="find-the-outbound-ip-addresses-in-the-azure-portal"></a>在 Azure 入口網站中找出輸出 IP 位址

新的輸出 IP 位址在生效之前即會顯示在入口網站。 當 Azure 開始使用新的位址時，將不再使用舊位址。 一次只會使用一組位址，因此包含清單中的項目必須同時具有新舊 IP 位址以避免切換時發生中斷。 

1.  開啟 [Azure 入口網站](https://portal.azure.com)。

2.  在左側導覽功能表中選取 [應用程式服務]。

3.  從清單中選取 App Service 應用程式。

1.  若該應用程式為函式應用程式，請參閱[函式應用程式輸出 IP 位址](../azure-functions/ip-addresses.md#find-outbound-ip-addresses)。

4.  在 [設定] 標題底下，按一下左側導覽中的 [屬性]，並找到標示為 [輸出 IP 位址] 的區段。

5. 複製 IP 位址，並將其新增至您輸出流量的特殊處理，例如篩選或允許的清單。 請勿刪除清單中現有的 IP 位址。

## <a name="next-steps"></a>後續步驟

本文說明如何針對由 Azure 起始的 IP 位址變更進行準備。 如需有關 Azure App Service 中 IP 位址的詳細資訊，請參閱 [Azure App Service 中的輸入和輸出 IP 位址](overview-inbound-outbound-ips.md)。

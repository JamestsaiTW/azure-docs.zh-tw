---
title: Azure PowerShell 指令碼範例 - 訂閱資源群組和依資源進行篩選 | Microsoft Docs
description: Azure PowerShell 指令碼範例 - 訂閱資源群組和依資源進行篩選
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/10/2018
ms.author: tomfitz
ms.openlocfilehash: 21719ab4e6b999f262ff53adf31d855d6e1833b4
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271397"
---
# <a name="subscribe-to-events-for-a-resource-group-and-filter-for-a-resource-with-powershell"></a>使用 PowerShell 訂閱資源群組的事件和篩選資源

此指令碼可針對資源群組的事件建立 Event Grid 訂用帳戶。 它會使用篩選條件以便只取得資源群組中指定資源的事件。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

預覽範例指令碼需要事件方格模組。 若要安裝，請執行 `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

## <a name="sample-script---stable"></a>範例指令碼 - Stable

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events/filter-events.ps1 "Filter events")]

## <a name="sample-script---preview-module"></a>範例指令碼 - 預覽版模組

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events-preview/filter-events-preview.ps1 "Filter events")]


## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令來建立事件訂用帳戶。 下表中的每個命令都會連結至命令特定的文件。

| 命令 | 注意 |
|---|---|
| [New-AzureRmEventGridSubscription](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridsubscription) | 建立事件格線訂用帳戶。 |

## <a name="next-steps"></a>後續步驟

* 如需受控應用程式的簡介，請參閱 [Azure 受控應用程式概觀](../overview.md)。
* 如需 PowerShell 的詳細資訊，請參閱 [Azure PowerShell 文件](https://docs.microsoft.com/powershell/azure/get-started-azureps)。

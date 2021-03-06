---
title: Azure PowerShell 指令碼範例 - 取得受控資源群組並調整 VM 的大小 | Microsoft Docs
description: Azure PowerShell 指令碼範例 - 取得受控資源群組並調整 VM 的大小
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: 9e8930c95495673c0082a82757ed6d8137900b6f
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110892"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>使用 PowerShell 取得受控資源群組中的資源並調整 VM 大小

此指令碼會從受控資源群組擷取資源，並調整該資源群組中 VM 的大小。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>範例指令碼

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>指令碼說明

此指令碼會使用下列命令來部署受控應用程式。 下表中的每個命令都會連結至命令特定的文件。

| 命令 | 注意 |
|---|---|
| [Get-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | 列出受控應用程式。 提供用來聚焦結果的資源群組名稱。 |
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | 列出資源。 提供用來聚焦結果的資源群組和資源類型。 |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | 更新虛擬機器的大小。 |


## <a name="next-steps"></a>後續步驟

* 如需受控應用程式的簡介，請參閱 [Azure 受控應用程式概觀](../overview.md)。
* 如需 PowerShell 的詳細資訊，請參閱 [Azure PowerShell 文件](https://docs.microsoft.com/powershell/azure/get-started-azureps)。

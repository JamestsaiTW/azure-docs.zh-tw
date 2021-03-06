---
title: Microsoft Azure Marketplace 常見 SAS URL 問題及修正 | Microsoft Docs
description: 列出使用共用存取簽章 URI 的常見問題和可能的解決方案。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/27/2018
ms.author: pbutlerm
ms.openlocfilehash: cdee17185b7051220f66ede3b9da50a333409e6d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "58119257"
---
# <a name="common-sas-url-issues-and-fixes"></a>常見 SAS URL 問題及修正

下表列出一些使用共用存取簽章 (用於識別及共享解決方案之上傳 VHD) 時常見問題，以及建議的解決方案。

| **問題** | 失敗訊息 | 修正 | 
| --------- | ------------------- | ------- | 
| &emsp;  *複製映像失敗* |  |  |
| 在 SAS URL 中找不到 "?" | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 使用建議工具更新 SAS URL。 |
| "st" 和 "se" 參數不在 SAS URL 中 | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 更新 SAS URL，將其中**開始日期**與**結束日期**修改為恰當的值。 | 
| "sp = rl" 不在 SAS URL 中 | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | 更新 SAS Url，將權限設定為`Read`和`List`。 | 
| SAS URL 中的 VHD 名稱包含空格 | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 更新 SAS URL 以移除空格。 |
| SAS URL 授權錯誤 | `Failure: Copying Images. Not able to download blob due to authorization error` | 檢閱並修正 SAS URI 格式。 必要時請重新產生。 |
| SAS URL 之 "st" 和 "se" 參數沒有完整的日期時間規格 | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | SAS URL **開始日期**與**結束日期**參數 (`st`和` se`子字串) 都需要完整的日期時間格式，例如：`11-02-2017T00:00:00Z`。 縮短版本無效。 (Azure CLI 中的某些命令可能會預設產生縮短的值)。 | 
|  |  |  |

如需詳細資訊，請參閱[使用共用存取簽章 (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

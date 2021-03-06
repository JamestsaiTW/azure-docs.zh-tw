---
title: 取消及刪除 Azure 匯入/匯出作業 | Microsoft Docs
description: 了解如何取消及刪除 Microsoft Azure 匯入/匯出服務的作業。
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 6eb56413319208feef1b4ab51296fe12a1e0bcf2
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/04/2019
ms.locfileid: "55700123"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>取消及刪除 Azure 匯入/匯出作業

 若要要求讓作業在進入 `Packaging` 狀態之前予以取消，請呼叫 [Update Job Properties](/rest/api/storageimportexport/jobs) 作業並將 `CancelRequested` 元素設定為 `true`。 系統會盡可能地取消作業。 如果磁碟機正在傳輸資料，即使已經要求取消，資料可能會繼續傳送。

 取消的作業會進入 `Completed` 狀態並保留 90 天，90 天過後就會遭到刪除。

 若要刪除作業，在傳送作業之前請先呼叫 [Delete Job](/rest/api/storageimportexport/jobs) 作業，(也就是當作業處於 `Creating` 狀態時)。 您也可以在作業為 `Completed` 狀態時將它刪除。 作業遭到刪除後，便無法再透過 REST API 或 Azure 入口網站存取其資訊和狀態。

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>後續步驟

* [使用匯入/匯出服務 REST API](storage-import-export-using-the-rest-api.md)

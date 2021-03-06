---
title: 更新現有的 Azure 容器供應項目 | Microsoft Docs
description: 如何更新 Azure Marketplace 上的現有容器供應項目。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 2b568717b6656fb9ae15e9a6dbd27441689c4372
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980159"
---
# <a name="update-an-existing-container-offer"></a>更新現有的容器供應項目

本文將逐步說明在 [Cloud Partner 入口網站](https://cloudpartner.azure.com/)中更新容器供應項目的各個層面。

您可能會因為某些原因而想要更新供應項目，例如：

-  將新的容器映像版本新增至現有的 SKU。
-  新增 SKU。
-  更新供應項目或個別 SKU 的市集中繼資料。

為了協助您修改這些項目，入口網站提供了 [比較] 與 [歷程記錄] 功能。  


## <a name="unpermitted-changes-to-a-container-offer-or-sku"></a>不允許對容器供應項目或 SKU 進行的變更

供應項目於 Azure Marketplace 上線後，便無法變更容器供應項目或 SKU 的某些屬性。 您無法變更下列設定：

-  供應項目的**供應項目識別碼**和**發行者識別碼**
-  現有 SKU 的 **SKU** 識別碼
-  版本標記，例如：`1.0.1`
-  現有 SKU 的計費/授權模式變更

## <a name="common-update-operations"></a>一般更新作業

下列更新作業很常見。

### <a name="update-container-image-version-for-a-sku"></a>更新 SKU 的容器映像版本

使用安全性修補程式、額外功能等等定期更新容器映像很常見。 在此案例中，您要使用以下步驟更新 SKU 參考的容器映像：

1. 登入 [Cloud Partner 入口網站](https://cloudpartner.azure.com/)。
2. 在 [所有供應項目] 之下，尋找您要更新的供應項目。
3. 在 [SKU] 索引標籤中，選取與容器映像關聯的 SKU，即可執行更新。
4. 在 [容器映像] 底下，選取 [+ 新增新的映像版本] 來新增容器映像。
5. 提供新的容器**映像版本**。 映像版本須遵循與舊版相同的標記指導方針。 版本格式應該是 X.Y.Z，其中 X、Y 和 Z 是整數。 請確認您提供的新版本大於所有先前的版本。
6. 按一下 [發佈] 啟動工作流程，將新的容器映像版本發佈至 Azure Marketplace。

### <a name="add-a-new-sku"></a>新增 SKU

依照下列步驟，讓您的供應項目可使用新的 SKU：

1. 登入 [Cloud Partner 入口網站](https://cloudpartner.azure.com/)。
2. 在 [所有供應項目] 之下，尋找您要更新的供應項目。
3. 在 [SKU] 索引標籤下方，按一下 [新增 SKU]，並在快顯視窗中提供 [SKU 識別碼]。
4. 使用[發佈容器供應項目](./cpp-publish-offer.md)中所述的步驟，重新發佈容器。
5. 選取 [發佈] 啟動工作流程，以發佈新 SKU。

### <a name="update-offer-marketplace-metadata"></a>更新供應項目市集中繼資料

使用下列步驟來更新與供應項目關聯的市集中繼資料。 (例如：公司名稱、標誌等等)

1. 登入 [Cloud Partner 入口網站](https://cloudpartner.azure.com/)。
2. 在 [所有供應項目] 下，尋找您要更新的供應項目。
3. 移至 [Marketplace ] 索引標籤。使用[發佈容器供應項目](./cpp-publish-offer.md)提供文件中的指示，進行中繼資料變更。
4. 選取 [發佈] 啟動工作流程，以發佈變更。

## <a name="compare-feature"></a>比較功能

當您變更已發佈的供應項目時，可以使用 [比較] 功能稽核已進行的變更。

### <a name="to-use-the-compare-feature"></a>若要使用 [比較] 功能：

1. 在編輯流程中的任何時間點，選取您供應項目的 [比較]。
2. 並排檢視行銷資產與中繼資料。


## <a name="history-of-publishing-actions"></a>發佈動作的歷程記錄

若要查看發佈活動記錄，請在 Cloud Partner 入口網站左邊導覽功能表列中，選擇 [記錄] 索引標籤。 您可以檢視在您的 Microsoft Azure Marketplace 供應項目生命週期期間發生的動作 (含時間戳記)。
---
title: Microsoft Azure Marketplace 中的虛擬機器供應項目 | Microsoft Docs
description: 在 Microsoft Azure Marketplace 上發佈虛擬機器供應項目的流程概觀。
services: Azure, Marketplace, Cloud Partner Portal
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: bbe757ccd1d6a37cbcf04f3ecd6dd088ef1ff211
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353005"
---
# <a name="virtual-machine-offer"></a>虛擬機器供應項目

|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| 本節說明如何將新的虛擬機器供應項目發佈到 [Azure Marketplace](https://azuremarketplace.microsoft.com)。 所提供的支援適用於 Windows 型和 Linux 型虛擬機器，其中包含作業系統虛擬硬碟 (VHD) 和零或多個資料 VHD。 | ![虛擬機器圖示](./media/virtual-machine-icon.png)  |


## <a name="publishing-overview"></a>發佈概觀

下列影片 ([將您的 Azure Marketplace 供應項目最佳化](https://channel9.msdn.com/Events/Build/2017/P4026?ocid=player) \(英文\)) 會呈現 Azure Marketplace 的要點，包括如何在此市集上進行發佈 (使用虛擬機器解決方案)、如何將產品頁面的使用者體驗和選擇性的試用產品體驗最佳化、如何產生使用者潛在客戶，以及您如何取用它們，並將客戶參與最佳化。

> [!VIDEO https://channel9.msdn.com/Events/Build/2017/P4026/player]


## <a name="vm-publishing-process-flow"></a>虛擬機器發佈流程

下圖說明大致的虛擬機器發佈流程。 

![虛擬機器發佈流程](./media/publishvm_001.png)

1. 建立供應項目：設定所有供應項目相關的詳細資料，包括供應項目說明、行銷資料、法律、支援資訊和資產的規格。

2. 建立商業及技術資產：建立相關解決方案 (此處為虛擬機器和附加磁碟) 的商業資產 (法律文件和行銷資料) 以及技術資產。 

3. 建立 SKU：建立與供應項目相關的 SKU 並提交。  您需要針對預計發佈的每個映像提供個別的 SKU。 
 
4. 認證及發佈供應項目：供應項目和技術資產完成後，便可以提交。 提交動作將啟動發佈流程，其中解決方案會經過測試、驗證、認證和核准，接著在市集上架。  

## <a name="next-steps"></a>後續步驟

在考慮進行下列步驟前，您必須符合發佈虛擬機器至 Microsoft Azure Marketplace[ 的技術和商務需求](./cpp-prerequisites.md)。 

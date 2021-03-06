---
title: 調整 Azure 資料總管叢集的大小，適應變更的需求
description: 本文說明相應增加和相應減少根據變更的隨選 Azure 資料總管叢集的步驟。
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: a74c529fc3543d5cbdcf009a5b7736309e15569e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961698"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>管理叢集相應增加以因應不斷變化的需求

適當調整叢集的大小對 Azure 資料總管的效能來說非常重要。 但是，在叢集上的需求無法預測以絕對的精確度。 靜態的叢集大小可能會導致使用量過低或使用量過高，兩者都不適合。

更好的方法是以*擴展*叢集中，新增及移除容量和與變更要求的 CPU 資源。 有兩個工作流程的縮放比例： 向上延展及向外延展。本文說明如何管理叢集相應增加。

1. 請移至您的叢集。 底下**設定**，選取**相應增加**。

    您會看到一份可用的 Sku。 例如，在下圖中，只有四個 Sku 可。

    ![相應增加](media/manage-cluster-scale-up/scale-up.png)

    Sku 會停用，因為它們目前的 SKU，或它們都無法使用叢集所在的區域中。

1. 若要變更您的 SKU，選取您想要並選擇的 SKU**選取** 按鈕。

> [!NOTE]
> 相應增加程序可能需要幾分鐘的時間，並在這段期間即會暫止您的叢集。 請注意，相應減少可能會導致叢集效能下降。

您現在已完成您的 Azure 資料總管叢集的相應增加或相應減少作業。 您也可以[管理叢集向外延展](manage-cluster-scale-out.md)動態相應放大的執行個體計數會根據您指定的計量。

如果您需要叢集調整問題，協助[開啟支援要求](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)在 Azure 入口網站中。

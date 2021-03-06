---
title: 收集 Azure Sentinel 預覽版中的 Azure ATP 資料 |Microsoft Docs
description: 了解如何收集 Azure ATP 的資料，在 Azure Sentinel。
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: 154af3988084792331db082b99cae0ae06126f1b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242168"
---
# <a name="collect-data-from-azure-advanced-threat-protection-atp"></a>收集的資料從 Azure 進階威脅防護 (ATP)

> [!IMPORTANT]
> Azure 的 Sentinel 目前處於公開預覽狀態。
> 此預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。


您可以從串流記錄[Azure 進階威脅防護](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp)到 Azure Sentinel 只要按一下。

## <a name="prerequisites"></a>必要條件

- 以全域系統管理員或安全性系統管理員權限的使用者
- 您必須是 Azure atp 私人預覽客戶

## <a name="connect-to-azure-atp"></a>連接到 Azure ATP

請確定 Azure ATP 私人預覽版本[在您網路上啟用](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1)。
如果 Azure ATP 會部署並擷取您的資料，可疑的警示可以輕鬆地串流到 Azure 的 Sentinel。 可能需要 24 小時的時間開始串流處理至 Azure 的 Sentinel 警示。



1. 在 Azure Sentinel，選取**資料收集**，然後按一下**Azure ATP**圖格。

2. 按一下 [ **連接**]。


## <a name="next-steps"></a>後續步驟
在本文件中，您已了解如何連接到 Azure Sentinel 的 Azure 進階威脅防護。 若要深入了解 Azure Sentinel，請參閱下列文章：
- 了解如何[了解您的資料，與潛在的威脅](quickstart-get-visibility.md)。
- 開始[偵測威脅與 Azure Sentinel](tutorial-detect-threats.md)。


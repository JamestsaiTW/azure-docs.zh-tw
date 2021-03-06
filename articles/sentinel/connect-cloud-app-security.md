---
title: 收集 Azure Sentinel 預覽版中的 Cloud App Security 資料 |Microsoft Docs
description: 了解如何收集在 Azure Sentinel 的 Cloud App Security 資料。
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: cd9e5e27-fdd4-4717-8924-be4c1c430f23
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: b0033f5f8636053f88825541b8b2cfcbf2fc9f8b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2019
ms.locfileid: "57245483"
---
# <a name="collect-data-from-microsoft-cloud-app-security"></a>從 Microsoft Cloud App Security 中收集資料 

> [!IMPORTANT]
> Azure 的 Sentinel 目前處於公開預覽狀態。
> 此預覽版本是在沒有服務等級協定的情況下提供，不建議用於生產工作負載。 可能不支援特定功能，或可能已經限制功能。 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

您可以從串流記錄[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security)到 Azure Sentinel 只要按一下。 此連線可讓您串流處理至 Azure 的 Sentinel 的從 Cloud App Security 警示。 

## <a name="prerequisites"></a>必要條件

- 以全域系統管理員或安全性系統管理員權限的使用者

## <a name="connect-to-cloud-app-security"></a>連接至 Cloud App Security

如果您已有 Cloud App Security，請確定它是[在您網路上啟用](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)。
如果部署 Cloud App Security，並擷取您的資料，警示資料可以輕鬆地串流到 Azure 的 Sentinel。


1. 在 Azure Sentinel，選取**資料收集**，然後按一下**Cloud App Security**圖格。

2. 按一下 [ **連接**]。

3. 若要使用 Log Analytics 中的 Cloud App Security 警示相關的結構描述，搜尋**SecurityAlert**。


## <a name="next-steps"></a>後續步驟
本文件中，您已了解如何在 Microsoft Cloud App Security 連線到 Azure 的 Sentinel。 若要深入了解 Azure Sentinel，請參閱下列文章：
- 了解如何[了解您的資料，與潛在的威脅](quickstart-get-visibility.md)。
- 開始[偵測威脅與 Azure Sentinel](tutorial-detect-threats.md)。

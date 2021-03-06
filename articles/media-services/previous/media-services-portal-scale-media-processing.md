---
title: 使用 Azure 入口網站調整媒體處理 | Microsoft Docs
description: 本教學課程將逐步引導您完成使用 Azure 入口網站調整媒體處理的步驟。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: c840764dc978a8dacb3450c0aca5e5d93284b8a6
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260106"
---
# <a name="change-the-reserved-unit-type"></a>更改预留单位类型
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [入口網站](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>概觀

媒體服務帳戶是與保留單元類型相關聯，後者決定媒體處理工作的速度。 您可以選擇下列的保留單元類型：**S1**、**S2** 或 **S3**。 例如，在執行相同編碼作業的前提下，使用 **S2** 保留單元類型的速度會比 **S1** 類型快。

除了指定保留單元類型之外，您還可以指定使用**保留單元** (RU) 來佈建帳戶。 佈建的 RU 數目可決定指定帳戶中可同時處理的媒體工作數目。

>[!NOTE]
>RU 用於平行化所有媒體處理，包括使用 Azure 媒體索引器的索引作業。 不過，與編碼不同，索引工作的處理速度不會因為使用較快的保留單元而變快。

> [!IMPORTANT]
> 請務必檢閱 [概觀](media-services-scale-media-processing-overview.md) 主題，以取得調整媒體處理主題的詳細資訊。
> 
> 

## <a name="scale-media-processing"></a>調整媒體處理
若要變更保留單元類型以及保留單元數目，請執行下列操作：

1. 在 [Azure 入口網站](https://portal.azure.com/)中，選取您的 Azure 媒體服務帳戶。
2. 在 [設定] 視窗中，選取 [媒體保留單元]。
   
    若要變更所選取保留單元類型的保留單元數目，請使用畫面頂端的 [媒體保留單元] 滑桿。
   
    若要變更 [保留單元類型]，按一下 [保留媒體處理器的速度] 列。 然後，選取您需要的定價層：S1、S2 或 S3。
   
3. 按 [儲存] 以儲存您的變更。
   
    新的保留單元會在您按 [儲存] 後配置。

## <a name="next-steps"></a>後續步驟
檢閱媒體服務學習路徑。

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>提供意見反應
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]


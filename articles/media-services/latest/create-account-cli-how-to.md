---
title: 使用 Azure CLI 建立媒體服務帳戶 - Azure | Microsoft Docs
description: 按照本快速入門的步驟來建立 Azure 媒體服務帳戶。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 02/15/2019
ms.author: juliako
ms.openlocfilehash: f4ce64599aad2b2eebbef6ca8d81acfca2a7a702
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342502"
---
# <a name="create-an-azure-media-services-account"></a>建立 Azure 媒體服務帳戶

若要在 Azure 中開始加密、編碼、分析、管理和串流處理媒體內容，您需要建立 Media Services 帳戶。 媒體服務帳戶必須與一或多個儲存體帳戶產生關聯。

> [!NOTE]
> 媒體服務帳戶和所有相關聯的儲存體帳戶必須位於相同的 Azure 訂用帳戶中。 強烈建議使用與媒體服務帳戶相同位置中的儲存體帳戶，以避免產生額外的延遲和資料輸出費用。

本文說明使用 Azure CLI 來建立新「Azure 媒體服務」帳戶的步驟。  

## <a name="prerequisites"></a>必要條件

有效的 Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) 。

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="set-the-azure-subscription"></a>設定 Azure 訂用帳戶

在下列命令中，提供您要用於媒體服務帳戶的 Azure 訂用帳戶識別碼。 您可以瀏覽至[訂閱](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)，以查看可以存取的訂用帳戶。

```azurecli
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>後續步驟

[串流處理檔案](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>另請參閱

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)


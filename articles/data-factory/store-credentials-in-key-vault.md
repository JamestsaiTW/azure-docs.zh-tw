---
title: 在 Azure Key Vault 中儲存認證 | Microsoft Docs
description: 了解如何為 Azure Key Vault 中使用的資料存放區儲存認證，Azure Data Factory 可以在執行階段自動擷取。
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: jingwang
ms.openlocfilehash: a7d440509e2b823400cde83c1ac2ec054c37eb74
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57896206"
---
# <a name="store-credential-in-azure-key-vault"></a>在 Azure Key Vault 中儲存認證

您可以在 [Azure Key Vault](../key-vault/key-vault-whatis.md) 中儲存供資料存放區和計算使用的認證。 執行活動 (該活動使用資料存放區/計算) 時，Azure Data Factory 會擷取認證。

目前，自訂活動以外的所有活動類型都支援這項功能。 特別針對連接器設定，查看[每個連接器主題](copy-activity-overview.md#supported-data-stores-and-formats)中的＜連結服務屬性＞一節以取得詳細資訊。

## <a name="prerequisites"></a>必要條件

這項功能會依賴 data factory 受控身分識別。 了解其運作方式從[受管理的 Data factory 的識別](data-factory-service-identity.md)，並確定您的 data factory 有相關聯。

## <a name="steps"></a>步驟

若要參考儲存在 Azure Key Vault 中的認證，您需要：

1. **擷取 data factory 受控身分識別**藉由複製"SERVICE IDENTITY APPLICATION ID"站一起產生的值。 如果您使用 ADF 撰寫 UI，受管理的身分識別應用程式識別碼將會顯示在 Azure 金鑰保存庫的連結的服務建立視窗;您也可以擷取它從 Azure 入口網站，請參閱[擷取 data factory 受控身分識別](data-factory-service-identity.md#retrieve-managed-identity)。
2. **受管理的身分識別存取權授與您的 Azure 金鑰保存庫。** 在金鑰保存庫-> 的存取原則]-> [新增-> 的搜尋此管理身分識別來授與的應用程式識別碼**取得**祕密權限下拉式清單中的權限。 這樣可以讓這個指定的處理站存取金鑰保存庫中的密碼。
3. **建立指向您的 Azure Key Vault 的已連結服務。** 請參閱 [Azure Key Vault 已連結服務](#azure-key-vault-linked-service)。
4. **建立資料存放區已連結服務，其中參考儲存在金鑰保存庫的對應密碼。** 請參閱[參考儲存在金鑰保存庫中的祕密](#reference-secret-stored-in-key-vault)。

## <a name="azure-key-vault-linked-service"></a>Azure Key Vault 已連結服務

以下是針對 Azure Key Vault 已連結服務支援的屬性：

| 屬性 | 描述 | 必要項 |
|:--- |:--- |:--- |
| type | 類型屬性必須設定為：**AzureKeyVault**。 | 是 |
| baseUrl | 指定 Azure Key Vault URL。 | 是 |

**使用編寫 UI：**

按一下 [連線] -> [連結的服務] -> [+ 新增] -> 搜尋「Azure Key Vault」：

![搜尋 AKV](media/store-credentials-in-key-vault/search-akv.png)

選取儲存認證的已佈建 Azure Key Vault。 您可以 [測試連線]，確保您的 AKV 連線有效。 

![設定 AKV](media/store-credentials-in-key-vault/configure-akv.png)

**JSON 範例：**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>參考儲存在金鑰保存庫中的祕密

當您在參考金鑰保存庫密碼的已連結服務中設定欄位時，支援下列屬性：

| 屬性 | 描述 | 必要項 |
|:--- |:--- |:--- |
| type | 欄位的類型屬性必須設定為：**AzureKeyVaultSecret**。 | 是 |
| secretName | Azure Key Vault 中密碼的名稱。 | 是 |
| secretVersion | Azure Key Vault 中密碼的版本。<br/>如果未指定，它會一律使用最新版本的密碼。<br/>如果指定，則它會遵循指定的版本。| 否 |
| store | 代表您用來儲存認證的 Azure Key Vault 已連結服務。 | 是 |

**使用編寫 UI：**

在建立資料存放區/計算的連線時，請針對祕密欄位選取 [Azure Key Vault]。 選取已佈建的 Azure Key Vault 連結服務，並提供 [祕密名稱]。 您也可以選擇性地提供祕密版本。 

>[!TIP]
>使用連接字串，例如 SQL Server、 Blob 儲存體等的連結服務中的連接器，您可以選擇將只有密碼欄位例如密碼儲存在 AKV 中，或是將整個連接字串儲存在 AKV。 您可以找到這兩個選項，在 UI 上。

![設定 AKV 祕密](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON 範例：(請參閱「密碼」一節)**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>後續步驟
如需 Azure Data Factory 中的複製活動所支援作為來源和接收器的資料存放區清單，請參閱[支援的資料存放區](copy-activity-overview.md#supported-data-stores-and-formats)。

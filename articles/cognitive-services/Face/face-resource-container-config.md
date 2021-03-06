---
title: 設定容器
titlesuffix: Face - Azure Cognitive Services
description: 適用於容器的組態設定。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: diberry
ms.openlocfilehash: 4215b008af21a3473a1d2dcef5f73a1b19133215
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821554"
---
# <a name="configure-face-docker-containers"></a>設定臉部 Docker 容器

**臉部**容器執行階段環境可使用 `docker run` 命令引數來設定。 此容器有數個必要的設定，和一些選擇性的設定。 命令有相關[範例](#example-docker-run-commands)可供參考。 容器專屬設定包括計費設定。 

容器設定是[階層式](#hierarchical-settings)的，可使用[環境變數](#environment-variable-settings)或 Docker [命令列引數](#command-line-argument-settings)來設定。

## <a name="configuration-settings"></a>組態設定

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> 系統會同時使用 [`ApiKey`](#apikey-configuration-setting)、[`Billing`](#billing-configuration-setting) 及 [`Eula`](#eula-setting) 設定，因此您必須同時為這三個設定提供有效的值，否則容器將不會啟動。 如需使用這些組態設定來將容器具現化的詳細資訊，請參閱[帳單](face-how-to-install-containers.md#billing)。

## <a name="apikey-configuration-setting"></a>ApiKey 組態設定

`ApiKey` 設定會指定用來追蹤容器帳單資訊的 Azure資源金鑰。 您必須指定 ApiKey 的值，該值必須是有效金鑰，且適用於為 [`Billing`](#billing-configuration-setting) 組態設定而指定的_臉部_資源。

此設定可在下列位置找到：

* Azure 入口網站：位於 [金鑰] 下方，**臉部**資源管理

## <a name="applicationinsights-setting"></a>ApplicationInsights 設定

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Billing 組態設定

`Billing` 設定會指定 Azure 上用來計量容器帳單資訊的_臉部_資源端點 URI。 您必須為此組態設定指定值，且該值必須是適用於 Azure 上_臉部_資源的有效端點 URI。 容器會每隔 10 到 15 分鐘回報使用量。

此設定可在下列位置找到：

* Azure 入口網站：**臉部**概觀，標示為 `Endpoint`

|必要項| 名稱 | 資料類型 | 描述 |
|--|------|-----------|-------------|
|是| `Billing` | 字串 | 計費端點 URI<br><br>範例：<br>`Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0` |

<!-- specific to face only -->

## <a name="cloudai-configuration-settings"></a>CloudAI 組態設定

`CloudAI` 區段中的組態設定能提供對您容器而言是唯一的容器特定選項。 下列是 `CloudAI` 區段中支援臉部容器的設定和物件

| 名稱 | 資料類型 | 描述 |
|------|-----------|-------------|
| `Storage` | Object | 臉部容器所使用的儲存體案例。 如需適用於 `Storage` 物件之儲存體案例和相關設定的詳細資訊，請參閱[儲存體案例設定](#storage-scenario-settings) |

### <a name="storage-scenario-settings"></a>儲存體案例設定

依所儲存的內容而定，臉部容器有可能會儲存 Blob、快取、中繼資料，以及佇列資料。 例如，針對大型人員群組的定型索引和結果會被儲存為 Blob 資料。 與下列資料類型互動及儲存它們時，臉部容器會提供兩種不同的儲存體案例：

* 記憶體  
  全部四種資料類型都會被儲存在記憶體中。 它們不會散發，也不具永續性。 如果停止或移除臉部容器，系統便會終結該容器之儲存體中的所有資料。  
  這是適用於臉部容器的預設儲存體案例。
* Azure  
  臉部容器會使用 Azure 儲存體和 Azure Cosmos DB 來將這四種資料類型散發至永續性儲存體上。 Blob 和佇列資料是由 Azure 儲存體負責處理。 中繼資料和快取資料是由 Azure Cosmos DB 來處理。 如果停止或移除臉部容器，該容器之儲存體中的所有資料都會繼續存放在 Azure 儲存體和 Azure Cosmos DB 中。  
  Azure 儲存體案例所使用的資源具有下列額外需求
  * Azure 儲存體資源必須使用 StorageV2 帳戶類型
  * Azure Cosmos DB 資源必須使用 Azure Cosmos DB 的 MongoDB 版 API

儲存體案例和相關的組態設定是由 `CloudAI` 組態區段底下的 `Storage` 物件所管理。 `Storage` 物件提供下列組態設定：

| 名稱 | 資料類型 | 描述 |
|------|-----------|-------------|
| `StorageScenario` | 字串 | 容器所支援的儲存體案例。 可以使用下列值<br/>`Memory`：預設值。 容器會針對單一節點的暫時性使用方式，使用非永續性、非分散式且記憶體內部的儲存體。 如果停止或移除容器，系統便會終結該容器的儲存體。<br/>`Azure`：容器使用 Azure 資源作為儲存體。 如果停止或移除容器，該容器的儲存體仍會保存。|
| `ConnectionStringOfAzureStorage` | 字串 | 容器所使用之 Azure 儲存體資源的連接字串。<br/>此設定只有在已針對 `StorageScenario` 組態設定指定 `Azure` 的情況下才會套用。 |
| `ConnectionStringOfCosmosMongo` | 字串 | 容器所使用之 Azure Cosmos DB 資源的 MongoDB 連接字串。<br/>此設定只有在已針對 `StorageScenario` 組態設定指定 `Azure` 的情況下才會套用。 |

例如，下列命令會指定 Azure 儲存體案例，並針對用來儲存臉部容器資料的 Azure 儲存體和 Cosmos DB 資源提供範例連接字串。

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

儲存體案例會與輸入裝載和輸出裝載分開處理。 您可以針對單一容器指定那些功能的組合。 例如，下列命令會將 Docker 繫結裝載定義至主機電腦上的 `D:\Output` 資料夾作為輸出裝載，然後從臉部容器映像將容器具現化，並以 JSON 格式將記錄檔儲存至輸出裝載。 該命令也會指定 Azure 儲存體案例，並針對用來儲存臉部容器資料的 Azure 儲存體和 Cosmos DB 資源提供範例連接字串。

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

## <a name="eula-setting"></a>Eula 設定

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd 設定

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP Proxy 認證設定

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>記錄設定
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>裝載設定

使用繫結裝載將資料讀取和寫入至容器，及從中讀取和寫入。 您可以在 [docker run](https://docs.docker.com/engine/reference/commandline/run/) 命令中指定 `--mount` 選項，以指定輸入裝載或輸出裝載。

臉部容器不會使用輸入或輸出裝載來儲存訓練或服務資料。 

主機裝載位置的正確語法會隨著主機作業系統而有所不同。 此外，[主機電腦](face-how-to-install-containers.md#the-host-computer)的裝載位置可能會因為 Docker 服務帳戶所使用的權限與主機裝載位置的權限互相衝突，而無法存取。 

|選用| 名稱 | 資料類型 | 描述 |
|-------|------|-----------|-------------|
|不允許| `Input` | 字串 | 臉部容器不會使用此項目。|
|選用| `Output` | 字串 | 輸出裝載的目標。 預設值為 `/output`。 這是記錄的位置。 這包括容器記錄。 <br><br>範例：<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="hierarchical-settings"></a>階層式設定

[!INCLUDE [Container shared configuration hierarchical settings](../../../includes/cognitive-services-containers-configuration-shared-hierarchical-settings.md)]

## <a name="example-docker-run-commands"></a>範例 docker run 命令 

下列範例會使用組態設定來說明如何撰寫和使用 `docker run` 命令。  開始執行後，容器就會持續執行，直到您加以[停止](face-how-to-install-containers.md#stop-the-container)。

* **行接續字元**：以下幾節的 Docker 命令會使用反斜線 `\` 作為行接續字元。 請根據您主機作業系統的需求加以替換或移除。 
* **引數順序**：若非十分熟悉 Docker 容器，請勿變更引數的順序。

請將 {_argument_name_} 取代為您自己的值：

| Placeholder | 值 | 格式或範例 |
|-------------|-------|---|
|{BILLING_KEY} | 臉部資源的端點金鑰。 |xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|{BILLING_ENDPOINT_URI} | 包括區域的計費端點值。|`https://westcentralus.api.cognitive.microsoft.com/face/v1.0`|

> [!IMPORTANT]
> 必須指定 `Eula`、`Billing` 及 `ApiKey` 選項以執行容器，否則容器將不會啟動。  如需詳細資訊，請參閱[帳單](face-how-to-install-containers.md#billing)。
> ApiKey 值是 [Azure 臉部資源金鑰] 頁面中的**金鑰**。 

## <a name="face-container-docker-examples"></a>臉部容器 Docker 範例

下列是臉部容器的 Docker 範例。 

### <a name="basic-example"></a>基本範例 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY} 
  ```

### <a name="logging-example-with-command-line-arguments"></a>使用命令列引數的記錄範例

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} ApiKey={BILLING_KEY} \
  Logging:Console:LogLevel=Information
  ```

### <a name="logging-example-with-environment-variable"></a>使用環境變數的記錄範例

  ```
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={BILLING_ENDPOINT_URI} \
  ApiKey={BILLING_KEY}
  ```

## <a name="next-steps"></a>後續步驟

* 檢閱[如何安裝及執行容器](face-how-to-install-containers.md)

---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: c20f86fe7fdcfc7ecc940923a8c98fa1fbf4cf65
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246775"
---
## <a name="create-a-resource-group"></a>建立資源群組

使用 [az group create](/cli/azure/group) 來建立資源群組。 Azure 資源群組是在其中部署與管理 Azure 資源 (如函式應用程式、資料庫和儲存體帳戶) 的邏輯容器。

下列範例會建立名為 `myResourceGroup` 的資源群組。  
如果您未使用 Cloud Shell，請先使用 `az login` 登入。

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
您通常會在附近的區域中建立資源群組和資源。 若要查看所有支援的 App Service 方案位置，請執行 [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations) 命令。

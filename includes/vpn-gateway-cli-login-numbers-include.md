---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 21dbec52dded5a195cc764741ab9e8d79737c549
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56246763"
---
1. 使用 [az login](/cli/azure/) 命令登入 Azure 訂用帳戶並遵循畫面上的指示。 如需關於登入的詳細資訊，請參閱[開始使用 Azure CLI](/cli/azure/get-started-with-azure-cli)。

   ```azurecli
   az login
   ```
2. 如果您有多個 Azure 訂用帳戶，請列出帳戶的所有訂用帳戶。

   ```azurecli
   az account list --all
   ```
3. 指定您要使用的訂用帳戶。

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```

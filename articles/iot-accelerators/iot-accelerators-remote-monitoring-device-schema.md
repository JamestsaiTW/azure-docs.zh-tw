---
title: 遠端監視解決方案中的裝置結構描述 - Azure | Microsoft Docs
description: 本文說明 JSON 結構描述，它會定義遠端監視解決方案中的模擬裝置。
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2018
ms.topic: conceptual
ms.openlocfilehash: 3c2bca45a7927689992cf44fb70199e246641760
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445417"
---
# <a name="understand-the-device-model-schema"></a>了解裝置機型結構描述

您可以在遠端監視解決方案中使用模擬裝置以測試其行為。 遠端監視解決方案包含用以執行模擬裝置的裝置模擬服務。 當您部署遠端監視解決方案時，會自動佈建模擬裝置集合。 您可以自訂現有的模擬裝置，或建立您自己的裝置。

本文說明裝置機型結構描述，它會指定模擬裝置的功能和行為。 裝置機型會儲存在 JSON 檔案中。

> [!NOTE]
> 此裝置模型結構描述只適用於裝載於裝置模擬服務中的模擬裝置。 如果您想要建立真實裝置，請參閱[將裝置連線到遠端監視解決方案加速器](iot-accelerators-connecting-devices.md)。

下列文章與目前的文章相關：

* [實作裝置機型行為](iot-accelerators-remote-monitoring-device-behavior.md)描述您用來實作模擬裝置行為的 JavaScript 檔案。
* [建立新的模擬裝置](iot-accelerators-remote-monitoring-create-simulated-device.md)會統整並示範如何將新的模擬裝置類型部署到您的解決方案。

在本文中，您將了解：

>[!div class="checklist"]
> * 使用 JSON 檔案以定義模擬裝置機型
> * 指定模擬裝置的屬性
> * 指定模擬裝置傳送的遙測
> * 指定裝置回應的雲端到裝置方法

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>後續步驟

本文說明如何建立您自己的自訂模擬裝置機型。 本文示範如何：

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * 使用 JSON 檔案以定義模擬裝置機型
> * 指定模擬裝置的屬性
> * 指定模擬裝置傳送的遙測
> * 指定裝置回應的雲端到裝置方法

現在您已經了解 JSON 結構描述，建議的下一個步驟是了解如何[實作模擬裝置的行為](iot-accelerators-remote-monitoring-device-behavior.md)。

如需關於遠端監視解決方案的開發人員詳細資訊，請參閱：

* [開發人員參考指南](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [開發人員疑難排解指南](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

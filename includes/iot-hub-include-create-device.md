---
title: 包含檔案
description: 包含檔案
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 11/06/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 4a4ec51430242cc3e3d6a0b801f2b4be7858ab50
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516071"
---
<!-- put the ## header in the file that includes this file -->

在本節中，您會在 IoT 中樞的身分識別登錄中建立裝置身分識別。 裝置無法連線到 IoT 中樞，除非它在身分識別登錄中具有項目。 如需詳細資訊，請參閱 [IoT 中樞開發人員指南](../articles/iot-hub/iot-hub-devguide-identity-registry.md)的＜身分識別登錄＞一節 

1. 在 IoT 中樞導覽功能表中，開啟 [IoT 裝置]，然後按一下 [新增] 在 IoT 中樞註冊新裝置。

    ![在入口網站中建立裝置識別](./media/iot-hub-include-create-device/create-identity-portal.png)

1. 提供新裝置的名稱 (例如 **myDeviceId**)，然後按一下 [儲存]。 此動作會為您的 IoT 中樞建立新的裝置身分識別。

   ![新增裝置](./media/iot-hub-include-create-device/create-a-device.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]


1. 建立裝置之後，請在 [IoT 裝置] 窗格中的清單開啟裝置。 複製**連接字串---主索引鍵**，以供日後使用。

    ![裝置連接字串](./media/iot-hub-include-create-device/device-details.png)

> [!NOTE]
> IoT 中樞身分識別登錄只會儲存裝置身分識別，以啟用對 IoT 中樞的安全存取。 它會儲存裝置識別碼和金鑰來作為安全性認證，以及啟用或停用旗標，讓您用來停用個別裝置的存取。 如果您的應用程式需要儲存其他裝置特定的中繼資料，它應該使用應用程式專用的存放區。 如需詳細資訊，請參閱 [IoT 中樞開發人員指南](../articles/iot-hub/iot-hub-devguide-identity-registry.md)。

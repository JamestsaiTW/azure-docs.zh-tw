---
title: Azure 快速入門 - 使用 Azure 入口網站建立事件中樞 | Microsoft Docs
description: 在本快速入門中，您將了解如何使用 Azure 入口網站建立 Azure 事件中樞，然後使用 .NET Standard SDK 傳送和接收事件。
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/23/2019
ms.author: shvija
ms.openlocfilehash: 073d1b54b1c6da2b1178f482d17b4de7abcc8a51
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888918"
---
# <a name="quickstart-create-an-event-hub-using-azure-portal"></a>快速入門：使用 Azure 入口網站建立事件中樞
Azure 事件中樞是巨量資料串流平台和事件擷取服務，每秒可接收和處理數百萬個事件。 事件中樞可以處理及儲存分散式軟體和裝置所產生的事件、資料或遙測。 傳送至事件中樞的資料可以透過任何即時分析提供者或批次/儲存體配接器來轉換和儲存。 如需事件中樞的詳細概觀，請參閱[事件中樞概觀](event-hubs-about.md)和[事件中樞功能](event-hubs-features.md)。

在此快速入門中，您將使用 [Azure 入口網站](https://portal.azure.com)建立事件中樞。

## <a name="prerequisites"></a>必要條件

若要完成本快速入門，請確定您具備︰

- Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請在開始前[建立免費帳戶](https://azure.microsoft.com/free/)。
- [Visual Studio 2017 Update 3 (版本 15.3, 26730.01)](https://www.visualstudio.com/vs) 或更新版本。
- [.NET Standard SDK](https://www.microsoft.com/net/download/windows)，2.0 版或更新版本。

## <a name="create-a-resource-group"></a>建立資源群組

資源群組是 Azure 資源的邏輯集合。 所有資源都會在資源群組中部署與管理。 若要建立資源群組︰

1. 登入 [Azure 入口網站](https://portal.azure.com)。
2. 在左側導覽中，按一下 [資源群組]。 然後按一下 [ **新增**]。

   ![資源群組 - 新增按鈕](./media/event-hubs-quickstart-portal/resource-groups1.png)

2. 在 [訂用帳戶] 中，選取要在其中建立資源群組的 Azure 訂用帳戶名稱。
3. 輸入**資源群組的唯一名稱**。 系統會立即檢查名稱是否可用於目前選取的 Azure 訂用帳戶。
4. 選取資源群組的**區域**。
5. 選取 [檢閱 + 建立]。

   ![資源群組 - 建立](./media/event-hubs-quickstart-portal/resource-groups2.png)
6. 在 [檢閱 + 建立] 頁面上，選取 [建立]。 

## <a name="create-an-event-hubs-namespace"></a>建立事件中樞命名空間

事件中樞命名空間提供唯一的範圍容器 (依其完整網域名稱來參考)，您可以在其中建立一或多個事件中樞。 若要使用入口網站在您的資源群組中建立命名空間，請執行下列動作：

1. 在 Azure 入口網站中，按一下畫面左上方的 [建立資源]。
2. 選取左側功能表中的 [所有服務] ，然後選取 [分析] 類別中 [事件中樞] 旁邊的 **星號 (`*`)**。 確認 [事件中樞] 已新增至左側導覽功能表中的 [我的最愛]。 
    
   ![搜尋事件中樞](./media/event-hubs-quickstart-portal/select-event-hubs-menu.png)
3. 選取左側導覽功能表中 [我的最愛] 下方的 [事件中樞]，然後選取工具列上的 [新增]。

   ![新增工具列按鈕](./media/event-hubs-quickstart-portal/event-hubs-add-toolbar.png)
4. 在 [建立命名空間] 頁面上，採取下列步驟：
    1. 輸入命名空間的名稱。 系統會立即檢查此名稱是否可用。
    2. 選擇定價層 (基本或標準)。
    3. 選取您要在其中建立命名空間的**訂用帳戶**。
    4. 選取命名空間的**位置**。
    5. 選取 [建立] 。 您可能必須等候幾分鐘，讓系統完整佈建資源。

       ![建立事件中樞命名空間](./media/event-hubs-quickstart-portal/create-event-hub1.png)
5. 重新整理 [事件中樞] 頁面，以查看事件中樞命名空間。 您可以檢查警示中事件中樞建立的狀態。 

    ![建立事件中樞命名空間](./media/event-hubs-quickstart-portal/event-hubs-refresh.png)
6. 選取命名空間。 您會在入口網站中看到**事件中樞命名空間**的首頁。 

   ![命名空間的首頁](./media/event-hubs-quickstart-portal/namespace-home-page.png)
    
## <a name="create-an-event-hub"></a>建立事件中樞

若要在命名空間內建立事件中樞，請執行下列動作：

1. 在 [事件中樞命名空間] 頁面上，選取左功能表中的 [事件中樞]。
1. 在視窗頂端，按一下 [+ 事件中樞]。
   
    ![新增事件中樞 - 按鈕](./media/event-hubs-quickstart-portal/create-event-hub4.png)
1. 輸入您的事件中樞名稱，然後按一下 [建立]。
   
    ![建立事件中樞](./media/event-hubs-quickstart-portal/create-event-hub5.png)
4. 您可以檢查警示中事件中樞建立的狀態。 建立事件中樞之後，您會在事件中樞清單中看到它 (如下圖所示)：

    ![已建立事件中樞](./media/event-hubs-quickstart-portal/event-hub-created.png)

恭喜！ 您已使用入口網站建立事件中樞命名空間和該命名空間內的事件中樞。 

## <a name="next-steps"></a>後續步驟

在本文中，您已建立資源群組、事件中樞命名空間和事件中樞。 如需將事件傳送至事件中樞或從事件樞接收事件的逐步指示，請參閱下列教學課程：  

- **將事件傳送至事件中樞**：[.NET Core](event-hubs-dotnet-standard-getstarted-send.md)、[.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)、[Java](event-hubs-java-get-started-send.md)、[Python](event-hubs-python-get-started-send.md)、[Node.js](event-hubs-node-get-started-send.md)、[Go](event-hubs-go-get-started-send.md)、[C](event-hubs-c-getstarted-send.md)
- **從事件中樞接收事件**：[.NET Core](event-hubs-dotnet-standard-getstarted-receive-eph.md)、[.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md)、[Java](event-hubs-java-get-started-receive-eph.md)、[Python](event-hubs-python-get-started-receive.md)、[Node.js](event-hubs-node-get-started-receive.md)、[Go](event-hubs-go-get-started-receive-eph.md)、[Apache Storm](event-hubs-storm-getstarted-receive.md)


[Azure portal]: https://portal.azure.com/
[3]: ./media/event-hubs-quickstart-portal/sender1.png
[4]: ./media/event-hubs-quickstart-portal/receiver1.png

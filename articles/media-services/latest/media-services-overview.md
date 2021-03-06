---
title: Azure 媒體服務 v3 概觀 | Microsoft Docs
description: 本文提供媒體服務的高階概觀，並提供詳細資訊的文章連結。
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: Azure 媒體服務, 串流, 廣播, 即時, 離線
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 02/26/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: de1bd8599682baae860737401d02c7c1dec9e223
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960644"
---
# <a name="what-is-azure-media-services-v3"></a>什麼是 Azure 媒體服務 v3？

Azure 媒體服務是雲端式平台，可讓您建置的解決方案擁有廣播品質的視訊串流、增強協助工具和散佈、分析內容等等。 不論您是應用程式開發人員、話務中心、政府機構、娛樂公司，媒體服務都能協助您建立應用程式，為時下最熱門的行動裝置和瀏覽器上的廣大觀眾，傳遞優異品質的媒體體驗。 

## <a name="what-can-i-do-with-media-services"></a>我可以如何運用媒體服務？

媒體服務可讓您在雲端建置各種媒體工作流程，以下範例是使用媒體服務可以完成的項目。  

* 以各種格式傳遞影片，使它們能在多種瀏覽器和裝置上播放。 不論是隨選播放或是即時串流傳遞至各種用戶端 (行動裝置、電視、電腦等)，視訊和音訊內容都必須適當地編碼和封裝。 若要了解如何傳遞和串流這類內容，請參閱[快速入門：編碼和串流檔案](stream-files-dotnet-quickstart.md)。
* 將即時體育賽事串流至廣大的線上觀眾，例如足球、棒球、大專院校和高中體育競賽等等。 
* 廣播公眾會議和活動，例如市政府、市議會會議，及立法機構。
* 分析錄製的視訊或音訊內容。 例如，為了達到更高的客戶滿意度，組織可以擷取語音轉換文字，並建置搜尋索引和儀表板。 接著便可以從一般客訴、客訴來源及其他相關資料中獲取情報。
* 建立訂閱影片服務，並在客戶需要限制對著作權所有作品的存取及使用時，串流受 DRM 保護的內容。
* 傳遞離線內容，以在飛機、火車和汽車上播放。 當客戶預期會從網路中斷連線時，他們可能需要將內容下載到手機或平板電腦上播放。
* 使用 Azure 媒體服務和 [Azure 認知服務 API](https://docs.microsoft.com/azure/#pivot=products&panel=ai) 的語音文字轉換、翻譯成多語言等功能，實作教育電子化學習影片平台。 
* 搭配使用 Azure 媒體服務與 [Azure 認知服務 API](https://docs.microsoft.com/azure/#pivot=products&panel=ai) 在影片加入標題和字幕，以服務更廣大的觀眾 (例如聽障人士或想要以其他語言閱讀的人)。
* 啟用 Azure CDN 以達成大規模調整，進而妥善處理瞬間高負載 (例如，在產品上市活動開始時)。 

## <a name="v3-capabilities"></a>v3 功能

v3 是會統一的 API 表面為基礎，併公開建置於 Azure Resource Manager 上的管理和操作功能。 

此版本提供以下功能：  

* **轉換**可協助您定義媒體處理或分析工作的簡單工作流程。 轉換是處理您的視訊和音訊檔案的配方。 然後藉由將工作提交至轉換，您可以重複地套用它來處理內容庫中的所有檔案。
* **工作**可處理 (編碼或分析) 您的視訊。 您可以使用 HTTPS URL、SAS URL 或 Azure Blob 儲存體中的檔案路徑，在作業上指定輸入內容。 目前，AMS v3 不支援透過 HTTPS URL 的區塊傳送編碼。
* **通知**可監視工作進度或狀態，或直播頻道開始/停止及錯誤事件。 通知會與 Azure Event Grid 通知系統整合。 您輕鬆就能在 Azure 媒體服務中的數個資源上訂閱活動。 
* **Azure Resource Management** 範本可用於建立及部署轉換、串流端點、頻道等等。
* **角色型存取控制**可以設定在資源層級，讓您能鎖定對特定資源 (如轉換、頻道等) 的存取。
* **用戶端 SDK** 有多種語言：.NET、.NET Core、Python、Go、Java 和 Node.js。

## <a name="naming-conventions"></a>命名慣例

Azure 媒體服務 v3 資源名稱 (例如資產、作業、轉換) 會受到 Azure Resource Manager 命名規則的約束。 根據 Azure Resource Manager，資源名稱永遠是唯一的。 因此，您可以對資源名稱使用任何唯一識別碼字串 (例如，GUID)。 

媒體服務資源名稱不可包含：'<'、'>'、'%'、'&'、':'、'&#92;'、'?'、'/'、'*'、'+'、'.'、單引號字元或任何控制字元。 允許所有其他字元。 資源名稱的長度上限是 260 個字元。 

如需有關 Azure Resource Manager 命名的詳細資訊，請參閱：[命名需求](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource)和[命名慣例](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)。

## <a name="v3-api-design-principles"></a>v3 API 設計原則

v3 API 的金鑰設計原則之一，是讓 API 更為安全。 v3 API 不會從**取得**或**列出**作業傳回秘密或認證。 回應中的金鑰一律為 Null、空白或處理過的。 您必須呼叫個別的動作方法，以取得秘密或認證。 在有些 API 會擷取/顯示秘密，而有些 API 不會的情況下，個別的動作可讓您設定不同的 RBAC 安全性權限。 如需如何使用 RBAC 管理存取權的相關資訊，請參閱[使用 RBAC 管理存取權](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)。

其範例包括 

* 在取得 StreamingLocator 時不傳回 ContentKey 值、 
* 在取得 ContentKeyPolicy 時不傳回限制金鑰、 
* 不傳回作業的 HTTP 輸入 URL 的查詢字串部分 (以移除簽章)。

請參閱[取得內容金鑰原則 - .NET](get-content-key-policy-dotnet-howto.md) 範例。

## <a name="how-can-i-get-started-with-v3"></a>如何開始使用 v3？

如需如何使用各種工具和 SDK，利用媒體服務 v3 API 開始進行開發的相關資訊，請參閱[開始開發](developers-guide.md)。

## <a name="v3-content-map"></a>v3 內容對應

媒體服務 v3 內容會根據下列結構 (也會反映在目錄中) 來組織：

|章節| 說明|
|---|---|
| 概觀 | 說明媒體服務的功能以及您可以使用該服務進行哪些工作。|
| 快速入門 | 顯示第 1 天的基本指示，讓新客戶快速試用媒體服務。|
| 教學課程 | 顯示某些常用媒體服務工作的案例式程序。|
| 範例 | 程式碼範例的連結。 |
| 概念 | 包含[媒體服務 v3 概念和功能](concepts-overview.md)的深入說明。 請先檢閱這些主題所述的基本概念再開始進行開發。<br/><br/>* 雲端上傳和儲存體<br/>* 編碼<br/>* 媒體分析<br/>* 封裝、傳遞、保護<br/>* 即時資料流<br/>* 監視<br/>* 播放機用戶端<br/><br/>不勝枚舉。 |
| 使用說明指南 | 示範如何完成工作。|

## <a name="next-steps"></a>後續步驟

若要了解開始撰寫程式碼並串流視訊檔案是多麼容易，請查看[串流檔案](stream-files-dotnet-quickstart.md)。 


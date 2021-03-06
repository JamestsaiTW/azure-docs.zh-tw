---
title: Microsoft Dynamics CRM 和 Azure Application Insights | Microsoft Docs
description: 使用 Application Insights 取得 Microsoft Dynamics CRM Online 遙測。 設定、取得資料、視覺化與匯出的逐步解說。
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/16/2018
ms.reviewer: mazhar
ms.author: mbullwin
ms.openlocfilehash: 6119f1116d255f7cd2a2bfc20e86eeca9e5dfe82
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121153"
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>逐步解說：使用 Application Insights 啟用Microsoft Dynamics CRM Online 遙測
本文說明如何使用 [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 從 [Microsoft Dynamics CRM Online](https://www.dynamics.com/) 取得遙測資料。 我們會逐步解說將 Application Insights 指令碼加入至您的應用程式、擷取資料和資料視覺化的完整程序。

> [!NOTE]
> [瀏覽範例解決方案](https://dynamicsandappinsights.codeplex.com/)。
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>將 Application Insights 加入至新的或現有的 CRM Online 執行個體
若要監視您的應用程式，請將 Application Insights SDK 加入應用程式。 所有 SDK 均會將遙測資料傳送至 [Application Insights 入口網站](https://portal.azure.com)，您可以在該入口網站中使用我們功能強大的分析與診斷工具，並將資料匯出至儲存體。

### <a name="create-an-application-insights-resource-in-azure"></a>在 Azure 中建立 Application Insights 資源
1. 取得 [Microsoft Azure 帳戶](https://azure.com/pricing)。 
2. 登入 [Azure 入口網站](https://portal.azure.com) 並加入新的 Application Insights 資源。 這是處理與顯示您資料的位置。

    ![按一下 [+]、[開發人員服務]、[Application Insights]。](./media/sample-mscrm/01.png)

    選擇 ASP.NET 做為應用程式類型。
3. 依照指示來[取得適用於您應用程式的 JavaScript SDK 指令碼](../../azure-monitor/app/javascript.md#set-up-application-insights-for-your-web-page)、複製 JavaScript 程式碼片段，並請務必以您 Application Insights 資源的正確值取代「檢測金鑰」。

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>在 Microsoft Dynamics CRM 中建立 JavaScript Web 資源
1. 開啟您的 CRM Online 執行個體並使用系統管理員權限登入。
2. 開啟 [Microsoft Dynamics CRM 設定]、[自訂]、[自訂系統]

    ![Microsoft Dynamics CRM 設定](./media/sample-mscrm/00001.png)

    ![[設定] > [自訂]](./media/sample-mscrm/00002.png)

1. 建立 JavaScript 資源。

    ![新增 Web 資源對話方塊](./media/sample-mscrm/07.png)

    命名資源、選取 **指令碼 (JScript)** 並開啟文字編輯器。

    ![開啟文字編輯器](./media/sample-mscrm/00004.png)
2. 從您先前設定「檢測金鑰」的 Application Insights JavaScript SDK 中複製程式碼。 在複製時請務必略過 script 標記。 請參考下方的螢幕擷取畫面：

    ![設定您的檢測金鑰](./media/sample-mscrm/000005.png)

    程式碼包含用來識別您 Application insights 資源的檢測金鑰。
3. 儲存並發佈。

    ![儲存並發佈](./media/sample-mscrm/00006.png)

### <a name="instrument-forms"></a>檢測表單
1. 在 Microsoft CRM Online 中，開啟 [帳戶] 表單

    ![帳戶表單](./media/sample-mscrm/00007.png)
2. 開啟 [表單屬性]

    ![表單屬性](./media/sample-mscrm/00008.png)
3. 加入您建立的 JavaScript Web 資源

    ![[新增] 功能表](./media/sample-mscrm/13.png)

4. 儲存並發佈您的表單自訂內容。

## <a name="metrics-captured"></a>擷取的度量
您現在已設定遙測擷取表單。 只要使用，資料就會傳送至 Application Insights 資源。

以下是您會看到的資料範例。

#### <a name="application-health"></a>應用程式健全狀況
![範例頁面載入時間](./media/sample-mscrm/15.png)

![範例頁面檢視圖表](./media/sample-mscrm/16.png)

瀏覽器例外狀況：

![瀏覽器例外狀況圖表](./media/sample-mscrm/17.png)

按一下圖表以取得詳細資料：

![例外狀況清單](./media/sample-mscrm/18.png)

#### <a name="usage"></a>使用量
![使用者數、工作階段數及頁面檢視數](./media/sample-mscrm/19.png)

![工作階段圖表](./media/sample-mscrm/20.png)

![瀏覽器版本](./media/sample-mscrm/21.png)

#### <a name="browsers"></a>瀏覽器
![頁面載入時間明細](./media/sample-mscrm/22.png)

![依瀏覽器版本區分的工作階段計數](./media/sample-mscrm/23.png)

#### <a name="geolocation"></a>地理位置
![依國家/地區區分的工作階段計數](./media/sample-mscrm/24.png)

![依國家/地區區分的工作階段數和使用者數](./media/sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>深入了解頁面檢視要求
![頁面檢視摘要](./media/sample-mscrm/26.png)

![依頁面檢視事件進行搜尋](./media/sample-mscrm/27.png)

![相似頁面檢視數](./media/sample-mscrm/28.png)

![頁面檢視屬性](./media/sample-mscrm/29.png)

![每個工作階段的頁面數](./media/sample-mscrm/30.png)

## <a name="sample-code"></a>範例程式碼
[取得範例程式碼](https://dynamicsandappinsights.codeplex.com/)。

## <a name="power-bi"></a>Power BI
如果您 [將資料匯出到 Microsoft Power BI](../../azure-monitor/app/export-power-bi.md )，則可以進行更深入的分析。

## <a name="sample-microsoft-dynamics-crm-solution"></a>Microsoft Dynamics CRM 解決方案範例
[以下是在 Microsoft Dynamics CRM 中實作的解決方案範例](https://dynamicsandappinsights.codeplex.com/)。

## <a name="learn-more"></a>深入了解
* [什麼是 Application Insights？](../../azure-monitor/app/app-insights-overview.md)
* [適用於網頁的 Application Insights](../../azure-monitor/app/javascript.md)
* [更多範例和逐步解說](../../azure-monitor/app/app-insights-overview.md)

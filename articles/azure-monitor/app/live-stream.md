---
title: Azure Application Insights 中具有自訂計量和診斷功能的即時計量資料流 | Microsoft Docs
description: 透過自訂計量即時監視您的 Web 應用程式，並透過失敗、追蹤和事件的即時摘要診斷問題。
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/28/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: b741528b2770314be7e851f38817611d6908352b
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814940"
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>即時計量資料流：以 1 秒的延遲進行監視與診斷

使用 [Application Insights](../../azure-monitor/app/app-insights-overview.md) 中的即時計量資料流，探查您即時生產環境 Web 應用程式中的活動訊號。 選取並篩選要即時監看的計量和效能計數器，而不會對您的服務造成任何干擾。 檢查失敗要求和例外狀況範例中的堆疊追蹤。 即時計量資料流可搭配[分析工具](../../azure-monitor/app/profiler.md)、[快照集偵錯工具](../../azure-monitor/app/snapshot-debugger.md)和[效能測試](../../azure-monitor/app/monitor-web-app-availability.md#performance-tests)，為您的即時網站提供強大且非侵入性的診斷工具。

您可以使用即時計量資料流：

* 藉由監看效能和失敗計數，在發行修正程式時進行驗證。
* 監看測試負載的影響，並即時診斷問題。 
* 藉由選取並篩選您想要監看的計量，專注於特定測試工作階段或篩選出已知問題。
* 在發生例外狀況時取得其追蹤。
* 試驗篩選，以尋找最相關的 KPI。
* 即時監看任何 Windows 效能計數器。
* 輕鬆識別有問題的伺服器，並將所有 KPI/即時摘要篩選到只有該伺服器。

[![即時計量資料流影片](./media/live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

目前針對 ASP.NET、ASP.NET Core、Azure Functions 及 Java 應用程式可支援「即時計量」。

## <a name="get-started"></a>開始使用

1. 如果您尚未在 Web 應用程式中[安裝 Application Insights](../../azure-monitor/azure-monitor-app-hub.md)，請立即安裝。
2. 除了標準 Application Insights 套件之外，還需要 [Microsoft.ApplicationInsights.PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/)，才能啟用「即時計量」串流。
3. **更新至最新版本**的 Application Insights 套件。 在 Visual Studio 中，以滑鼠右鍵按一下專案，然後選擇 [管理 Nuget 套件]。 開啟 [更新] 索引標籤，然後選取所有 Microsoft.ApplicationInsights.* 套件。

    重新部署您的應用程式。

3. 在 [Azure 入口網站](https://portal.azure.com)中，開啟您應用程式的 Application Insights 資源，然後開啟 [即時資料流]。

4. 如果您可能在篩選中使用客戶名稱等敏感性資料，請[保護控制通道](#secure-the-control-channel)。

### <a name="no-data-check-your-server-firewall"></a>沒有資料？ 請檢查您的伺服器防火牆

檢查是否已開啟您伺服器防火牆中[即時計量資料流的連出連接埠](../../azure-monitor/app/ip-addresses.md#outgoing-ports)。 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>即時計量資料流與計量瀏覽器和分析有何不同？

| |即時資料流 | 計量瀏覽器和分析 |
|---|---|---|
|延遲|在一秒內顯示資料|在幾分鐘後進行彙總|
|沒有保留|資料在圖表上就會保存，之後便會捨棄該資料|[資料會保留 90 天](../../azure-monitor/app/data-retention-privacy.md#how-long-is-the-data-kept)|
|隨選|當您開啟即時計量時會串流處理資料|每當安裝並啟用 SDK 時都會傳送資料|
|免費|即時資料流資料免費|依[價格](../../azure-monitor/app/pricing.md)付費
|取樣|傳輸所有選取的計量和計數器。 取樣失敗和堆疊追蹤。 不會套用 TelemetryProcessors。|可能[取樣](../../azure-monitor/app/api-filtering-sampling.md)事件|
|控制通道|篩選控制項訊號會傳送至 SDK。 建議您保護這個通道。|對入口網站的單向通訊|


## <a name="select-and-filter-your-metrics"></a>選取並篩選您的計量

(適用於 ASP.NET、ASP.NET Core 及 Azure Functions (v2))。

您可以從入口網站將任何篩選條件套用在任何 Application Insights 遙測上，以便即時監視自訂 KPI。 按一下您將滑鼠移過任何圖表時所顯示的篩選條件控制項。 下列圖表是使用 URL 和 Duration 屬性的篩選條件來繪製自訂要求計數 KPI。 利用可顯示符合您在任一時間點所指定準則之遙測即時摘要的「串流預覽」區段來驗證您的篩選條件。 

![自訂要求 KPI](./media/live-stream/live-stream-filteredMetric.png)

您可以監視與計數不同的值。 選項取決於串流的類型，可能是任何的 Application Insights 遙測︰要求、相依性、例外狀況、追蹤、事件或計量。 它可以是您自己的[自訂測量](../../azure-monitor/app/api-custom-events-metrics.md#properties)：

![值選項](./media/live-stream/live-stream-valueoptions.png)

除了 Application Insights 遙測之外，您也可以監視任何 Windows 效能計數器，方法是從串流選項中選取，並提供效能計數器的名稱。

即時計量會在兩個地方進行彙總︰在每一部伺服器上本機進行，以及在所有伺服器上進行。 您可以選取個別下拉式清單中的其他選項，在任一位置變更預設。

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>遙測範例：自訂即時診斷事件
依預設，事件的即時摘要會顯示失敗要求和相依性呼叫、例外狀況、事件以及追蹤的範例。 按一下篩選圖示可查看任一時間點套用的準則。 

![預設的即時摘要](./media/live-stream/live-stream-eventsdefault.png)

如同計量，您可以將任意準則指定為任何的 Application Insights 遙測類型。 在此範例中，我們要選取特定的要求失敗、追蹤及事件。 我們還會選取所有的例外狀況和相依性失敗。

![自訂即時摘要](./media/live-stream/live-stream-events.png)

注意：目前針對以例外狀況訊息為基礎的準則，請使用最外部的例外狀況訊息。 在上述範例中，若要使用內部例外狀況訊息 (後接 "<--" 分隔符號)「用戶端中斷連線。」將良性的例外狀況篩選掉， 請使用不包含「讀取要求內容時發生錯誤」準則的訊息。

按一下即時摘要中的項目，以查看它的詳細資料。 您可以按一下 [暫停] 或只向下捲動，或是按一下項目來將摘要暫停。 在您捲動回到頂端後，或是按一下暫停時所收集到的項目計數器，即時摘要就會繼續進行。

![取樣的即時失敗](./media/live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>依伺服器執行個體篩選

如果您想要監視特定伺服器角色執行個體，可以依伺服器篩選。

![取樣的即時失敗](./media/live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>SDK 需求
[Application Insights SDK for Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 的 2.4.0-beta2 版或更新版本提供「自訂即時計量串流」。 請記得選取 NuGet 套件管理員的 [包括預先發行] 選項。

## <a name="secure-the-control-channel"></a>保護控制通道
您指定的自訂篩選條件準則會傳回給 Application Insights SDK 中的即時計量元件。 篩選條件可能會包含機密資訊，例如 customerIDs。 除了檢測金鑰之外，您還可以利用祕密 API 金鑰來保護頻道安全。
### <a name="create-an-api-key"></a>建立 API 金鑰

![建立 API 金鑰](./media/live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>將 API 金鑰新增至設定

### <a name="classic-aspnet"></a>傳統 ASP.NET

在 applicationinsights.config 檔案中，將 AuthenticationApiKey 新增至 QuickPulseTelemetryModule：
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add>

```
或在程式碼中，在 QuickPulseTelemetryModule 上設定它：

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
using Microsoft.ApplicationInsights.Extensibility;

             TelemetryConfiguration configuration = new TelemetryConfiguration();
            configuration.InstrumentationKey = "YOUR-IKEY-HERE";

            QuickPulseTelemetryProcessor processor = null;

            configuration.TelemetryProcessorChainBuilder
                .Use((next) =>
                {
                    processor = new QuickPulseTelemetryProcessor(next);
                    return processor;
                })
                        .Build();

            var QuickPulse = new QuickPulseTelemetryModule()
            {

                AuthenticationApiKey = "YOUR-API-KEY"
            };
            QuickPulse.Initialize(configuration);
            QuickPulse.RegisterTelemetryProcessor(processor);
            foreach (var telemetryProcessor in configuration.TelemetryProcessors)
                {
                if (telemetryProcessor is ITelemetryModule telemetryModule)
                    {
                    telemetryModule.Initialize(configuration);
                    }
                }

```

### <a name="azure-function-apps"></a>Azure 函式應用程式

就「Azure 函數應用程式 (v2)」而言，可以藉由環境變數來達到使用 API 金鑰保護通道的目的。 

請從 Application Insights 資源內部建立 API 金鑰，然後前往您「函數應用程式」的 [應用程式設定]。 選取 [新增設訂]，然後輸入名稱 `APPINSIGHTS_QUICKPULSEAUTHAPIKEY` 及與您 API 金鑰對應的值。

### <a name="aspnet-core-requires-application-insights-aspnet-core-sdk-230-beta-or-greater"></a>ASP.NET Core (需要 Application Insights ASP.NET Core SDK 2.3.0-beta 或更新版本)

修改您的 startup.cs 檔案，如下所示：

第一次新增

``` C#
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
```

然後在 ConfigureServices 方法內新增：

``` C#
services.ConfigureTelemetryModule<QuickPulseTelemetryModule> ((module, o) => module.AuthenticationApiKey = "YOUR-API-KEY-HERE");
```

不過，如果您認得並信任所有連線的伺服器，則無需透驗證的頻道就可以嘗試自訂篩選器。 這個選項有六個月可供使用。 一旦每個新的工作階段或是新的伺服器上線後，就需要此覆寫。

![即時計量驗證選項](./media/live-stream/live-stream-auth.png)

>[!NOTE]
>我們強烈建議您在篩選條件準則中輸入潛在的機密資訊 (例如 CustomerID) 之前，先設定驗證的頻道。
>

## <a name="generating-a-performance-test-load"></a>產生效能測試負載

如果您想要監看負載增加的影響，請使用 [效能測試] 刀鋒視窗。 它會模擬來自許多同時使用者的要求。 它可以執行單一 URL 的「手動測試」(Ping 測試)，或執行您上傳的[多重步驟 Web 效能測試](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) (上傳方式與可用性測試相同)。

> [!TIP]
> 建立效能測試之後，在不同的視窗中開啟測試和 [即時資料流] 刀鋒視窗。 您可以查看已排入佇列之效能測試的開始時間，並同時監看即時資料流。
>


## <a name="troubleshooting"></a>疑難排解

沒有資料？ 如果您的應用程式是在受保護的網路中︰即時計量資料流會使用與其他 Application Insights 遙測不同的 IP 位址。 請確定[這些 IP 位址](../../azure-monitor/app/ip-addresses.md)在您的防火牆中為開啟狀態。



## <a name="next-steps"></a>後續步驟
* [使用 Application Insights 監視使用情況](../../azure-monitor/app/usage-overview.md)
* [使用診斷搜尋](../../azure-monitor/app/diagnostic-search.md)
* [分析工具](../../azure-monitor/app/profiler.md)
* [快照集偵錯工具](../../azure-monitor/app/snapshot-debugger.md)

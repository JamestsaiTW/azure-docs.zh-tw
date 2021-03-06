---
title: 適用於容器的 Azure 監視器常見問題集 | Microsoft Docs
description: 適用於容器的 Azure 監視器是一種解決方案，可監視 Azure 中 AKS 叢集與容器執行個體的健康狀情況。 本文將回答常見問題。
services: azure-monitor
author: mgoedtel
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: magoedte
ms.openlocfilehash: 250bf6ede3a54b26b6e9c68850f8e7260020002e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960493"
---
# <a name="azure-monitor-for-containers-frequently-asked-questions"></a>適用於容器的 Azure 監視器常見問題集
此 Microsoft 常見問題集是適用於容器的 Azure 監視器常見問題清單。 若您有任何關於解決方案的其他問題，請前往[討論論壇](https://feedback.azure.com/forums/34192--general-feedback)並張貼您的問題。 當問到常見問題時，我們會將其新增至此文章，以便其他人可以快速輕鬆地找到此問題。

## <a name="i-am-unable-to-see-any-data-in-the-log-analytics-workspace-at-a-certain-time-everyday-how-do-i-resolve-this"></a>我無法查看日常的一段時間中的 Log Analytics 工作區中的任何資料。 如何解決這個問題？ 

預設值 500 MB 的限制或控制的每日收集的資料量指定每日上限，您可能已達到。 當一天達到上限時，資料收集會停止，而且只在下一步 繼續。 若要檢閱您的資料使用量，並更新至不同的定價層，根據您預期的使用模式，請參閱[記錄資料使用量和成本](../platform/manage-cost-storage.md)。 

## <a name="what-are-the-states-of-containers-specified-in-the-containerinventory-table"></a>ContainerInventory 資料表中指定的容器狀態有哪些？
ContainerInventory 資料表包含已停止和執行中容器的相關資訊。 此資料表由代理程式內的工作流程填入，該工作流程會查詢所有 (執行中和已停用) 容器的 Docker，並將該資料轉送到 Log Analytics 工作區。
 
## <a name="how-do-i-resolve-errors-related-to-missing-subscription-registration-for-microsoftoperationsmanagement"></a>如何解決與**遺漏 Microsoft.OperationsManagement 的訂用帳戶註冊**相關的問題？
若要解決此錯誤，請在定義工作區所在訂用帳戶中註冊資源提供者 **Microsoft.OperationsManagement**。 您可以在[這裡](../../azure-resource-manager/resource-manager-register-provider-errors.md)找到作法的相關文件。

## <a name="does-azure-monitor-for-containers-include-support-for-rbac-enabled-aks-clusters"></a>適用於容器的 Azure 監視器包含支援 RBAC 啟用的 AKS 叢集嗎？
容器監視解決方案不支援 RBAC，但它使用適用於容器的 Azure 監視器支援。 解決方案詳細資料頁面不會在顯示這些叢集資料的刀鋒視窗中顯示正確的資訊。

## <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>我如何透過 Helm 在 kube 系統命名空間中啟用容器的記錄檔收集？
kube 系統命名空間中容器的記錄檔收集預設為停用。 透過設定 omsagent 上的環境變數，可啟用記錄檔收集。 如需詳細資訊，請參閱 <<c0> [ 適用於容器的 Azure 監視器](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers)GitHub 頁面。 

## <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>如何將 omsagent 更新為最新的發行版本？
若要了解如何升級代理程式，請參閱[代理程式管理](container-insights-manage-agent.md)。

## <a name="how-do-i-enable-multi-line-logging"></a>如何啟用多行記錄？
適用於容器的 Azure 監視器目前不支援多行記錄，但有可用的因應措施。 您可以設定使用 JSON 格式寫入所有的服務，之後 Docker/Moby會將所有服務寫成一行。

例如，您可以將記錄檔包裝在 JSON 物件，如以下範例中範例 node.js 應用程式所示：

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

這項資料看起來像下列範例在 Azure 監視器中的記錄檔中，當您查詢：

```
LogEntry : ({“Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

如需此問題的詳細探討，請檢閱下列 [github 連結](https://github.com/moby/moby/issues/22920) \(英文\)。

## <a name="how-do-i-resolve-azure-active-directory-errors-when-i-enable-live-logs"></a>當我啟用即時記錄檔時如何解決 Azure Active Directory 錯誤？ 
您可能會看到下列錯誤：**回覆要求中指定的 url 不符合為應用程式設定的回覆 url: '< 應用程式識別碼\>'**。 來解決此問題的解決方案可在發行項[如何檢視容器記錄檔使用即時 Azure 監視器容器](container-insights-live-logs.md#configure-aks-with-azure-active-directory)。 

## <a name="next-steps"></a>後續步驟
若要開始監視您的 AKS 叢集，請檢閱[如何將適用於容器的 Azure 監視器上線](container-insights-onboard.md)，以了解啟用監視的需求和可用方法。 
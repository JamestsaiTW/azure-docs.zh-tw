---
title: 使用 PowerShell 建立和設定 Log Analytics 工作區 | Microsoft Docs
description: Log Analytics 會使用來自內部部署或雲端基礎結構中之伺服器的資料。 您可以在 Azure 診斷產生電腦資料時，從 Azure 儲存體加以收集。
services: log-analytics
author: richrundmsft
ms.service: log-analytics
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: richrund
ms.openlocfilehash: 1cfb4850c35806d702f76f4006ad46ad43d6bcf0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "58076406"
---
# <a name="manage-log-analytics-using-powershell"></a>使用 PowerShell 管理 Log Analytics

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

您可以從命令列或在指令碼中，使用 [Log Analytics PowerShell Cmdlet](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) 在 Log Analytics 中執行各種功能。  您可以使用 PowerShell 執行的工作範例包括︰

* 建立工作區
* 新增或移除方案
* 匯入和匯出已儲存的搜尋
* 建立電腦群組
* 從已安裝 Windows 代理程式的電腦啟用 IIS 記錄檔收集功能
* 從 Linux 和 Windows 電腦收集效能計數器
* 在 Linux 電腦上收集 syslog 事件
* 從 Windows 事件記錄檔收集事件
* 收集自訂事件記錄檔
* 將 Log Analytics 代理程式加入至 Azure 虛擬機器
* 設定 Log Analytics 將 Azure 診斷所收集的資料編製索引

本文提供兩個程式碼範例，示範您可以從 PowerShell 執行的一些功能。  關於其他功能，您可以參考 [Log Analytics PowerShell Cmdlet 參考文件](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) 。

> [!NOTE]
> Log Analytics 在以前稱為 Operational Insights，這也是 Cmdlet 中使用此名稱的原因。

## <a name="prerequisites"></a>必要條件
這些範例可使用版本 1.0.0 或更新版本的 Az.OperationalInsights 模組。


## <a name="create-and-configure-a-log-analytics-workspace"></a>建立及設定 Log Analytics 工作區
下列指令碼範例說明如何：

1. 建立工作區
2. 列出可用的方案
3. 將方案加入至工作區
4. 匯入已儲存的搜尋
5. 匯出已儲存的搜尋
6. 建立電腦群組
7. 從已安裝 Windows 代理程式的電腦啟用 IIS 記錄檔收集功能
8. 從 Linux 電腦收集邏輯磁碟效能計數器 (% Used Inodes; Free Megabytes; % Used Space; Disk Transfers/sec; Disk Reads/sec; Disk Writes/sec)
9. 從 Linux 電腦收集 syslog 事件
10. 從 Windows 電腦的應用程式事件記錄檔收集錯誤和警告事件
11. 從 Windows 電腦收集記憶體可用 Mb 效能計數器
12. 收集自訂記錄檔

```PowerShell

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1",
    "description": "Example custom log datasource",
    "inputs": [
        {
            "location": {
            "fileSystemLocations": {
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ],
                "linuxFileTypeLogPaths": [ "/var/logs" ]
                }
            },
        "recordDelimiter": {
            "regexDelimiter": {
                "pattern": "\\n",
                "matchIndex": 0,
                "matchIndexSpecified": true,
                "numberedGroup": null
                }
            }
        }
    ],
    "extractions": [
        {
            "extractionName": "TimeGenerated",
            "extractionType": "DateTime",
            "extractionProperties": {
                "dateTimeExtraction": {
                    "regex": null,
                    "joinStringRegex": null
                    }
                }
            }
        ]
    }
"@

# Create the resource group if needed
try {
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json

# Create Computer Group based on a query
New-AzOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernel syslog collection"
Enable-AzOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs

New-AzOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```
在上述範例中，regexDelimiter 已定義為用於換行的 "\\n"。 記錄分隔符號也可能是時間戳記。  以下是支援的格式：

| 格式 | Json RegEx 格式會對標準 RegEx 中每個 \ 使用兩個 \\，因此，如果在 RegEx 應用程式中測試，請將 \\ 減少為 \ | | |
| --- | --- | --- | --- |
| `YYYY-MM-DD HH:MM:SS` | `((\\\\d{2})\|(\\\\d{4}))-([0-1]\\\\d)-(([0-3]\\\\d)\|(\\\\d))\\\\s((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |
| `M/D/YYYY HH:MM:SS AM/PM` | `(([0-1]\\\\d)\|[0-9])/(([0-3]\\\\d)\|(\\\\d))/((\\\\d{2})\|(\\\\d{4}))\\\\s((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]\\\\s(AM\|PM\|am\|pm)` | | |
| `dd/MMM/yyyy HH:MM:SS` | `((([0-3]\\\\d)\` | `(\\\\d))/(Jan\|Feb\|Mar\|May\|Apr\|Jul\|Jun\|Aug\|Oct\|Sep\|Nov\|Dec\|jan\|feb\|mar\|may\|apr\|jul\|jun\|aug\|oct\|sep\|nov\|dec)/((\\\\d{2})\|(\\\\d{4}))\\\\s((\\\\d)\` | `([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9])` |
| `MMM dd yyyy HH:MM:SS` | `(((?:Jan(?:uary)?\|Feb(?:ruary)?\|Mar(?:ch)?\|Apr(?:il)?\|May\|Jun(?:e)?\|Jul(?:y)?\|Aug(?:ust)?\|Sep(?:tember)?\|Sept\|Oct(?:ober)?\|Nov(?:ember)?\|Dec(?:ember)?)).*?((?:(?:[0-2]?\\\\d{1})\|(?:[3][01]{1})))(?![\\\\d]).*?((?:(?:[1]{1}\\\\d{1}\\\\d{1}\\\\d{1})\|(?:[2]{1}\\\\d{3})))(?![\\\\d]).*?((?:(?:[0-1][0-9])\|(?:[2][0-3])\|(?:[0-9])):(?:[0-5][0-9])(?::[0-5][0-9])?(?:\\\\s?(?:am\|AM\|pm\|PM))?))` | | |
| `yyMMdd HH:mm:ss` | `([0-9]{2}([0][1-9]\|[1][0-2])([0-2][0-9]\|[3][0-1])\\\\s\\\\s?([0-1]?[0-9]\|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `ddMMyy HH:mm:ss` | `(([0-2][0-9]\|[3][0-1])([0][1-9]\|[1][0-2])[0-9]{2}\\\\s\\\\s?([0-1]?[0-9]\|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s\\\\s?([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0-1]?[0-9]\|[2][0-3]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM  d HH:mm:ss` <br> MMM 後有兩個空格 | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s\\\\s([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `dd/MMM/yyyy:HH:mm:ss +zzzz` <br> 其中 + 是 + 或 - <br> zzzz 是時間位移 | `(([0-2][1-9]\|[3][0-1])\\\\/(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\/((19\|20)[0-9][0-9]):([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])\\\\s[\\\\+\|\\\\-][0-9]{4})` | | |
| `yyyy-MM-ddTHH:mm:ss` <br> T 是常值的字母 T | `((\\\\d{2})\|(\\\\d{4}))-([0-1]\\\\d)-(([0-3]\\\\d)\|(\\\\d))T((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |

## <a name="configuring-log-analytics-to-send-azure-diagnostics"></a>設定 Log Analytics 來傳送 Azure 診斷
若要以無代理程式的方式監視 Azure 資源，資源需要啟用 Azure 診斷並將其設定為寫入至 Log Analytics 工作區。 此方法會將資料直接傳送到 Log Analytics，而且不需要將資料寫入儲存體帳戶。 支援的資源包括：

| 資源類型 | 記錄檔 | 度量 |
| --- | --- | --- |
| 應用程式閘道    | 是 | 是 |
| 自動化帳戶     | 是 | |
| Batch 帳戶          | 是 | 是 |
| Data Lake Analytics     | 是 | |
| Data Lake Store         | 是 | |
| SQL 彈性集區        |     | 是 |
| 事件中樞命名空間     |     | 是 |
| IoT 中樞                |     | 是 |
| Key Vault               | 是 | |
| 負載平衡器          | 是 | |
| Logic Apps              | 是 | 是 |
| 網路安全性群組 | 是 | |
| Azure Cache for Redis             |     | 是 |
| 搜尋服務         | 是 | 是 |
| 服務匯流排命名空間   |     | 是 |
| SQL (v12)               |     | 是 |
| 網站               |     | 是 |
| Web 伺服器陣列        |     | 是 |

如需可用度量的詳細資訊，請參閱[支援 Azure 監視器的度量](../../azure-monitor/platform/metrics-supported.md)。

如需可用記錄檔的詳細資訊，請參閱[支援的服務以及診斷記錄檔的結構描述](../../azure-monitor/platform/diagnostic-logs-schema.md)。

```PowerShell
$workspaceId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

您也可以使用前述 Cmdlet，來收集不同訂用帳戶中之資源的記錄檔。 因為您會提供建立記錄檔之資源和記錄檔所傳送至之工作區這兩個項目的識別碼，因此這個 Cmdlet 可跨訂用帳戶運作。


## <a name="configuring-log-analytics-to-collect-azure-diagnostics-from-storage"></a>設定 Log Analytics 從儲存體收集 Azure 診斷
若要從傳統雲端服務或 Service Fabric 叢集的執行中執行個體內收集記錄檔資料，您必須先將資料寫入 Azure 儲存體。 然後，可將 Log Analytics 設定為從儲存體帳戶收集記錄檔。 支援的資源包括：

* 傳統雲端服務 (Web 和背景工作角色)
* Service Fabric 叢集

下列範例示範如何執行：

1. 列出現有的儲存體帳戶和 Log Analytics 檢索的資源來源位置
2. 建立組態以讀取儲存體帳戶
3. 更新新建立的組態以檢索其他位置的資料
4. 刪除新建立的組態

```PowerShell
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable"
$workspace = (Get-AzOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to index
$storageId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight"

```

您也可以使用前述指令碼，來收集不同訂用帳戶中之儲存體帳戶的記錄檔。 因為您會提供儲存體帳戶資源識別碼和對應的存取金鑰，因此指令碼可跨訂用帳戶運作。 當您變更存取金鑰時，您需要更新儲存體深入解析使其擁有新的金鑰。


## <a name="next-steps"></a>後續步驟
* [檢閱 Log Analytics PowerShell Cmdlet](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) 。


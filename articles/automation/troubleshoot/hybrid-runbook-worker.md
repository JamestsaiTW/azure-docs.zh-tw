---
title: 疑難排解 - Azure 自動化混合式 Runbook 背景工作角色
description: 本文提供針對 Azure 自動化混合式 Runbook 背景工作角色進行疑難排解的資訊
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 02/12/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 703f27dab6ca6252647ecb37d17d0f4faf045097
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56268681"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>混合式 Runbook 背景工作的疑難排解

本文提供針對混合式 Runbook 背景工作角色問題進行疑難排解的資訊。

## <a name="general"></a>一般

混合式 Runbook 背景工作角色取決於與自動化帳戶通訊的代理程式，來註冊背景工作角色、接收 Runbook 作業及報告狀態。 Windows 的代理程式為 Microsoft Monitoring Agent。 Linux 的代理程式為適用於 Linux 的 OMS 代理程式。

### <a name="runbook-execution-fails"></a>案例：Runbook 執行失敗

#### <a name="issue"></a>問題

Runbook 執行失敗且您收到下列錯誤：

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Runbook 在嘗試執行三次後會馬上暫止。 在某些情況下，Runbook 可能會因為受到干擾而無法完成。 當發生這種情況時，相關錯誤訊息可能不會包含指出原因的任何其他資訊。

#### <a name="cause"></a>原因

可能的原因如下：

* Runbook 無法使用本機資源驗證

* Hybrid Worker 位於 Proxy 或防火牆後方

* Runbook 無法使用本機資源驗證

* 設定用於執行混合式 Runbook 背景工作角色功能的電腦不符合最低硬體需求。

#### <a name="resolution"></a>解決方案

確認電腦可透過連接埠 443 輸出存取 *.azure-automation.net。

執行混合式 Runbook 背景工作角色的電腦應滿足最低硬體需求，您才能設定它來裝載這項功能。 Runbook 及其所使用的背景處理序可能會使系統過度使用，而導致 Runbook 作業延遲或逾時。

確認執行混合式 Runbook 背景工作角色功能的電腦滿足最低硬體需求。 如果滿足最低硬體需求，請監視 CPU 和記憶體使用率，判斷混合式 Runbook 背景工作角色處理序之效能和 Windows 之間是否有任何相互關聯。 如果有記憶體或 CPU 壓力，這可能表示需要升級資源。 您也可以選擇其他可支援最低需求，也能在工作負載需求指出需要增加資源時擴充的計算資源。

查閱 **Microsoft-SMA** 事件記錄檔，找出描述為「Win32 處理序結束，代碼為 [4294967295]」的對應事件。 此錯誤的原因是您尚未在 Runbook 中設定驗證，或尚未指定混合式背景工作角色群組的執行身分認證。 檢閱 [Runbook 權限](../automation-hrw-run-runbooks.md#runbook-permissions)，確認 Runbook 的驗證設定正確無誤。

### <a name="no-cert-found"></a>案例：在混合式 Runbook 背景工作角色的憑證存放區中找不到憑證

#### <a name="issue"></a>問題

混合式 Runbook 背景工作角色上執行的 Runbook 失敗並出現下列錯誤訊息：

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>原因

當您在混合式 Runbook 背景工作角色上執行的 Runbook 中嘗試使用[執行身分帳戶](../manage-runas-account.md)時，若沒有執行身分帳戶憑證，就會發生此錯誤。 混合式 Runbook 背景工作角色預設在本機沒有憑證資產，執行身分帳戶需要此憑證才能正常運作。

#### <a name="resolution"></a>解決方案

如果您的混合式 Runbook 背景工作角色是 Azure VM，您可以改用 [Azure 資源的受控識別](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources)。 此案例可讓您使用 Azure VM 的受控識別來向 Azure 資源進行驗證，而非使用執行身分帳戶，藉此簡化驗證。 當混合式 Runbook 背景工作角色是內部部署電腦時，您必須在電腦上安裝執行身分帳戶憑證。 若要了解如何安裝憑證，請參閱執行 [Export-RunAsCertificateToHybridWorker](../automation-hrw-run-runbooks.md#runas-script) Runbook 的步驟。

## <a name="linux"></a> Linux

Linux 混合式 Runbook 背景工作角色仰賴適用於 Linux 的 OMS 代理程式來與自動化帳戶通訊，以便註冊背景工作角色、接收 Runbook 作業和報告狀態。 如果註冊背景工作角色失敗，請參考以下一些可能的錯誤原因：

### <a name="oms-agent-not-running"></a>案例：適用於 Linux 的 OMS 代理程式未執行

#### <a name="issue"></a>問題

適用於 Linux 的 OMS 代理程式未執行

#### <a name="cause"></a>原因

如果適用於 Linux 的 OMS 代理程式未執行，就會讓 Linux 混合式 Runbook 背景工作角色無法與 Azure 自動化進行通訊。 代理程式無法執行的原因有很多種。

#### <a name="resolution"></a>解決方案

 輸入下列命令，確認代理程式正在執行：`ps -ef | grep python`。 您所看到的輸出應該會類似下列具有 **nxautomation** 使用者帳戶的 Python 處理序。 如果未啟用更新管理或 Azure 自動化解決方案，下列處理序都不會執行。

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

下列清單顯示已針對 Linux 混合式 Runbook 背景工作角色啟動的處理序。 這些程序全都位於 `/var/opt/microsoft/omsagent/state/automationworker/` 目錄。


* **oms.conf** - 此值是背景工作管理員程序。 它會直接從 DSC 啟動。

* **worker.conf** - 此處理序是自動註冊的混合式背景工作角色處理序，會由背景工作角色管理員來啟動。 更新管理會使用此處理序，且使用者不會注意到此處理序。 如果機器上未啟用更新管理解決方案，就不會出現此處理序。

* **diy/worker.conf** - 此處理序是 DIY 混合式背景工作處理序。 DIY 混合式背景工作處理序可用來在混合式 Runbook 背景工作角色上執行使用者 Runbook。 它與自動註冊的混合式背景工作處理序的主要差別在於會使用不同的設定。 如果已停用 Azure 自動化解決方案，而且未註冊 DIY Linux 混合式背景工作，就不會出現此處理序。

如果適用於 Linux 的 OMS 代理程式未執行，請執行下列命令來啟動服務：`sudo /opt/microsoft/omsagent/bin/service_control restart`。

### <a name="class-does-not-exist"></a>案例：指定的類別不存在

如果您看到錯誤：**指定的類別不存在。** `/var/opt/microsoft/omsconfig/omsconfig.log` 中，表示適用於 Linux 的 OMS 代理程式需要更新。 請執行下列命令來重新安裝 OMS 代理程式：

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

Windows 混合式 Runbook 背景工作角色取決於與自動化帳戶通訊的 Microsoft Monitoring Agent 來註冊背景工作角色、接收 Runbook 作業及報告狀態。 如果註冊背景工作角色失敗，請參考以下一些可能的錯誤原因：

### <a name="mma-not-running"></a>案例：Microsoft Monitoring Agent 未執行

#### <a name="issue"></a>問題

`healthservice` 服務未在混合式 Runbook 背景工作角色機器上執行。

#### <a name="cause"></a>原因

如果 Microsoft Monitoring Agent Windows 服務未執行，此狀態會讓混合式 Runbook 背景工作無法與 Azure 自動化進行通訊。

#### <a name="resolution"></a>解決方案

在 PowerShell 中輸入下列命令，確認代理程式正在執行：`Get-Service healthservice`。 如果服務已停止，在 PowerShell 中輸入下列命令可啟動服務：`Start-Service healthservice`。

### <a name="event-4502"></a> Operations Manager 記錄中的事件 4502

#### <a name="issue"></a>問題

在 **Application and Services Logs\Operations Manager** 事件記錄中，您會看到事件 4502 和 EventMessage 包含 **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**，並具有如下描述：*服務 \<wsid\>.oms.opinsights.azure.com 所提供的憑證並非由 Microsoft 服務所使用的憑證授權單位發行。請連絡網路管理員，以查看它們是否正在執行可攔截 TLS/SSL 通訊的 Proxy。文章 KB3126513 內提供針對連線能力問題的其他疑難排解資訊。*

#### <a name="cause"></a>原因

此問題是因 Proxy 或網路防火牆封鎖 Microsoft Azure 的通訊所引起。 確認電腦可透過連接埠 443 輸出存取 *.azure-automation.net。

#### <a name="resolution"></a>解決方案

記錄檔儲存每一個混合式背景工作角色本機的 C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes 中。 您可以檢查 **Application and Services Logs\Microsoft-SMA\Operations** 和 **Application and Services Logs\Operations Manager** 事件記錄中，是否有任何警告或錯誤事件可能表示有連線能力或其他會影響角色上架到 Azure 自動化的問題，或正常作業時的問題。

[Runbook 輸出和訊息](../automation-runbook-output-and-messages.md) 會從混合式背景工作角色傳送到 Azure 自動化，就像雲端中執行的 Runbook 工作一樣。 您也可以啟用詳細資訊和進度資料流，就像您在其他 Runbook 中的作法一樣。

### <a name="corrupt-cache"></a> 混合式 Runbook 背景工作角色未報告

#### <a name="issue"></a>問題

您的混合式 Runbook 背景工作角色電腦正在執行中，但您看不到工作區中機器的活動訊號資料。

下列範例查詢會顯示工作區中的機器及其上次活動訊號：

```loganalytics
// Last heartbeat of each computer
Heartbeat 
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>原因

這個問題可能是因為混合式 Runbook 背景工作角色上的快取損毀所造成的。

#### <a name="resolution"></a>解決方案

若要解決此問題，請登入混合式 Runbook 背景工作角色，並執行下列程式碼。 此指令碼會停止 Microsoft Monitoring Agent、移除其快取，並重新啟動服務。 此動作會強制混合式 Runbook 背景工作角色重新從「Azure 自動化」下載其設定。

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

### <a name="already-registered"></a>案例：您無法新增混合式 Runbook 背景工作角色

#### <a name="issue"></a>問題

您在嘗試使用 `Add-HybridRunbookWorker` Cmdlet 來新增「混合式 Runbook 背景工作角色」時收到下列訊息。

```
Machine is already registered to a different account
```

#### <a name="cause"></a>原因

這可能是因為機器已經向不同的「自動化帳戶」註冊，或是您在將「混合式 Runbook 背景工作角色」從機器中移除之後，嘗試重新新增此背景工作角色。

#### <a name="resolution"></a>解決方案

若要解決此問題，請移除下列登錄機碼，然後重新啟動 `HealthService` 並重新嘗試 `Add-HybridRunbookWorker` Cmdlet：

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\HybridRunbookWorker`

## <a name="next-steps"></a>後續步驟

如果您沒有看到您的問題，或無法解決您的問題，請瀏覽下列其中一個管道以取得更多支援：

* 透過 [Azure 論壇](https://azure.microsoft.com/support/forums/)獲得由 Azure 專家所提供的解答
* 與 [@AzureSupport](https://twitter.com/azuresupport) 連繫－專為改善客戶體驗而設的官方 Microsoft Azure 帳戶，協助 Azure 社群連接至適當的資源，像是解答、支援及專家等。
* 如果需要更多協助，您可以提出 Azure 支援事件。 請移至 [Azure 支援網站](https://azure.microsoft.com/support/options/)，然後選取 [取得支援]。


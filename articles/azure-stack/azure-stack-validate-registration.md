---
title: 驗證 Azure Stack 的 Azure 註冊 | Microsoft Docs
description: 使用 Azure Stack 整備檢查程式來驗證 Azure 註冊。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: a777fc1d9052eb58bbebd319fe6cc7f42a09cb9a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403607"
---
# <a name="validate-azure-registration"></a>驗證 Azure 註冊

使用 Azure Stack 整備檢查程式工具 (**AzsReadinessChecker**) 來驗證 Azure 訂用帳戶是否已準備好與 Azure Stack 搭配使用。 開始部署 Azure Stack 之前，請先驗證註冊。 整備檢查程式會驗證：

- 您使用的 Azure 訂用帳戶是否為支援的類型。 訂用帳戶必須是雲端服務提供者 (CSP) 或 Enterprise 合約 (EA)。
- 您用來向 Azure 註冊訂用帳戶的帳戶可以登入 Azure，且為訂用帳戶的擁有者。

如需 Azure Stack 註冊的詳細資訊，請參閱[向 Azure 註冊 Azure Stack](azure-stack-registration.md)。

## <a name="get-the-readiness-checker-tool"></a>取得整備檢查程式工具

從 **PowerShell 資源庫** \(英文\) 下載最新版的 [AzsReadinessChecker](https://aka.ms/AzsReadinessChecker)。  

## <a name="prerequisites"></a>必要條件

需要下列必要條件：

**執行這個工具的電腦：**

- Windows 10 或 Windows Server 2016，具有網際網路連線能力。
- PowerShell 5.1 或更新版本。 若要檢查版本，請執行下列 PowerShell Cmdlet，然後再檢閱「主要」和「次要」版本：  

  ```powershell
  $PSVersionTable.PSVersion
  ```

- [針對 Azure Stack 設定的 PowerShell](azure-stack-powershell-install.md)。
- 最新版的 [Microsoft Azure Stack 整備檢查程式](https://aka.ms/AzsReadinessChecker)。  

**Azure Active Directory 環境：**

- 識別會與 Azure Stack 搭配使用的 Azure 訂用帳戶擁有者的帳戶使用者名稱和密碼。  
- 識別您會使用的 Azure 訂用帳戶的訂用帳戶識別碼。
- 識別您將使用的 **AzureEnvironment**。 支援的環境名稱參數值為 **AzureCloud**、**AzureChinaCloud** 或 **AzureUSGovernment**，視您使用哪一個 Azure 訂用帳戶而定。

## <a name="steps-to-validate-azure-registration"></a>驗證 Azure 註冊的步

1. 在符合必要條件的電腦上，開啟提高權限的 PowerShell 提示字元，然後執行下列命令以安裝 **AzsReadinessChecker**：

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. 從 PowerShell 提示字元中，執行下列命令以將 `$registrationCredential` 設定為訂用帳戶擁有者的帳戶。 用您的帳戶和租用戶名稱取代 `subscriptionowner@contoso.onmicrosoft.com`：

   ```powershell
   $registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"
   ```

   > [!NOTE]
   > 身為 CSP，在使用共用服務或 IUR 訂用帳戶時，您必須從各自的 AAD 分別提供使用者認證。 通常，這會類似於 `subscriptionowner@iurcontoso.onmicrosoft.com`。 如以上步驟所述，該使用者必須擁有適當的認證。

3. 從 PowerShell 提示字元中，執行下列命令以將 `$subscriptionID` 設定為您將使用的 Azure 訂用帳戶。 以您自己的訂用帳戶 ID 取代 `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`：

   ```powershell
   $subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
   ```

4. 從 PowerShell 提示字元中，執行下列命令以開始驗證您的訂用帳戶：

   - 將 `AzureEnvironment` 的值指定為 **AzureCloud**、**AzureGermanCloud** 或 **AzureChinaCloud**。  
   - 提供 Azure Active Directory 系統管理員和 Azure Active Directory 租用戶名稱。

   ```powershell
   Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID
   ```

5. 在此工具執行之後，請檢閱輸出。 確認登入和註冊需求的狀態均正確。 驗證成功時，系統會顯示類似以下範例的輸出：

   ```shell
   Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
   Checking Registration Requirements: OK
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsRegistrationValidation Completed
   ```

## <a name="report-and-log-file"></a>報告與記錄檔

每當驗證執行時，它會將結果記錄到 **AzsReadinessChecker.log** 和 **AzsReadinessCheckerReport.json**。 PowerShell 中的驗證結果會一同顯示這些檔案的位置。

這些檔案可協助您在部署 Azure Stack 或調查驗證問題之前共用驗證狀態。 這兩個檔案會保存每個後續驗證檢查的結果。 此報告會向部署團隊提供身分識別設定的確認。 記錄檔可協助部署或支援小組調查驗證問題。

根據預設，這兩個檔案都會寫入到 C:\Users\<使用者名稱>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json。  

- 使用執行命令列結尾的 **-OutputPath** ***&lt;path&gt;*** 參數來指定不同的報告位置。
- 使用執行命令結尾的 **-CleanReport** 參數，從 **AzsReadinessCheckerReport.json** 清除先前執行工具時留下的資訊。

如需詳細資訊，請參閱 [Azure Stack 驗證報告](azure-stack-validation-report.md)。

## <a name="validation-failures"></a>驗證失敗

如果驗證檢查失敗，則會在 PowerShell 視窗中顯示有關失敗的詳細資料。 此工具也會將相關資訊記錄至 **AzsReadinessChecker.log** 檔案中。

下列範例會提供有關常見驗證失敗的指引：

### <a name="user-must-be-an-owner-of-the-subscription"></a>使用者必須是訂用帳戶的擁有者

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**原因** - 帳戶不是 Azure 訂用帳戶的系統管理員。

**解決方式** - 使用屬於 Azure 訂用帳戶系統管理員的帳戶，以作為 Azure Stack 部署的使用量計費帳戶。

### <a name="expired-or-temporary-password"></a>過期或暫時的密碼

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription [subscription ID] using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change Password.
Trace ID: [Trace ID]
Correlation ID: [Correlation ID]
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**原因** - 帳戶無法登入，因為密碼已過期，或屬於暫時密碼。

**解決方式** - 在 PowerShell 中執行下列命令，然後遵循提示來重設密碼。

```powershell
Login-AzureRMAccount
```

或者，以該帳戶的擁有者身分登入 [Azure 入口網站](https://portal.azure.com)，然後系統將會強制使用者變更密碼。

### <a name="unknown-user-type"></a>不明的使用者類型  

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription <subscription ID> using <account> failed with unknown_user_type: Unknown User Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**原因** - 帳戶無法登入指定的 Azure Active Directory 環境。 在此範例中，AzureChinaCloud 會指定為 AzureEnvironment。  

**解決方式** - 確認此帳戶對指定的 Azure 環境是有效的。 在 PowerShell 中，執行下列命令以確認此帳戶是可在特定環境中使用的有效帳戶：

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

## <a name="next-steps"></a>後續步驟

- [驗證 Azure 身分識別](azure-stack-validate-identity.md)
- [檢視整備報告](azure-stack-validation-report.md)
- [一般 Azure Stack 整合考量](azure-stack-datacenter-integration.md)

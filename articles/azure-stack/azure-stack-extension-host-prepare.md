---
title: Azure Stack 的延伸主機準備 | Microsoft Docs
description: 了解如何為透過未來 Azure Stack 更新套件自動啟用的延伸主機做好準備。
services: azure-stack
keywords: ''
author: mattbriggs
ms.author: mabrigg
ms.date: 02/07/2019
ms.topic: article
ms.service: azure-stack
ms.reviewer: thoroet
manager: femila
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: b0d3b3e4901fbcece13c201938be8bccb1bb9c82
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55962361"
---
# <a name="prepare-for-extension-host-for-azure-stack"></a>Azure Stack 的延伸主機準備

延伸主機可藉由減少所需的 TCP/IP 連接埠數目來保護 Azure Stack。 本文將探討如何準備 Azure Stack 以使用延伸主機，此功能會透過 1808 更新之後的 Azure Stack 更新套件自動啟用。 本文適用於 Azure Stack 更新 1808、1809 和 1811。

## <a name="certificate-requirements"></a>憑證需求

延伸主機會實作兩個新網域命名空間，以確保每個入口網站延伸模組都有唯一的主機項目。 新網域命名空間需要兩個額外的萬用字元憑證，如此才能確保通訊安全。

下表顯示新的命名空間和相關聯的憑證：

| 部署資料夾 | 必要的憑證主體和主體別名 (SAN) | 範圍 (每個區域) | 子網域命名空間 |
|-----------------------|------------------------------------------------------------------|-----------------------|------------------------------|
| 管理員延伸主機 | *.adminhosting.\<region>.\<fqdn> (萬用字元 SSL 憑證) | 管理員延伸主機 | adminhosting.\<region>.\<fqdn> |
| 公用延伸主機 | *.hosting.\<region>.\<fqdn> (萬用字元 SSL 憑證) | 公用延伸主機 | hosting.\<region>.\<fqdn> |

您可以在 [Azure Stack 公開金鑰基礎結構憑證需求](azure-stack-pki-certs.md)一文中，找到詳細的憑證要求。

## <a name="create-certificate-signing-request"></a>建立憑證簽署要求

Azure Stack 整備檢查工具可讓您為兩個必要的新 SSL 憑證建立憑證簽署要求。 請遵循 [Azure Stack 憑證簽署要求產生](azure-stack-get-pki-certs.md)一文中的步驟。

> [!Note]  
> 視您要求 SSL 憑證的方式而定，您也許可以略過此步驟。

## <a name="validate-new-certificates"></a>驗證新的憑證

1. 使用硬體生命週期主機或 Azure Stack 管理工作站上提升的權限開啟 PowerShell。
2. 執行下列 Cmdlet 來安裝 Azure Stack 整備檢查工具。

    ```PowerShell  
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker
    ```

3. 執行下列指令碼來建立必要的資料夾結構。

    ```PowerShell  
    New-Item C:\Certificates -ItemType Directory

    $directories = 'ACSBlob','ACSQueue','ACSTable','Admin Portal','ARM Admin','ARM Public','KeyVault','KeyVaultInternal','Public Portal', 'Admin extension host', 'Public extension host'

    $destination = 'c:\certificates'

    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!Note]  
    > 如果您使用 Azure Active Directory 同盟服務 (AD FS) 進行部署，必須將下列目錄新增至指令碼中的 **$directories**：`ADFS`、`Graph`。

4. 執行下列 Cmdlet 來啟動憑證檢查：

    ```PowerShell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Start-AzsReadinessChecker -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
    ```

5. 將您的憑證放在上適當目錄。

6. 檢查輸出，所有憑證都通過所有測試。


## <a name="import-extension-host-certificates"></a>匯入延伸主機憑證

針對後續步驟，請使用可以連線到 Azure Stack 特殊權限端點的電腦。 請確定您可以從該電腦存取新的的憑證檔案。

1. 針對後續步驟，請使用可以連線到 Azure Stack 特殊權限端點的電腦。 請確定您可以從該電腦存取新的的憑證檔案。
2. 開啟 PowerShell ISE 來執行後續的指令碼區塊
3. 匯入管理員主機端點的憑證。

    ```PowerShell  

    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$AdminHostingCertContent = [Byte[]](Get-Content c:\certificate\myadminhostingcertificate.pfx -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($AdminHostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($AdminHostingCertContent, $CertPassword)
            Import-AdminHostingServiceCert $AdminHostingCertContent $certPassword
    }
    ```
4. 匯入主機端點的憑證。
    ```PowerShell  
    $CertPassword = read-host -AsSecureString -prompt "Certificate Password"

    $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."

    [Byte[]]$HostingCertContent = [Byte[]](Get-Content c:\certificate\myhostingcertificate.pfx  -Encoding Byte)

    Invoke-Command -ComputerName <PrivilegedEndpoint computer name> `
    -Credential $CloudAdminCred `
    -ConfigurationName "PrivilegedEndpoint" `
    -ArgumentList @($HostingCertContent, $CertPassword) `
    -ScriptBlock {
            param($HostingCertContent, $CertPassword)
            Import-UserHostingServiceCert $HostingCertContent $certPassword
    }
    ```

### <a name="update-dns-configuration"></a>更新 DNS 組態

> [!Note]  
> 如果您已使用 DNS 整合的 DNS 區域委派，則不需要此步驟。
如果已設定個別主機 A 記錄來發佈 Azure Stack 端點，則您需要建立兩個額外的主機 A 記錄：

| IP | 主機名稱 | 類型 |
|----|------------------------------|------|
| \<IP> | *.Adminhosting.\<>.\<FQDN> | 具有使用  |
| \<IP> | *.Hosting.\<>.\<FQDN> | 具有使用  |

您可以透過執行 **Get AzureStackStampInformation** Cmdlet，使用特殊權限端點來擷取已配置的 IP。

### <a name="ports-and-protocols"></a>連接埠和通訊協定

在 [Azure Stack 資料中心整合 - 發佈端點](azure-stack-integrate-endpoints.md)一文中，包含推出延伸主機之前，需要輸入通訊才能發佈 Azure Stack 的連接埠和通訊協定。

### <a name="publish-new-endpoints"></a>發佈新端點

有兩個新端點需透過您的防火牆來發佈。 您可使用下列程式碼來擷取來自公用 VIP 集區的已配置 IP；您必須透過您 Azure Stack [環境的具特殊權限端點](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint)來執行該程式碼。

```PowerShell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Obtain DNS Servers and Extension Host information from Azure Stack Stamp Information and find the IPs for the Host Extension Endpoints
$StampInformation = Invoke-Command $PEPSession {Get-AzureStackStampInformation} | Select-Object -Property ExternalDNSIPAddress01, ExternalDNSIPAddress02, @{n="TenantHosting";e={($_.TenantExternalEndpoints.TenantHosting) -replace "https://*.","testdnsentry"-replace "/"}},  @{n="AdminHosting";e={($_.AdminExternalEndpoints.AdminHosting)-replace "https://*.","testdnsentry"-replace "/"}},@{n="TenantHostingDNS";e={($_.TenantExternalEndpoints.TenantHosting) -replace "https://",""-replace "/"}},  @{n="AdminHostingDNS";e={($_.AdminExternalEndpoints.AdminHosting)-replace "https://",""-replace "/"}}
If (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress01 -Name $StampInformation.TenantHosting -ErrorAction SilentlyContinue) {
    Write-Host "Can access AZS DNS" -ForegroundColor Green
    $AdminIP = (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress02 -Name $StampInformation.AdminHosting).IPAddress
    Write-Host "The IP for the Admin Extension Host is: $($StampInformation.AdminHostingDNS) - is: $($AdminIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.AdminHostingDNS), Value: $($AdminIP)" -ForegroundColor Green
    $TenantIP = (Resolve-DnsName -Server $StampInformation.ExternalDNSIPAddress01 -Name $StampInformation.TenantHosting).IPAddress
    Write-Host "The IP address for the Tenant Extension Host is $($StampInformation.TenantHostingDNS) - is: $($TenantIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.TenantHostingDNS), Value: $($TenantIP)" -ForegroundColor Green
}
Else {
    Write-Host "Cannot access AZS DNS" -ForegroundColor Yellow
    $AdminIP = (Resolve-DnsName -Name $StampInformation.AdminHosting).IPAddress
    Write-Host "The IP for the Admin Extension Host is: $($StampInformation.AdminHostingDNS) - is: $($AdminIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.AdminHostingDNS), Value: $($AdminIP)" -ForegroundColor Green
    $TenantIP = (Resolve-DnsName -Name $StampInformation.TenantHosting).IPAddress
    Write-Host "The IP address for the Tenant Extension Host is $($StampInformation.TenantHostingDNS) - is: $($TenantIP)" -ForegroundColor Yellow
    Write-Host "The Record to be added in the DNS zone: Type A, Name: $($StampInformation.TenantHostingDNS), Value: $($TenantIP)" -ForegroundColor Green
}
Remove-PSSession -Session $PEPSession
```

#### <a name="sample-output"></a>範例輸出

```PowerShell
Can access AZS DNS
The IP for the Admin Extension Host is: *.adminhosting.\<region>.\<fqdn> - is: xxx.xxx.xxx.xxx
The Record to be added in the DNS zone: Type A, Name: *.adminhosting.\<region>.\<fqdn>, Value: xxx.xxx.xxx.xxx
The IP address for the Tenant Extension Host is *.hosting.\<region>.\<fqdn> - is: xxx.xxx.xxx.xxx
The Record to be added in the DNS zone: Type A, Name: *.hosting.\<region>.\<fqdn>, Value: xxx.xxx.xxx.xxx
```

> [!Note]  
> 請先進行這項變更，再啟用延伸主機。 這可讓 Azure Stack 入口網站維持可存取的狀態。

| 端點 (VIP) | 通訊協定 | 連接埠 |
|----------------|----------|-------|
| 管理員主機 | HTTPS | 443 |
| 裝載 | HTTPS | 443 |

### <a name="update-existing-publishing-rules-post-enablement-of-extension-host"></a>更新現有的發佈規則 (延伸主機的啟用後動作)

> [!Note]  
> 1808 Azure Stack 更新套件**還不會**啟用延伸主機。 但會允許匯入必要的憑證來為延伸主機做好準備。 在延伸主機透過 1808 更新之後的 Azure Stack 更新套件自動啟用之前，請勿關閉任何通訊埠。

您必須關閉現有防火牆規則中的下列現有端點連接埠。

> [!Note]  
> 建議您在驗證成功之後，再關閉這些連接埠。

| 端點 (VIP) | 通訊協定 | 連接埠 |
|----------------------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| 入口網站 (系統管理員) | HTTPS | 12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13012<br>13020<br>13021<br>13026<br>30015 |
| 入口網站 (使用者) | HTTPS | 12495<br>12649<br>13001<br>13010<br>13011<br>13012<br>13020<br>13021<br>30015<br>13003 |
| Azure Resource Manager (系統管理員) | HTTPS | 30024 |
| Azure Resource Manager (使用者) | HTTPS | 30024 |

## <a name="next-steps"></a>後續步驟

- 深入了解[防火牆整合](azure-stack-firewall.md)。
- 深入了解 [Azure Stack 憑證簽署要求產生](azure-stack-get-pki-certs.md)

---
title: 將 Kubernetes 新增至 Azure Stack Marketplace | Microsoft Docs
description: 了解如何將 Kubernetes 新增至 Azure Stack Marketplace。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: d0051f081f005d61a1eed43d177a11781b2b3fa8
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997078"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>將 Kubernetes 新增至 Azure Stack Marketplace

*適用於：Azure Stack 整合式系統和 Azure Stack 開發套件*

> [!note]  
> Azure Stack 上的 Kubernetes 處於預覽狀態。 預覽版目前不支援 Azure Stack 中斷連線的案例。

您能以 Marketplace 項目的方式向使用者提供 Kubernetes。 接著您的使用者可以在單一協調作業中部署 Kubernetes。

以下文章說明如何使用 Azure Resource Manager 範本來部署及佈建獨立 Kubernetes 叢集的資源。 Kubernetes Cluster Marketplace 項目 0.3.0 需要 Azure Stack 1808 版。 開始之前，請檢查您的 Azure Stack 和全域的 Azure 租用戶設定。 收集有關您 Azure Stack 的必要資訊。 將必要的資源新增到您的租用戶及 Azure Stack Marketplace。 叢集仰賴 Ubuntu 伺服器、自訂指令碼和會在 Marketplace 中的 Kubernetes 項目。

## <a name="create-a-plan-an-offer-and-a-subscription"></a>建立方案、供應項目和訂用帳戶

建立 Kubernetes Marketplace 項目的方案、供應項目和訂用帳戶。 您也可以使用現有的方案和供應項目。

1. 登入[系統管理入口網站](https://adminportal.local.azurestack.external)。

1. 建立一個方案作為基本方案。 如需指示，請參閱[在 Azure Stack 中建立方案](azure-stack-create-plan.md)。

1. 建立供應項目。 如需指示，請參閱[在 Azure Stack 中建立供應項目](azure-stack-create-offer.md)。

1. 選取 [供應項目]，並尋找您所建立的供應項目。

1. 在 [供應項目] 刀鋒視窗中選取 [概觀]。

1. 選取 [變更狀態]。 選取 [公用]。

1. 選取 [+ 建立資源] > [供應項目與方案] > [訂用帳戶] 來建立訂用帳戶。

    a. 輸入 [顯示名稱]。

    b. 輸入 [使用者]。 使用與您的租用戶相關聯的 Azure AD 帳戶。

    c. [提供者描述]

    d. 將 [目錄租用戶] 設定成 Azure Stack 的 Azure AD 租用戶。 

    e. 選取 [供應項目]。 選取您所建立供應項目的名稱。 記下訂用帳戶識別碼。

## <a name="create-a-service-principal-and-credentials-in-ad-fs"></a>在 AD FS 中建立服務主體與認證

如果您要為您的身分識別管理服務使用 Active Directory 同盟服務 (AD FS)，您必須為部署 Kubernetes 叢集的使用者建立服務主體。

1. 建立並匯出用來建立服務主體的自我簽署憑證。 

    - 您需要下列幾個資訊：

       | 值 | 說明 |
       | ---   | ---         |
       | 密碼 | 輸入憑證的新密碼。 |
       | 本機憑證路徑 | 輸入憑證的路徑和檔案名稱。 例如：`c:\certfilename.pfx` |
       | 憑證名稱 | 輸入憑證的名稱。 |
       | 憑證存放區位置 |  例如， `Cert:\LocalMachine\My` |

    - 使用已提升權限的提示字元開啟 PowerShell。 執行下列程式碼，並將參數更新為您的值：

        ```PowerShell  
        # Creates a new self signed certificate 
        $passwordString = "<password>"
        $certlocation = "<local certificate path>.pfx"
        $certificateName = "CN=<certificate name>"
        $certStoreLocation="<certificate store location>"
        
        $params = @{
        CertStoreLocation = $certStoreLocation
        DnsName = $certificateName
        FriendlyName = $certificateName
        KeyLength = 2048
        KeyUsageProperty = 'All'
        KeyExportPolicy = 'Exportable'
        Provider = 'Microsoft Enhanced Cryptographic Provider v1.0'
        HashAlgorithm = 'SHA256'
        }
        
        $cert = New-SelfSignedCertificate @params -ErrorAction Stop
        Write-Verbose "Generated new certificate '$($cert.Subject)' ($($cert.Thumbprint))." -Verbose
        
        #Exports certificate with password in a .pfx format
        $pwd = ConvertTo-SecureString -String $passwordString -Force -AsPlainText
        Export-PfxCertificate -cert $cert -FilePath $certlocation -Password $pwd
        ```

2.  記下您 PowerShell 工作階段中所顯示的新憑證識別碼 `1C2ED76081405F14747DC3B5F76BB1D83227D824`。 此識別碼將會在建立服務主體時使用。

    ```PowerShell  
    VERBOSE: Generated new certificate 'CN=<certificate name>' (1C2ED76081405F14747DC3B5F76BB1D83227D824).
    ```

3. 使用憑證來建立服務主體。

    - 您需要下列幾個資訊：

       | 值 | 說明                     |
       | ---   | ---                             |
       | ERCS IP | 在 ASDK 中，特殊權限的端點通常為 `AzS-ERCS01`。 |
       | 應用程式名稱 | 輸入應用程式服務主體的簡單名稱。 |
       | 憑證存放區位置 | 電腦上您儲存憑證的路徑。 這會透過儲存位置及第一個步驟中所產生的憑證識別碼來表示。 例如：`Cert:\LocalMachine\My\1C2ED76081405F14747DC3B5F76BB1D83227D824` |

       出現提示時，使用下列認證來連線至權限端點。 
        - 使用者名稱：指定 CloudAdmin 帳戶，格式為 <Azure Stack domain>\cloudadmin (如果是 ASDK，使用者名稱為 azurestack\cloudadmin)。
        - 密碼：輸入與 AzureStackAdmin 網域系統管理員帳戶安裝期間所提供的相同密碼。

    - 執行下列程式碼，並將參數更新為您的值：

        ```PowerShell  
        #Create service principal using the certificate
        $privilegedendpoint="<ERCS IP>"
        $applicationName="<application name>"
        $certStoreLocation="<certificate location>"
        
        # Get certificate information
        $cert = Get-Item $certStoreLocation
        
        # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
        $creds = Get-Credential

        # Creating a PSSession to the ERCS PrivilegedEndpoint
        $session = New-PSSession -ComputerName $privilegedendpoint -ConfigurationName PrivilegedEndpoint -Credential $creds

        # Get Service principal Information
        $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name "$using:applicationName" -ClientCertificates $using:cert}

        # Get Stamp information
        $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }

        # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
        $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

        # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
        $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

        # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
        $TenantID = $AzureStackInfo.AADTenantID

        # Register an AzureRM environment that targets your Azure Stack instance
        Add-AzureRMEnvironment `
        -Name "AzureStackUser" `
        -ArmEndpoint $ArmEndpoint

        # Set the GraphEndpointResourceId value
        Set-AzureRmEnvironment `
        -Name "AzureStackUser" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true
        Add-AzureRmAccount -EnvironmentName "azurestackuser" `
        -ServicePrincipal `
        -CertificateThumbprint $ServicePrincipal.Thumbprint `
        -ApplicationId $ServicePrincipal.ClientId `
        -TenantId $TenantID

        # Output the SPN details
        $ServicePrincipal
        ```

    - 服務主體詳細資料類似於下列程式碼片段

        ```Text  
        ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
        ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
        Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
        ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
        PSComputerName        : azs-ercs01
        RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
        ```

## <a name="add-an-ubuntu-server-image"></a>新增 Ubuntu 伺服器映像

將以下 Ubuntu Server 映像新增到 Marketplace：

1. 登入[系統管理入口網站](https://adminportal.local.azurestack.external)。

1. 選取 [所有服務]，然後在 [管理] 類別下，選取 [Marketplace 管理]。

1. 選取 [+ 從 Azure 加入]。

1. 輸入 `Ubuntu Server` 。

1. 選取伺服器的最新版本。 檢查完整版本，並確定您已有最新的版本：
    - **發行者**：Canonical
    - **供應項目**：UbuntuServer
    - **版本**：16.04.201806120 (或最新版本)
    - **SKU**：16.04-LTS

1. 選取 [下載]。

## <a name="add-a-custom-script-for-linux"></a>新增 Custom Script for Linux

從 Marketplace 新增 Kubernetes：

1. 開啟[系統管理入口網站](https://adminportal.local.azurestack.external)。

1. 選取 [所有服務]，然後在 [管理] 類別下，選取 [Marketplace 管理]。

1. 選取 [+ 從 Azure 加入]。

1. 輸入 `Custom Script for Linux` 。

1. 選取具有下列設定檔的指令碼：
    - **供應項目**：Custom Script for Linux 2.0
    - **版本**：2.0.6 (或最新版本)
    - **發行者**：Microsoft Corp

    > [!Note]  
    > 列出的 Custom Script for Linux 版本可能會超過一個。 您必須新增最新版的項目。

1. 選取 [下載]。


## <a name="add-kubernetes-to-the-marketplace"></a>將 Kubernetes 新增至 Marketplace

1. 開啟[系統管理入口網站](https://adminportal.local.azurestack.external)。

1. 選取 [所有服務]，然後在 [管理] 類別下，選取 [Marketplace 管理]。

1. 選取 [+ 從 Azure 加入]。

1. 輸入 `Kubernetes` 。

1. 選取 `Kubernetes Cluster`。

1. 選取 [下載]。

    > [!note]  
    > 可能需要五分鐘，Marketplace 項目才會出現在 Marketplace 中。

    ![Kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>更新或移除 Kubernetes 

更新 Kubernetes 項目時，您將移除 Marketplace 中的過去項目。 請遵循這篇文章中的指示，將 Kubernetes 更新新增至 Marketplace。

移除 Kubernetes 項目：

1. 以操作員的身分使用 PowerShell 連線到 Azure Stack。 如需指示，請參閱[以操作員的身分使用 PowerShell 連線到 Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin)。

2. 在資源庫中尋找目前的 Kubernetes 叢集項目。

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. 請記下目前項目的名稱，例如 `Microsoft.AzureStackKubernetesCluster.0.3.0`

4. 使用以下 PowerShell Cmdlet 來移除項目：

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>後續步驟

[將 Kubernetes 部署至 Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

[Azure Stack 中的服務提供概觀](azure-stack-offer-services-overview.md)

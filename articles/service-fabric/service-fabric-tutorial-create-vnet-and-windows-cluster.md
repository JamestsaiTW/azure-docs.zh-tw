---
title: 在 Azure 中建立 Windows Service Fabric 叢集 | Microsoft Docs
description: 在本教學課程中，您會了解如何使用 PowerShell，將 Windows Service Fabric 叢集部署到 Azure 虛擬網路和網路安全性群組。
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/19/2019
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 64ba17053179d428f5ef7e5ce9685240bde6665f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822986"
---
# <a name="tutorial-deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>教學課程：將安全的 Service Fabric Windows 叢集部署到 Azure 虛擬網路

本教學課程是一個系列的第一部分。 您將會了解如何使用 PowerShell 和範本將執行 Windows 的 Service Fabric 叢集部署到 [Azure 虛擬網路 (VNET)](../virtual-network/virtual-networks-overview.md) 和[網路安全性群組](../virtual-network/virtual-networks-nsg.md)中。 完成時，您會有在您可以部署應用程式的雲端中執行的叢集。  若要使用 Azure CLI 建立 Linux 叢集，請參閱[在 Azure 上建立安全的 Linux 叢集](service-fabric-tutorial-create-vnet-and-linux-cluster.md)。

此教學課程說明的是生產環境案例。  如果您想要快速建立測試用的小型叢集，請參閱[建立測試叢集](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)。

在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 使用 PowerShell 在 Azure 中建立 VNET
> * 建立金鑰保存庫並上傳憑證
> * 設定 Azure Active Directory 驗證
> * 在 Azure PowerShell 中建立安全的 Service Fabric 叢集
> * 使用 X.509 憑證保護叢集
> * 使用 PowerShell 連線到叢集
> * 刪除叢集

在本教學課程系列中，您將了解如何：
> [!div class="checklist"]
> * 在 Azure 上建立安全叢集
> * [將叢集相應縮小或相應放大](service-fabric-tutorial-scale-cluster.md)
> * [升級叢集的執行階段](service-fabric-tutorial-upgrade-cluster.md)
> * [刪除叢集](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>必要條件

開始進行本教學課程之前：

* 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* 安裝 [Service Fabric SDK 和 PowerShell 模組](service-fabric-get-started.md)
* 安裝 [Azure PowerShell 模組 4.1 版或更新版本](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps)
* 檢閱 [Azure 叢集](service-fabric-azure-clusters-overview.md)的重要概念

下列程序會建立含七個節點的 Service Fabric 叢集。 若要計算在 Azure 中執行 Service Fabric 叢集產生的成本，請使用 [Azure 價格計算機](https://azure.microsoft.com/pricing/calculator/)。

## <a name="download-and-explore-the-template"></a>下載並瀏覽範本

下載下列 Resource Manager 範本檔案：

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

此範本會將一個由七個虛擬機器和三個節點類型組成的安全叢集部署到虛擬網路和網路安全性群組中。  您可以在 [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates) 上找到其他範例範本。  [azuredeploy.json][template] 會部署多項資源，包括下列各項。

### <a name="service-fabric-cluster"></a>Service Fabric 叢集

在 **Microsoft.ServiceFabric/clusters** 資源中，Windows 叢集會以下列特性設定：

* 三個節點類型
* 五個節點屬於主要節點類型 (可在範本參數中設定)，另外兩個節點類型各有一個節點
* 作業系統：Windows Server 2016 Datacenter with Containers (可在範本參數中設定)
* 受保護的憑證 (可在範本參數中設定)
* 啟用[反向 Proxy](service-fabric-reverseproxy.md)
* 啟用 [DNS 服務](service-fabric-dnsservice.md)
* Bronze 的[耐久性層級](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) (可在範本參數中設定)
* Silver 的[可靠性層級](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) (可在範本參數中設定)
* 用戶端連線端點：19000 (可在範本參數中設定)
* HTTP 閘道端點：19080 (可在範本參數中設定)

### <a name="azure-load-balancer"></a>Azure Load Balancer

在 **Microsoft.Network/loadBalancers** 資源中，會為下列連接埠設定負載平衡器，並進行探查和規則的設定：

* 用戶端連線端點：19000
* HTTP 閘道端點：19080
* 應用程式連接埠：80
* 應用程式連接埠：443
* Service Fabric 反向 Proxy：19081

如果需要其他應用程式連接埠，則您必須調整 **Microsoft.Network/loadBalancers** 資源和 **Microsoft.Network/networkSecurityGroups** 資源，以允許流量進入。

### <a name="virtual-network-subnet-and-network-security-group"></a>虛擬網路、子網路和網路安全性群組

虛擬網路、子網路和網路安全性群組的名稱會在範本參數中宣告。  虛擬網路和子網路的位址空間也會在範本參數中宣告，並設定於 **Microsoft.Network/virtualNetworks** 資源中：

* 虛擬網路位址空間：172.16.0.0/20
* Service Fabric 子網路位址空間：172.16.2.0/23

在 **Microsoft.Network/networkSecurityGroups** 資源中會啟用下列輸入流量規則。 您可以藉由變更範本變數來變更連接埠值。

* ClientConnectionEndpoint (TCP)：19000
* HttpGatewayEndpoint (HTTP/TCP)：19080
* SMB：445
* Internodecommunication - 1025、1026、1027
* 暫時連接埠範圍 – 49152 到 65534 (至少需要 256 個連接埠)
* 應用程式使用的連接埠：80 和 443
* 應用程式連接埠範圍 – 49152 到 65534 (用於服務之間的通訊，但不會在負載平衡器上開啟)
* 封鎖所有其他連接埠

如果需要其他應用程式連接埠，則您必須調整 **Microsoft.Network/loadBalancers** 資源和 **Microsoft.Network/networkSecurityGroups** 資源，以允許流量進入。

### <a name="windows-defender"></a>Windows Defender
根據預設，Windows Server 2016 會安裝並執行 [Windows Defender 防毒軟體](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)。 某些 SKU 上會預設安裝使用者介面，但並不需要。  針對在範本中宣告的每個節點類型/VM 擴展集，可以使用 [Azure VM 反惡意程式碼擴充功能](/azure/virtual-machines/extensions/iaas-antimalware-windows)來排除 Service Fabric 目錄和程序：

```json
{
"name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
"properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
        "AntimalwareEnabled": "true",
        "Exclusions": {
                "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
        },
        "RealtimeProtectionEnabled": "true",
        "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
        }
        },
        "protectedSettings": null
}
}
```

## <a name="set-template-parameters"></a>設定範本參數

[azuredeploy.parameters.json][parameters] 參數檔案會宣告多個用來部署叢集和相關聯資源的值。 您可能需要為自己的部署修改某些參數：

|參數|範例值|注意|
|---|---||
|adminUserName|vmadmin| 叢集虛擬機器的管理員使用者名稱。[虛擬機器的使用者名稱需求](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| 叢集 VM 的系統管理員密碼。 [虛擬機器的密碼需求](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| 叢集的名稱。 只能包含字母和數字。 長度可介於 3 到 23 個字元之間。|
|location|southcentralus| 叢集的位置。 |
|certificateThumbprint|| <p>如果建立自我簽署憑證或提供憑證檔案，則值應該空白。</p><p>若要使用先前上傳至金鑰保存庫的現有憑證，請填入憑證 SHA1 指紋值。 例如 "6190390162C988701DB5676EB81083EA608DCCF3"</p>. |
|certificateUrlValue|| <p>如果建立自我簽署憑證或提供憑證檔案，則值應該空白。 </p><p>若要使用先前上傳至金鑰保存庫的現有憑證，請填入憑證 URL。 例如，"https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346"。</p>|
|sourceVaultValue||<p>如果建立自我簽署憑證或提供憑證檔案，則值應該空白。</p><p>若要使用先前上傳至金鑰保存庫的現有憑證，請填入來源保存庫值。 例如 "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT"。</p>|

## <a name="set-up-azure-active-directory-client-authentication"></a>設定 Azure Active Directory 用戶端驗證
在裝載於 Azure 上的公用網路中部署的 Service Fabric 叢集時，我們建議從用戶端到節點的相互驗證應採用：
* 使用 Azure Active Directory 進行用戶端識別
* 以憑證進行伺服器識別，並且對 HTTP 通訊使用 SSL 加密

在[建立叢集](#createvaultandcert)之前，必須先設定 Azure AD 以驗證 Service Fabric 叢集的用戶端。  Azure AD 可讓組織 (稱為租用戶) 管理使用者對應用程式的存取。 

Service Fabric 叢集提供其管理功能的各種進入點 (包括 Web 型 [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) 和 [Visual Studio](service-fabric-manage-application-in-visual-studio.md))。 因此，您將建立兩個 Azure AD 應用程式來控制對叢集的存取：一個 Web 應用程式和一個原生應用程式。  建立應用程式之後，您可以將使用者指派給唯讀和系統管理員角色。

> [!NOTE]
> 建立叢集之前，您必須先完成下列步驟。 由於指令碼會預期叢集名稱和端點，因此這些值應該是計劃的值，而不是您已經建立的值。

在本文中，我們假設您已經建立租用戶。 如果您尚未建立租用戶，請先閱讀[如何取得 Azure Active Directory 租用戶](../active-directory/develop/quickstart-create-new-tenant.md)。

為了簡化與設定 Azure AD 搭配 Service Fabric 叢集相關的一些步驟，我們建立了一組 Windows PowerShell 指令碼。 [將指令碼下載](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool)到您的電腦。

### <a name="create-azure-ad-applications-and-assign-users-to-roles"></a>建立 Azure AD 應用程式，並將使用者指派給角色
建立兩個 Azure AD 應用程式來控制對叢集的存取：一個 Web 應用程式和一個原生應用程式。 建立應用程式來代表您的叢集之後，請將使用者指派給 [Service Fabric 所支援的角色](service-fabric-cluster-security-roles.md)︰唯讀和系統管理員。

執行 `SetupApplications.ps1`，並提供租用戶識別碼、叢集名稱和 Web 應用程式回覆 URL 作為參數。  同時也應指定使用者的使用者名稱和密碼。  例如︰

```PowerShell
$Configobj = .\SetupApplications.ps1 -TenantId '<MyTenantID>' -ClusterName 'mysfcluster123' -WebApplicationReplyUrl 'https://mysfcluster123.eastus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestUser' -Password 'P@ssword!123'
.\SetupUser.ps1 -ConfigObj $Configobj -UserName 'TestAdmin' -Password 'P@ssword!123' -IsAdmin
```

> [!NOTE]
> 對於國家雲 (例如 Azure Government、Azure 中國、Azure 德國)，您也應指定 `-Location` 參數。

您可以在 [Azure 入口網站](https://portal.azure.com)中找到您的*租用戶識別碼*或目錄識別碼。 選取 [Azure Active Directory] -> [屬性]，然後複製 [目錄識別碼] 值。

*ClusterName* 會用來為指令碼所建立的 Azure AD 應用程式加上前置詞。 它不需要與實際叢集名稱完全相符。 其用意只是要讓您更容易將 Azure AD 構件對應到與之搭配使用的 Service Fabric 叢集。

*WebApplicationReplyUrl* 是在您的使用者完成登入之後，Azure AD 傳回給他們的預設端點。 請將此端點設定為您叢集的 Service Fabric Explorer 端點，預設為︰

https://&lt;cluster_domain&gt;:19080/Explorer

系統會提示您登入具有 Azure AD 租用戶系統管理權限的帳戶。 在您登入之後，指令碼會建立 Web 和原生應用程式來代表 Service Fabric 叢集。 如果您在 [Azure 入口網站](https://portal.azure.com)中查看租用戶的應用程式，應該會看到兩個新項目︰

   * *ClusterName*\_叢集
   * *ClusterName*\_用戶端

此指令碼會列印您建立叢集時 Azure Resource Manager 範本所需的 JSON，因此建議讓 PowerShell 視窗保持開啟。

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

### <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>新增 Azure AD 設定以針對用戶端存取使用 Azure AD
在 [azuredeploy.json][template] 的 **Microsoft.ServiceFabric/clusters** 區段中設定 Azure AD。  新增租用戶識別碼、叢集應用程式識別碼和用戶端應用程式識別碼的參數。  

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...

    "aadTenantId": {
      "type": "string",
      "defaultValue": "0e3d2646-78b3-4711-b8be-74a381d9890c"
    },
    "aadClusterApplicationId": {
      "type": "string",
      "defaultValue": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
    },
    "aadClientApplicationId": {
      "type": "string",
      "defaultValue": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
    }
  },

...

{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

在 [azuredeploy.parameters.json][parameters] 參數檔案中新增參數值。  例如︰

```json
"aadTenantId": {
"value": "0e3d2646-78b3-4711-b8be-74a381d9890c"
},
"aadClusterApplicationId": {
"value": "cb147d34-b0b9-4e77-81d6-420fef0c4180"
},
"aadClientApplicationId": {
"value": "7a8f3b37-cc40-45cc-9b8f-57b8919ea461"
}
```

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>部署虛擬網路和叢集

接下來，請設定網路拓撲並部署 Service Fabric 叢集。 [azuredeploy.json][template] Resource Manager 範本會建立虛擬網路 (VNET)，同時也會建立適用於 Service Fabric 的子網路與網路安全性群組 (NSG)。 範本也會部署啟用憑證安全性的叢集。  對於生產叢集，請使用憑證授權單位 (CA) 提供的憑證作為叢集憑證。 自我簽署憑證可用來保護測試叢集。

本文中的範本會部署使用憑證指紋來識別叢集憑證的叢集。  憑證的指紋皆不相同，因而使憑證管理更為困難。 將使用憑證指紋的已部署叢集切換為使用憑證通用名稱，有助於大幅簡化憑證管理作業。  若要了解如何更新叢集以使用憑證通用名稱進行憑證管理，請參閱[將叢集變更為使用憑證通用名稱進行管理](service-fabric-cluster-change-cert-thumbprint-to-cn.md)。

### <a name="create-a-cluster-using-an-existing-certificate"></a>使用現有的憑證建立叢集

下列指令碼會使用 [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) Cmdlet 和範本在 Azure 中部署新的叢集。 此 Cmdlet 也會在 Azure 中建立新的金鑰保存庫，並上傳您的憑證。

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # must match the clusterName parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>使用新的自我簽署憑證建立叢集

下列指令碼會使用 [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) Cmdlet 和範本在 Azure 中部署新的叢集。 此 Cmdlet 也會在 Azure 中建立新的金鑰保存庫、將新的自我簽署憑證新增至金鑰保存庫，並將憑證檔案下載至本機。

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>連線到安全的叢集

使用隨 Service Fabric SDK 一起安裝的 Service Fabric PowerShell 模組連線到叢集。  首先，將憑證安裝到您的電腦上目前使用者的個人 (My) 存放區。  執行下列 PowerShell 命令：

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

您現在即可連線到您安全的叢集。

**Service Fabric** PowerShell 模組提供許多 Cmdlet 來管理 Service Fabric 叢集、應用程式和服務。  使用 [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) Cmdlet 連接到安全的叢集。 在上一個步驟的輸出中找到憑證 SHA1 指紋和連線端點詳細資料。

如果您先前曾設定 AAD 用戶端驗證，請執行下列作業： 
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
        -KeepAliveIntervalInSec 10 `
        -AzureActiveDirectory `
        -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10
```

如果您未曾設定 AAD 用戶端驗證，請執行下列作業：
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

使用 [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) Cmdlet 檢查已連線，而且叢集狀況良好。

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>清除資源

本教學課程系列的其他文章會使用您剛才建立的叢集。 如果您現在不打算繼續閱讀下一篇文章，您可能要[刪除該叢集](service-fabric-cluster-delete.md)以避免產生費用。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 使用 PowerShell 在 Azure 中建立 VNET
> * 建立金鑰保存庫並上傳憑證
> * 設定 Azure Active Directory 驗證
> * 使用 PowerShell 在 Azure 中建立安全的 Service Fabric 叢集
> * 使用 X.509 憑證保護叢集
> * 使用 PowerShell 連線到叢集
> * 刪除叢集

接下來，前進到下列的教學課程，了解如何調整叢集。
> [!div class="nextstepaction"]
> [調整叢集](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json

---
title: 建立具有 HTTP 到 HTTPS 重新導向的應用程式閘道 - Azure PowerShell | Microsoft Docs
description: 了解如何使用 Azure PowerShell，以建立可將流量從 HTTP 重新導向到 HTTPS 的應用程式閘道。
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 7/13/2018
ms.author: victorh
ms.openlocfilehash: a9bff2ab8569a6e070ce5ea4c02b022a172450a3
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2019
ms.locfileid: "57310602"
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-azure-powershell"></a>使用 Azure PowerShell 以建立具有 HTTP 到 HTTPS 重新導向功能的應用程式閘道

您可以使用 Azure PowerShell，以建立包含 SSL 終止憑證的[應用程式閘道](overview.md)。 路由規則可用來將 HTTP 流量重新導向至您應用程式閘道中的 HTTPS 連接埠。 在此範例中，您也會為應用程式閘道的後端集區，建立一個包含兩個虛擬機器執行個體的[虛擬機器擴展集](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)。 

在本文中，您將了解：

> [!div class="checklist"]
> * 建立自我簽署憑證
> * 設定網路
> * 建立包含憑證的應用程式閘道
> * 新增接聽程式和重新導向規則
> * 建立包含預設後端集區的虛擬機器擴展集

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

本教學課程需要 Azure PowerShell 模組版本 1.0.0 或更新版本。 執行 `Get-Module -ListAvailable Az` 以尋找版本。 如果您需要升級，請參閱[安裝 Azure PowerShell 模組](/powershell/azure/install-az-ps)。 若要在本教學課程中執行命令，則也需要執行 `Login-AzAccount` 以建立與 Azure 的連線。

## <a name="create-a-self-signed-certificate"></a>建立自我簽署憑證

若要在生產環境中使用，您應該匯入由受信任提供者所簽署的有效憑證。 在此教學課程中，您要使用 [New-selfsignedcertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。 您可以使用 [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) 搭配系統所傳回的指紋，以將 pfx 檔案從憑證匯出。

```powershell
New-SelfSignedCertificate `
  -certstorelocation cert:\localmachine\my `
  -dnsname www.contoso.com
```

您應該會看到類似這個結果的內容：

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

使用指紋來建立 pfx 檔案：

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate `
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 `
  -FilePath c:\appgwcert.pfx `
  -Password $pwd
```

## <a name="create-a-resource-group"></a>建立資源群組

資源群組是在其中部署與管理 Azure 資源的邏輯容器。 建立名為 Azure 資源群組*myResourceGroupAG*使用[新增 AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)。 

```powershell
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>建立網路資源

建立子網路設定*myBackendSubnet*並*myAGSubnet*使用[新增 AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)。 建立名為虛擬網路*myVNet*使用[新增 AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)與子網路設定。 最後，建立名為的公用 IP 位址*myAGPublicIPAddress*使用[新增 AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)。 這些資源可用來為應用程式閘道及其相關聯的資源提供網路連線。

```powershell
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>建立應用程式閘道

### <a name="create-the-ip-configurations-and-frontend-port"></a>建立 IP 設定與前端連接埠

建立關聯*myAGSubnet*您先前建立的應用程式閘道使用[新增 AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration)。 指派*myAGPublicIPAddress*應用程式閘道使用[新增 AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig)。 然後您可以在其中建立 HTTPS 使用連接埠[新增 AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport)。

```powershell
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendPort = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 443
```

### <a name="create-the-backend-pool-and-settings"></a>建立後端集區和設定

建立名為後端集區*appGatewayBackendPool*使用應用程式閘道[新增 AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool)。 設定後端集區使用的設定[新增 AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsettings)。

```powershell
$defaultPool = New-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>建立預設接聽程式和規則

需要接聽程式才能讓應用程式閘道將流量適當地路由到後端集區。 在此範例中，您會建立基本接聽程式，接聽根 URL 的 HTTPS 流量。 

建立憑證物件，使用[新增 AzApplicationGatewaySslCertificate](/powershell/module/az.network/new-azapplicationgatewaysslcertificate) ，然後建立名為接聽程式*appGatewayHttpListener*使用[新 AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener)前端設定、 前端連接埠，和您先前建立的憑證。 接聽程式需要規則以便知道要針對連入流量使用哪個後端集區。 建立名為的基本規則*rule1*使用[新增 AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule)。

```powershell
$pwd = ConvertTo-SecureString `
  -String "Azure123456!" `
  -Force `
  -AsPlainText
$cert = New-AzApplicationGatewaySslCertificate `
  -Name "appgwcert" `
  -CertificateFile "c:\appgwcert.pfx" `
  -Password $pwd
$defaultListener = New-AzApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -Protocol Https `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendPort `
  -SslCertificate $cert
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>建立應用程式閘道

現在，您會建立必要的支援資源，指定應用程式閘道名為的參數*myAppGateway*使用[新增 AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku)，然後再建立使用[新增 AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway)的憑證。

```powershell
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendPort `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku `
  -SslCertificates $cert
```

## <a name="add-a-listener-and-redirection-rule"></a>新增接聽程式和重新導向規則

### <a name="add-the-http-port"></a>新增 HTTP 連接埠

將 HTTP 連接埠新增至應用程式閘道使用[新增 AzApplicationGatewayFrontendPort](/powershell/module/az.network/add-azapplicationgatewayfrontendport)。

```powershell
$appgw = Get-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG
Add-AzApplicationGatewayFrontendPort `
  -Name httpPort  `
  -Port 80 `
  -ApplicationGateway $appgw
```

### <a name="add-the-http-listener"></a>新增 HTTP 接聽程式

新增名為 HTTP 接聽程式*myListener*應用程式閘道使用[新增 AzApplicationGatewayHttpListener](/powershell/module/az.network/add-azapplicationgatewayhttplistener)。

```powershell
$fipconfig = Get-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -ApplicationGateway $appgw
$fp = Get-AzApplicationGatewayFrontendPort `
  -Name httpPort `
  -ApplicationGateway $appgw
Add-AzApplicationGatewayHttpListener `
  -Name myListener `
  -Protocol Http `
  -FrontendPort $fp `
  -FrontendIPConfiguration $fipconfig `
  -ApplicationGateway $appgw
```

### <a name="add-the-redirection-configuration"></a>新增重新導向設定

新增 HTTP 至 HTTPS 重新導向設定與應用程式閘道使用[新增 AzApplicationGatewayRedirectConfiguration](/powershell/module/az.network/add-azapplicationgatewayredirectconfiguration)。

```powershell
$defaultListener = Get-AzApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -ApplicationGateway $appgw
Add-AzApplicationGatewayRedirectConfiguration -Name httpToHttps `
  -RedirectType Permanent `
  -TargetListener $defaultListener `
  -IncludePath $true `
  -IncludeQueryString $true `
  -ApplicationGateway $appgw
```

### <a name="add-the-routing-rule"></a>新增路由規則

將重新導向設定的路由規則新增至應用程式閘道使用[新增 AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/add-azapplicationgatewayrequestroutingrule)。

```powershell
$myListener = Get-AzApplicationGatewayHttpListener `
  -Name myListener `
  -ApplicationGateway $appgw
$redirectConfig = Get-AzApplicationGatewayRedirectConfiguration `
  -Name httpToHttps `
  -ApplicationGateway $appgw
Add-AzApplicationGatewayRequestRoutingRule `
  -Name rule2 `
  -RuleType Basic `
  -HttpListener $myListener `
  -RedirectConfiguration $redirectConfig `
  -ApplicationGateway $appgw
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-a-virtual-machine-scale-set"></a>建立虛擬機器擴展集

在此範例中，您會建立虛擬機器擴展集，以在應用程式閘道中提供後端集區的伺服器。 當您設定 IP 設定時，要將擴展集指派給後端集區。

```powershell
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw
$ipConfig = New-AzVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id
$vmssConfig = New-AzVmssConfig `
  -Location eastus `
  -SkuCapacity 2 `
  -SkuName Standard_DS2 `
  -UpgradePolicyMode Automatic
Set-AzVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest
  -OsDiskCreateOption FromImage
Set-AzVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword "Azure123456!" `
  -ComputerNamePrefix myvmss
Add-AzVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig
New-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>安裝 IIS

```powershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
$vmss = Get-AzVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss
Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings
Update-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-the-application-gateway"></a>測試應用程式閘道

您可以使用[Get AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress)取得應用程式閘道的公用 IP 位址。 將公用 IP 位址複製並貼到您瀏覽器的網址列。 例如， http://52.170.203.149

```powershell
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![安全警告](./media/redirect-http-to-https-powershell/application-gateway-secure.png)

若要在使用自我簽署憑證時接受安全性警告，請依序按一下 [詳細資料] 與 [繼續瀏覽網頁]。 接著會顯示受保護的 IIS 網站，如下列範例所示：

![在應用程式閘道中測試基底 URL](./media/redirect-http-to-https-powershell/application-gateway-iistest.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立自我簽署憑證
> * 設定網路
> * 建立包含憑證的應用程式閘道
> * 新增接聽程式和重新導向規則
> * 建立包含預設後端集區的虛擬機器擴展集

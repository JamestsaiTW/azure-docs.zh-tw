---
title: 建立具有外部重新導向的應用程式閘道 - Azure PowerShell | Microsoft Docs
description: 了解如何使用 Azure Powershell 建立應用程式閘道，以將 Web 流量重新導向至外部網站。
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: victorh
ms.openlocfilehash: e2c3dbef6204c1b6b75c362ead066b3465a8a5a9
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2019
ms.locfileid: "57307049"
---
# <a name="create-an-application-gateway-with-external-redirection-using-azure-powershell"></a>使用 Azure PowerShell 建立具有外部重新導向的應用程式閘道

您可以使用 Azure PowerShell，在建立[應用程式閘道](application-gateway-introduction.md)時設定 [Web 流量重新導向](application-gateway-multi-site-overview.md)。 在本教學課程中，您會設定接聽程式和規則，以將抵達應用程式閘道的 Web 流量重新導向至外部網站。

在本文中，您將了解：

> [!div class="checklist"]
> * 設定網路
> * 建立接聽程式和重新導向規則
> * 建立應用程式閘道

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

如果您選擇要安裝在本機使用 PowerShell，本教學課程需要 Azure PowerShell 模組版本 1.0.0 或更新版本。 若要尋找版本，請執行 ` Get-Module -ListAvailable Az`。 如果您需要升級，請參閱[安裝 Azure PowerShell 模組](/powershell/azure/install-az-ps)。 如果您在本機執行 PowerShell，則也需要執行 `Connect-AzAccount` 以建立與 Azure 的連線。

## <a name="create-a-resource-group"></a>建立資源群組

資源群組是在其中部署與管理 Azure 資源的邏輯容器。 建立使用您建立 Azure 資源群組[新增 AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)。  

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>建立網路資源

建立子網路組態*myAGSubnet*使用[新增 AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)。 建立名為虛擬網路*myVNet*使用[新增 AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)與子網路組態。 最後，建立 公用 IP 位址 using[新增 AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)。 這些資源可用來為應用程式閘道及其相關聯的資源提供網路連線。

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig
$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>建立應用程式閘道

### <a name="create-the-ip-configurations-and-frontend-port"></a>建立 IP 設定與前端連接埠

建立關聯*myAGSubnet*您先前建立的應用程式閘道使用[新增 AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration)。 將公用 IP 位址指派給應用程式閘道使用[新增 AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig)。 然後您可以建立 HTTP 使用連接埠[新增 AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport)。

```azurepowershell-interactive
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
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool-and-settings"></a>建立後端集區和設定

建立名為後端集區*New-azurermapplicationgatewaybackendaddresspool*使用應用程式閘道[新增 AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool)。 設定集區使用的設定[新增 AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsettings)。

```azurepowershell-interactive
$defaultPool = New-AzApplicationGatewayBackendAddressPool `
  -Name defaultPool 
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-rule"></a>建立接聽程式和規則

需要接聽程式才能讓應用程式閘道適當地路由傳送流量。 建立接聽程式使用[新增 AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener)前端設定和您先前建立的前端連接埠。 您可以藉由建立可新增至您所建立之路由規則的重新導向設定，來將流量重新導向。

接聽程式需要路由規則才能知道要將傳入流量傳送到何處。 建立名為的基本規則*New-azurermapplicationgatewayrequestroutingrule*使用[新增 AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule)重新導向設定。

```azurepowershell-interactive
$defaultListener = New-AzApplicationGatewayHttpListener `
  -Name defaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$redirectConfig = New-AzApplicationGatewayRedirectConfiguration `
  -Name myredirect `
  -RedirectType Temporary `
  -TargetUrl "https://bing.com"
$redirectRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name redirectRule `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -RedirectConfiguration $redirectConfig
```

### <a name="create-the-application-gateway"></a>建立應用程式閘道

現在，您會建立必要的支援資源，指定應用程式閘道名為的參數*myAppGateway*使用[新增 AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku)，然後再建立使用[新 AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway)。

```azurepowershell-interactive
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
  -FrontendPorts $frontendport `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $redirectRule `
  -RedirectConfigurations $redirectConfig `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>測試應用程式閘道

您可以使用[Get AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress)取得應用程式閘道的公用 IP 位址。 將公用 IP 位址複製並貼到您瀏覽器的網址列。

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

您應該會看到瀏覽器中出現 bing.com。

## <a name="next-steps"></a>後續步驟

在本文中，您已了解如何：

> [!div class="checklist"]
> * 設定網路
> * 建立接聽程式和重新導向規則
> * 建立應用程式閘道

> [!div class="nextstepaction"]
> [深入了解應用程式閘道的用途](./application-gateway-introduction.md)

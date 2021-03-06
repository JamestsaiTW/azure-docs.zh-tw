---
title: 設定 Always On 可用性群組的外部接聽程式 | Microsoft Docs
description: 本教學課程將逐步引導您完成建立 Azure 中 Always On 可用性群組接聽程式的步驟，並且可使用相關聯雲端服務的公用虛擬 IP 位址從外部存取。
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 89623adbddce07cbc3c3ead811f5174d108c9b0e
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57444781"
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>設定 Azure 中 Always On 可用性群組的外部接聽程式
> [!div class="op_single_selector"]
> * [內部接聽程式](../classic/ps-sql-int-listener.md)
> * [外部侦听器](../classic/ps-sql-ext-listener.md)
> 
> 

本主題說明如何設定 Always On 可用性群組的接聽程式，並且能夠在網際網路上從外部存取。 如此可將雲端服務的 **公用虛擬 IP (VIP)** 位址與接聽程式建立關聯。

> [!IMPORTANT] 
> Azure 針對建立和使用資源方面，有二種不同的的部署模型：[Resource Manager 和傳統](../../../azure-resource-manager/resource-manager-deployment-model.md)。 本文涵蓋之內容包括使用傳統部署模型。 Microsoft 建議讓大部分的新部署使用 Resource Manager 模式。

您的可用性群組可包含的複本為僅限內部部署、僅限 Azure，或同時跨內部部署和 Azure 的混合式組態。 Azure 複本可位於相同區域內，或使用多個虛擬網路 (VNet) 跨多個區域。 下面的步骤假设已经[配置可用性组](../classic/portal-sql-alwayson-availability-groups.md)，但未配置侦听器。

## <a name="guidelines-and-limitations-for-external-listeners"></a>外部接聽程式的指導方針和限制
當您使用雲端服務公用 VIP 位址部署時，請注意下列有關 Azure 中可用性群組接聽程式的指導方針：

* 可用性群組接聽程式支援 Windows Server 2008 R2、Windows Server 2012 和 Windows Server 2012 R2。
* 用戶端應用程式必須與包含可用性群組 VM 的雲端服務位於不同雲端服務上。 Azure 在相同的雲端服務中不支援伺服器直接回傳搭配用戶端和伺服器使用。
* 根據預設，本文中的步驟示範了如何設定一個接聽程式使用雲端服務的虛擬 IP (VIP) 位址； 不過，您可以為雲端服務保留並建立多個 VIP 位址。 這可讓您利用本文的步驟建立多個個別與不同 VIP 相關聯的接聽程式。 如需有關如何建立多個 VIP 位址的資訊，請參閱[每一雲端服務可有多個 VIP](../../../load-balancer/load-balancer-multivip.md)。
* 如果要为混合环境创建侦听器，则本地网络必须连接到公共 Internet，并通过 Azure 虚拟网络连接到站点到站点 VPN。 位於 Azure 子網路時，僅能透過相應雲端服務的公用 IP 位址才能連線至可用性群組接聽程式。
* 有外部接聽程式也使用內部負載平衡器 (ILB) 時，無法在同一個雲端服務中建立外部接聽程式。

## <a name="determine-the-accessibility-of-the-listener"></a>判斷接聽程式的存取性
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

本文著重於建立使用 **外部負載平衡**的接聽程式。 如果您想要虛擬網路專屬的接聽程式，請參閱本文提供設定 [包含 ILB 之接聽程式](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>使用伺服器直接回傳建立負載平衡 VM 端點
外部負載平衡會使用主控 VM 之雲端服務的虛擬和公用虛擬 IP 位址。 因此在此情況下，您不需要建立或設定負載平衡器。

您必須為每個主控 Azure 複本的 VM 建立負載平衡端點。 如果您的複本位於多個區域，則該區域的每個複本必須位於相同 VNet 中的相同雲端服務。 跨越多个 Azure 区域创建可用性组副本需要配置多个 Vnet。 如需有關設定跨 VNet 連線的詳細資訊，請參閱[設定 VNet 對 VNet 連線](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)。

1. 在 Azure 门户中，导航到托管副本的每个 VM 并查看详细信息。
2. 按一下每個 VM 的 [ **端點** ] 索引標籤。
3. 針對您想要使用的接聽程式端點，確認**名稱**和**公用連接埠**並未處於使用中狀態。 在下列範例中，名稱是「MyEndpoint」，而通訊埠為「1433」。
4. 在您的本機用戶端上，下載並安裝 [最新的 PowerShell 模組](https://azure.microsoft.com/downloads/)。
5. 啟動 **Azure PowerShell**。 新的 PowerShell 工作階段會使用載入的 Azure 系統管理模組來開啟。
6. 執行 **Get-AzurePublishSettingsFile**。 這個 Cmdlet 會將您導向瀏覽器，以便將發佈設定檔案下載至本機目錄。 系統可能會提示您輸入 Azure 訂用帳戶的登入認證。
7. 使用您所下載發佈設定檔案的路徑來執行 **Import-AzurePublishSettingsFile** 命令：
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    一旦匯入發佈設定檔案，您便可以在 PowerShell 工作階段中管理 Azure 訂用帳戶。
    
1. 将以下 PowerShell 脚本复制到文本编辑器中，并根据你的环境设置变量值（这里为某些参数提供了默认值）。 請注意，如果您的可用性群組跨越 Azure 區域，您必須針對雲端服務和位於該資料中心的節點，在每個資料中心執行一次指令碼。
   
        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. 一旦您已設定變數，請從文字編輯器將指令碼複製到您的 Azure PowerShell 工作階段來執行它。 如果提示符仍然显示 >>，请再次按 Enter，以确保脚本开始运行。

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>必要時，請確認已安裝 KB2854082
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>在可用性群組節點中開啟防火牆連接埠
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>创建可用性组侦听器

透過兩個步驟建立可用性群組接聽程式。 首先，建立用戶端存取點叢集資源，並設定相依性。 接下來，使用 PowerShell 設定叢集資源。

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>创建客户端接入点并配置群集依赖关系
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>在 PowerShell 中設定叢集資源
1. 对于外部负载均衡，你必须获取包含副本的云服务的公共虚拟 IP 地址。 登入 Azure 入口網站。 巡覽至包含可用性群組 VM 的雲端服務。 開啟 **儀表板** 檢視。
2. 記下 [ **公用虛擬 IP (VIP) 位址**] 下方顯示的位址。 如果您的解決方案跨越多個 VNet，請針對包含主控複本之 VM 的每個雲端服務重複此步驟。
3. 在其中一個 VM 上，將下方的 PowerShell 指令碼複製到文字編輯器，並將變數設定為之前記下的值。
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. 設定變數之後，開啟提升權限的 Windows PowerShell 視窗中，然後從文字編輯器將指令碼複製並貼到您的 Azure PowerShell 工作階段來執行它。 如果提示依然顯示「>>」，請再次按 ENTER 鍵以確定指令碼開始執行。
5. 在每個 VM 上重複此步驟。 此指令碼會使用雲端服務的 IP 位址來設定 IP 位址資源，並設定類似探查連接埠的其他參數。 當 IP 位址資源處於線上時，它會從本教學課程中稍早所建立的負載平衡端點，接著回應探查連接埠上的輪詢。

## <a name="bring-the-listener-online"></a>使接聽程式上線
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>待處理項目
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>测试可用性组侦听器（在同一 VNet 中）
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>測試可用性群組接聽程式 (位於網際網路)
若要存取從虛擬網路外部的接聽程式，您必須使用外部/公用負載平衡 （如本主題所述） 而不 ILB，這只在相同的 VNet 內存取。 在連接字串中，您將指定雲端服務名稱。 例如，如果您雲端服務的名稱為 *mycloudservice*，sqlcmd 陳述式便如下所示：

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

不同於先前範例，必須使用 SQL 驗證，因為呼叫端無法透過網際網路使用 Windows 驗證。 如需詳細資訊，請參閱[Always On 可用性群組在 Azure VM 中：用戶端連線案例](https://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx)。 使用 SQL 驗證時，請確定您在兩個複本上建立相同的登入。 如需有關使用可用性群組疑難排解登入的詳細資訊，請參閱 [如何對應登入或使用包含的 SQL Database 使用者以連線到其他複本並對應到可用性資料庫](https://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx)。

如果 Always On 複本位於其他子網路中，用戶端必須在連接字串中指定 **MultisubnetFailover=True** 。 這會導致對於不同子網路中的複本進行平行連接嘗試。 請注意，此情況包含跨區域的 Always On 可用性群組部署。

## <a name="next-steps"></a>後續步驟
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]


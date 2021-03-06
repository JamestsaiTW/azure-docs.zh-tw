---
title: 使用 Azure PowerShell 佈建 SQL Server VM 的指南 | Microsoft Docs
description: 提供使用 SQL Server 虛擬機器資源庫映像建立 Azure VM 的步驟和 PowerShell 命令。
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/21/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 3feb691f1f708452b6560dbe92b77ed0417ffb82
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329399"
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>如何使用 Azure PowerShell 佈建 SQL Server 虛擬機器

本指南說明使用 Azure PowerShell 建立 Windows SQL Server VM 的選項。 如需包含更多預設值的簡化版 Azure PowerShell 範例，請參閱 [SQL VM Azure PowerShell 快速入門](quickstart-sql-vm-create-powershell.md)。

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

## <a name="configure-your-subscription"></a>設定您的訂用帳戶

1. 開啟 PowerShell，然後執行 **Connect-AzAccount** 命令來建立存取您 Azure 帳戶的權限。

   ```PowerShell
   Connect-AzAccount
   ```

1. 您應該會看到輸入認證的畫面。 請使用與登入 Azure 入口網站相同的電子郵件和密碼。

## <a name="define-image-variables"></a>定義映像變數
若想要重複使用值並簡化建立指令碼的作業，請先定義數個變數。 視需要變更參數值，但在修改所提供的值時，請注意與名稱長度和特殊字元相關的命名限制。

### <a name="location-and-resource-group"></a>位置和資源群組
將資料區域和資源群組定義為會在其中建立其他 VM 資源。

視需要進行修改，然後執行這些 Cmdlet 來初始化這些變數。

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>儲存體屬性
定義儲存體帳戶和虛擬機器所要使用的儲存體類型。

視需要進行修改，然後執行下列 Cmdlet 來初始化這些變數。 我們建議針為生產環境工作負載使用[進階 SSD](../disks-types.md#premium-ssd)。

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>網路屬性
定義要由虛擬機器中的網路使用的屬性。 

- Linux
- TCP/IP 配置方法
- 虛擬網路名稱
- 虛擬子網路名稱
- 虛擬網路的 IP 位址範圍
- 子網路的 IP 位址範圍
- 公用網域名稱標籤

視需要進行修改，然後執行此 Cmdlet 來初始化這些變數。

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = $ResourceGroupName
```

### <a name="virtual-machine-properties"></a>虛擬機器屬性
定義虛擬機器名稱、電腦名稱、虛擬機器大小，以及虛擬機器的作業系統磁碟名稱。

視需要進行修改，然後執行此 Cmdlet 來初始化這些變數。

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>選擇 SQL Server 映像

使用下列變數來定義用於虛擬機器的 SQL Server 映像。 

1. 首先，請使用 `Get-AzVMImageOffer` 命令，列出 SQL Server 映像供應項目的完整清單。 此命令會列出 Azure 入口網站上可用的最新映像清單，以及只能搭配 PowerShell 安裝的較舊映像：

   ```PowerShell
   Get-AzVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. 在本教學課程中，會使用下列變數，以在 Windows Server 2016 上指定 SQL Server 2017。

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. 接著列出您供應項目的可用版本。

   ```PowerShell
   Get-AzVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. 在本教學課程中，使用的是 SQL Server 2017 Developer 版本 (**SQLDEV**)。 Developer 版本免費授權用於測試及開發，而且只需要支付執行 VM 的費用。

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>建立資源群組
在 Resource Manager 部署模型中，您所建立的第一個物件是資源群組。 使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) \(英文\) Cmdlet，來建立 Azure 資源群組及其資源。 指定您先前針對資源群組名稱和位置所初始化的變數。

執行此 Cmdlet 來建立新的資源群組。

```PowerShell
New-AzResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>建立儲存體帳戶
虛擬機器需要作業系統磁碟和 SQL Server 資料和記錄檔的儲存體資源。 為了簡單起見，您會針對兩者建立單一磁碟。 您可以在之後使用 [Add-Azure Disk](https://docs.microsoft.com/powershell/module/servicemanagement/azure/add-azuredisk) Cmdlet 來附加額外的磁碟，以將您的 SQL Server 資料和記錄檔放在專用的磁碟上。 使用 [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) \(英文\) Cmdlet，在新的資源群組中建立標準儲存體帳戶。 指定您先前針對儲存體帳戶名稱、儲存體 Sku 名稱及位置所初始化的變數。

執行此 Cmdlet 來建立新的儲存體帳戶。

```PowerShell
$StorageAccount = New-AzStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> 建立儲存體帳戶可能需要幾分鐘的時間。

## <a name="create-network-resources"></a>建立網路資源
虛擬機器需要數個網路資源才能連接網路。

* 每部虛擬機器都需要虛擬網路。
* 虛擬網路必須定義至少一個子網路。
* 網路介面必須以公用或私人 IP 位址定義。

### <a name="create-a-virtual-network-subnet-configuration"></a>建立虛擬網路子網路組態
首先要建立虛擬網路的子網路設定。 針對本教學課程，使用 [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) \(英文\) Cmdlet 來建立預設子網路。 指定您先前針對子網路名稱和位址前置詞所初始化的變數。

> [!NOTE]
> 您可以使用這個 Cmdlet 來定義虛擬網路子網路組態的其他屬性，但這已超出本教學課程的範圍。

執行此 Cmdlet 來建立虛擬子網路設定。

```PowerShell
$SubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>建立虛擬網路
接著，使用 [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) \(英文\) Cmdlet，在新的資源群組中建立虛擬網路。 指定您先前針對名稱、位置及位址前置詞所初始化的變數。 使用您在上一個步驟中所定義的子網路設定。

執行此 Cmdlet 來建立虛擬網路。

```PowerShell
$VNet = New-AzVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>建立公用 IP 位址
您已定義虛擬網路，現在必須設定 IP 位址以連線至該虛擬機器。 針對本教學課程，請使用動態 IP 位址來建立公用 IP 位址，以支援網際網路連線。 使用 [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) \(英文\) Cmdlet，在新的資源群組中建立公用 IP 位址。 指定您先前針對名稱、位置、配置方法及 DNS 網域名稱標籤所初始化的變數。

> [!NOTE]
> 您可以使用這個 Cmdlet 來定義公用 IP 位址的其他屬性，但這已超出本初期教學課程的範圍。 您也可以建立私人位址或具有靜態位址的位址，但這也已超出本教學課程的範圍。

執行此 Cmdlet 來公用 IP 位址。

```PowerShell
$PublicIp = New-AzPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>建立網路安全性群組
若要保護 VM 和 SQL Server 的流量，請建立網路安全性群組。

1. 首先，建立 RDP 的網路安全性群組規則，以允許遠端桌面連線。

   ```PowerShell
   $NsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. 設定網路安全性群組規則，允許 TCP 通訊埠 1433 上的流量。 如此即可透過網際網路連線至 SQL Server。

   ```PowerShell
   $NsgRuleSQL = New-AzNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. 建立網路安全性群組。

   ```PowerShell
   $Nsg = New-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>建立網路介面
您現在已準備好建立適用於虛擬機器將的網路介面。 使用 [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) \(英文\) Cmdlet，在新的資源群組中建立網路介面。 指定先前所定義的名稱、位置、子網路及公用 IP 位址。

執行此 Cmdlet 來建立網路介面。

```PowerShell
$Interface = New-AzNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>設定 VM 物件
您已定義儲存體和網路資源，現在已準備好定義虛擬機器的計算資源。

- 指定虛擬網路大小和各種作業系統屬性。
- 指定您先前所建立的網路介面。
- 定義 Blob 儲存體。
- 指定作業系統磁碟。

### <a name="create-the-vm-object"></a>建立 VM 物件
首先要指定虛擬機器的大小。 針對此教學課程，請指定 [DS13]。 使用 [New-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) \(英文\) Cmdlet，來建立可設定的虛擬網路物件。 指定您先前針對名稱和大小所初始化的變數。

執行此 Cmdlet 來建立虛擬機器物件。

```PowerShell
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>建立認證物件，以保留本機系統管理員認證的名稱和密碼
在您可以設定虛擬機器的作業系統屬性之前，您必須先以安全字串的形式提供本機系統管理員帳戶的認證。 為了達成此目的，要使用 [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) Cmdlet。

執行下列 Cmdlet，然後在 PowerShell 認證要求視窗中，輸入要用於虛擬機器中本機系統管理員帳戶的名稱和密碼。

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>設定虛擬機器的作業系統屬性
您現在已準備好使用 [Set-AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) \(英文\) Cmdlet，來設定虛擬機器的作業系統屬性。

- 將作業系統的類型設定為 [Windows]。
- 需要安裝[虛擬機器代理程式](../../extensions/agent-windows.md)。
- 指定該 Cmdlet 會啟用自動更新。
- 指定您先前針對虛擬機器名稱、電腦名稱及認證所初始化的變數。

執行此 Cmdlet 來設定虛擬機器的作業系統屬性。

```PowerShell
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>將網路介面新增至虛擬機器
接下來，使用 [Add-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) \(英文\) Cmdlet，以您稍早定義的變數來新增網路介面。

執行此 Cmdlet 來設定虛擬機器的網路介面。

```PowerShell
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>設定虛擬機器所要使用之磁碟的 Blob 儲存體位置
接下來，使用您稍早所定義的變數設定 VM 磁碟的 Blob 儲存體位置。

執行此 Cmdlet 來設定 Blob 儲存體位置。

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>設定虛擬機器的作業系統磁碟屬性
接著，使用 [Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) \(英文\) Cmdlet，來設定虛擬機器的作業系統磁碟屬性。 

- 指定虛擬機器的作業系統將會來自映像。
- 將快取設定成唯讀 (因為 SQL Server 會安裝到相同的磁碟上)。
- 指定您先前針對 VM 名稱和作業系統磁碟所初始化的變數。

執行此 Cmdlet 來設定虛擬機器的作業系統磁碟屬性。

```PowerShell
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>指定虛擬機器的平台映像
最後一個設定步驟是指定虛擬機器的平台映像。 針對本教學課程，請使用最新的 SQL Server 2016 CTP 映像。 使用 [Set-AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) \(英文\) Cmdlet，搭配您稍早定義的變數來使用此映像。

執行此 Cmdlet 來指定虛擬機器的平台映像。

```PowerShell
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>建立 SQL VM
您已完成設定步驟，現在已準備好建立虛擬機器。 使用 [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) \(英文\) Cmdlet，以您定義的變數來建立虛擬機器。

> [!TIP]
> 建立 VM 可能需要幾分鐘的時間。

執行此 Cmdlet 來建立虛擬機器。

```PowerShell
New-AzVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

虛擬機器已建立。

> [!NOTE]
> 如果您收到有關開機診斷的錯誤，則可以忽略它。 由於針對虛擬機器磁碟所指定的儲存體帳戶是進階儲存體帳戶，因此系統會針對開機診斷建立標準儲存體帳戶。

## <a name="install-the-sql-iaas-agent"></a>安裝 SQL IaaS 代理程式
SQL Server 虛擬機器能以 [SQL Server IaaS 代理程式延伸模組](virtual-machines-windows-sql-server-agent-extension.md)支援自動化管理功能。 若要在新的 VM 上安裝代理程式，請在其建立之後執行下列命令。


   ```PowerShell
   Set-AzVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="stop-or-remove-a-vm"></a>停止或移除 VM

如果您不需要持續執行 VM，您可以在不使用時將其停止，以避免不必要的費用。 下列命令會停止 VM，但會將其保留供未來使用。

```PowerShell
Stop-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

您也可以使用 **Remove-AzResourceGroup** 命令，將與虛擬機器相關聯的所有資源永久刪除。 這麼做也會永久刪除虛擬機器，因此請小心使用此命令。

## <a name="example-script"></a>範例指令碼
下列指令碼包含本教學課程的完整 PowerShell 指令碼。 我們假設您已經將 Azure 訂用帳戶設定為要與 **Connect-AzAccount** 和 **Select-AzSubscription** 命令搭配使用。

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = $ResourceGroupName

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2017-WS2016"
$Sku = "SQLDEV"
$Version = "latest"

# Resource Group
New-AzResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Nsg = New-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $Location -Name $NsgName -SecurityRules $NsgRuleRDP,$NsgRuleSQL
$Interface = New-AzNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension
Set-AzVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
```

## <a name="next-steps"></a>後續步驟
建立虛擬機器之後，您可以：

- 使用 RDP 連線到虛擬機器
- 在入口網站中為您的虛擬機器設定 SQL Server，包括：
   - [儲存體設定](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [自動化的管理工作](virtual-machines-windows-sql-server-agent-extension.md)
- [設定連線能力](virtual-machines-windows-sql-connect.md)
- 將用戶端與應用程式連線至新的 SQL Server 執行個體


---
title: Microsoft Azure 備份伺服器和 System Center DPM 的支援矩陣
description: 本文摘要說明您使用 Microsoft Azure 備份伺服器或 System Center DPM 來備份內部部署和 Azure VM 資源時的 Azure 備份支援。
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 704bb409d2b21e2ae258dbb2d627b1c088d80db7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57860919"
---
# <a name="support-matrix-for-backup-with-microsoft-azure-backup-server-or-system-center-dpm"></a>使用 Microsoft Azure 備份伺服器或 System Center DPM 備份的支援矩陣

您可以使用[Azure 備份服務](backup-overview.md)備份內部部署機器和工作負載和 Azure 虛擬機器 (Vm)。 本文摘要說明支援設定和備份機器，藉由使用 Microsoft Azure 備份伺服器 (MABS) 或 System Center Data Protection Manager (DPM)，以及 Azure 備份的限制。

## <a name="about-dpmmabs"></a>關於 DPM/MABS

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview?view=sc-dpm-1807)是企業解決方案，來設定，可加速，及管理企業機器和資料的備份和復原。 它是 [System Center](https://www.microsoft.com/cloud-platform/system-center-pricing) 產品套件的一部分。

MABS 會是可用來備份內部部署實體伺服器、 Vm 和其上執行的應用程式的伺服器產品。

MABS 會根據 System Center DPM，並提供類似的功能，有幾個差異：
- MABS 不需要 System Center 授權即可執行。
- MABS 和 DPM 中，Azure 會提供長期的備份儲存體。 此外，DPM 可讓您將資料備份在磁帶上進行長期儲存。 MABS 未提供這項功能。

下載從 MABS [Microsoft 下載中心](https://www.microsoft.com/en-us/download/details.aspx?id=57520)。 它可以執行內部部署或 Azure VM 上。

DPM 和 MABS 皆提供備份多種不同應用程式和伺服器與用戶端作業系統的支援。 它們適用於多種備份案例：

- 您可以使用系統狀態或裸機備份在機器層級進行備份。
- 您可以備份特定磁碟區、共用、資料夾和檔案。
- 您可以使用最佳化感知應用程式的設定，以備份特定的應用程式。

## <a name="dpmmabs-backup"></a>DPM/MABS 備份

使用 DPM/MABS 和 Azure 備份備份運作方式如下：

1. 將備份的每部機器上安裝 DPM/MABS 保護代理程式。
1. 機器和應用程式會備份到本機儲存體 DPM/MABS 上。
1. 在 DPM 伺服器/MABS 上安裝 Microsoft Azure 復原服務 (MARS) 代理程式。
1. MARS 代理程式備份的 DPM/MABS 磁碟備份的復原服務保存庫，在 Azure 中使用 Azure 備份。

其他資訊：

- [深入了解](backup-architecture.md#architecture-back-up-to-dpmmabs) MABS 架構。
- [檢閱支援的項目](backup-support-matrix-mars-agent.md)MARS 代理程式。

## <a name="supported-scenarios"></a>支援的案例 

**案例** | **代理程式** | **位置**
--- | --- | ---
**備份內部部署機器/工作負載** | 您想要備份的機器上，執行 DPM/MABS 保護代理程式。<br/><br/> DPM/MABS 伺服器上的 MARS 代理程式 」。 | DPM/MABS 必須執行在內部部署環境。
**Azure VM/工作負載的備份** | 受保護的電腦上的 DPM/MABS 保護代理程式。<br/><br/> DPM/MABS 伺服器上的 MARS 代理程式 」。 | DPM/MABS 必須在 Azure VM 上執行。

## <a name="supported-deployments"></a>支援的部署

您可以部署 DPM/MABS 下, 表摘要說明。

**部署** | **支援** | **詳細資料**
--- | --- | ---
**部署於內部部署** | 實體伺服器<br/><br/>Hyper-V VM<br/><br/> VMware VM | 如果 DPM/MABS 安裝做為 VMware VM，它只會備份 VMware Vm 和那些 Vm 執行的工作負載。
**部署為 Azure Stack VM** | 僅限 MABS | DPM 無法用來備份 Azure Stack VM。
**部署為 Azure VM** | 保護 Azure Vm，而這些 Vm 執行的工作負載 | 在 Azure 中執行的 DPM/MABS 無法備份內部部署機器。


## <a name="supported-mabs-and-dpm-operating-systems"></a>支援的 MABS 和 DPM 作業系統

Azure 備份可以備份 DPM/MABS 會執行下列作業系統的執行個體。 作業系統應執行最新的服務套件和更新。

**案例** | **DPM/MABS** 
--- | --- 
**Azure VM 上的 MABS** |  Windows Server 2012 R2。<br/><br/> Windows 2016 資料中心。<br/><br/> Windows 2019 資料中心。<br/><br/> 我們建議您先從 marketplace 映像。<br/><br/> 最小 A2 標準，具備雙核心及 3.5 GB 的 RAM。 
**Azure VM 上的 DPM** | System Center 2012 R2 (含 Update 3 或更新版本)。<br/><br/> 符合 [System Center 需求](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server)的 Windows 作業系統。<br/><br/> 我們建議您先從 marketplace 映像。<br/><br/> 最小 A2 標準，具備雙核心及 3.5 GB 的 RAM。 
**內部部署 MABS** | 支援的 64 位元作業系統：<br/><br/> MABS v3 和更新版本：Windows Server 2019 (Standard、 Datacenter、 Essentials)。 <br/><br/> MABS v2 和更新版本：Windows Server 2016 (Standard、 Datacenter、 Essentials)。<br/><br/> 所有 MABS 版本：Windows Server 2012 R2 中，Windows Server 2012 (Standard、 Datacenter、 Foundation)。<br/><br/>所有 MABS 版本：Windows Storage Server 2012 R2，Windows Server 2012 （Standard、 Workgroup）。
**內部部署 DPM** | 實體伺服器/Hyper-V VM：System Center 2012 SP1 或更新版本。<br/><br/> VMware VM：System Center 2012 R2 (含 Update 5 或更新版本)。 



## <a name="management-support"></a>管理支援

**問題** | **詳細資料**
--- | ---
**安裝** | 在單一用途的電腦上安裝 DPM/MABS。<br/><br/> 請勿安裝 DPM/MABS，網域控制站上，安裝應用程式伺服器角色的電腦上，執行 Microsoft Exchange Server 或 System Center Operations Manager 的電腦上或叢集節點上。<br/><br/> [檢閱所有的 DPM 系統需求](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server)。
**網域** | DPM/MABS 應該已加入網域。 請先安裝 DPM/MABS，再將其加入網域。 不支援在部署之後將 DPM/MABS 移至新網域。
**儲存體** | 從 DPM 2016/MABS v2 和更新版本支援新式備份儲存體 (MB)。 MBS 不適用於 MABS v1。
**MABS 升級** | 您可以直接安裝 MABS v3，或是從 MABS v2 升級至 MABS v3。 [深入了解](backup-azure-microsoft-azure-backup.md#upgrade-mabs)。
**移動 MABS** | 使用 MABS 時，支援將 MABS 移至新伺服器，同時保留儲存體。<br/><br/> 伺服器必須與原始伺服器同名。 如果您想要保留相同的儲存體集區，並使用相同的 MABS 資料庫來儲存資料復原點，則不可變更名稱。<br/><br/> 您將需要 MABS 資料庫的備份，因為您將必須加以還原。


## <a name="mabs-support-on-azure-stack"></a>Azure Stack 上的 MABS 支援

您可以將 MABS 部署在 Azure Stack VM 上，以便從單一位置管理 Azure Stack VM 和工作負載的備份。

**元件** | **詳細資料**
--- | --- 
**Azure Stack VM 上的 MABS** | 在至少大小 A2。 我們建議您從 Azure Marketplace 的 Windows Server 2012 R2 或 Windows Server 2016 映像開始。<br/><br/> 請勿在 MABS VM 上安裝任何其他項目。
**MABS 儲存體** | MABS vm 使用個別的儲存體帳戶。 在 MABS 上執行的 MARS 代理程式需要暫存儲存體，為快取位置，並保存從雲端還原的資料。
**MABS 儲存體集區** | MABS 存放集區的大小取決於連接到 MABS VM 的磁碟大小與數量。 每個 Azure Stack VM 大小皆有磁碟數目上限。 例如，A2 是四個磁碟。
**MABS 保留期** | 不會超過五天保留在本機的 MABS 磁碟上的備份的資料。
**MABS 相應增加** | 若要相應增加您的部署，您可以增加 MABS VM 的大小。 比方說，您可以變更從 A 至 D 系列。<br/><br/> 您也可以確保，您要卸載的資料至 Azure 的備份定期。 如有必要，您可以部署額外的 MABS 伺服器。
**在 MABS 上的.NET framework** | MABS VM 需要.NET Framework 3.3 SP1 或更新版本安裝在其上。
**MABS 網域** | MABS VM 必須加入網域。 具有管理員權限的網域使用者必須在 VM 上安裝 MABS。
**Azure Stack VM 資料備份** | 您可以備份檔案、資料夾和應用程式。
**支援的備份** | 這些作業系統可支援您想要備份的 Vm:<br/><br/> Windows Server 半年通道 (Datacenter、 Enterprise、 Standard)<br/><br/> Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012、 Windows Server 2008 R2
**Azure Stack Vm 的 SQL Server 支援** | 備份 SQL Server 2016，SQL Server 2014，SQL Server 2012 SP1。<br/><br/> 備份和復原資料庫。
**Azure Stack VM 的 SharePoint 支援** | SharePoint 2016，SharePoint 2013 中，SharePoint 2010。<br/><br/> 備份和復原伺服器陣列、 資料庫、 前端及 web 伺服器。
**已備份 VM 的網路需求** | Azure Stack 的工作負載中的所有 Vm 必須屬於相同的虛擬網路，並屬於相同的訂用帳戶。

## <a name="dpmmabs-networking-support"></a>DPM/MABS 網路支援

### <a name="url-access"></a>URL 存取

DPM 伺服器/MABS 需要存取下列 URL：

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

### <a name="dpmmabs-connectivity-to-azure-backup"></a>DPM/MABS 與 Azure 備份的連線

必須連線至 Azure 備份服務才能正常執行備份，且 Azure 訂用帳戶必須作用中。 下表說明不符合這兩個條件時的行為。

**MABS 對 Azure** | **訂用帳戶** | **備份/還原** 
--- | --- | --- 
連線 | Active | 備份至 DPM/MABS 磁碟。<br/><br/> 備份至 Azure。<br/><br/> 從磁碟還原。<br/><br/> 從 Azure 還原。
連線 | 已過期/取消佈建 | 不會備份至磁碟或 Azure。<br/><br/> 如果訂用帳戶已過期，您可以從磁碟或 Azure 中還原。<br/><br/> 如果訂用帳戶已解除任務，您無法從磁碟或 Azure 中還原。 Azure 復原點已刪除。
未連線超過 15 天 | Active | 不會備份至磁碟或 Azure。<br/><br/> 您可以從磁碟或 Azure 還原。
未連線超過 15 天 | 已過期/取消佈建 | 不會備份至磁碟或 Azure。<br/><br/> 如果訂用帳戶已過期，您可以從磁碟或 Azure 中還原。<br/><br/> 如果訂用帳戶已解除任務，您無法從磁碟或 Azure 中還原。 Azure 復原點已刪除。

## <a name="dpmmabs-storage-support"></a>DPM/MABS 儲存體支援

備份至 DPM/MABS 的資料會儲存在本機的磁碟儲存體。 

**儲存體** | **詳細資料**
--- | ---
**MBS** | 從 DPM 2016/MABS v2 和更新版本支援新式備份儲存體 (MB)。 MBS 不適用於 MABS v1。
**Azure VM 上的 MABS 儲存體** | 資料會儲存在 Azure 磁碟的已連結至 DPM/MABS VM，且管理 DPM/MABS 中。 用於 DPM/MABS 存放集區的磁碟數目會受到 VM 的大小。<br/><br/> A2 VM：4 個磁碟；A3 VM：8 個磁碟；A4 VM：16 個磁碟，每個磁碟的大小上限為 1 TB。 這會決定可用的備份儲存體總計集區。<br/><br/> 您可以備份的資料量取決於連結的磁碟數目和大小。
**Azure VM 上的 MABS 資料保留** | 我們建議您保留 DPM/MABS Azure 磁碟上的一天的資料，並從 DPM/MABS 備份到保存庫來延長保留時間。 藉此，您就可以藉由將資料卸載至 Azure 備份來保護較大量的資料。


### <a name="modern-backup-storage-mbs"></a>新式備份儲存體 (MBS)
從 DPM 2016/MABS v2 （Windows Server 2016 上執行） 和更新版本中，您可以利用新式備份儲存體 (MB)。

- MBS 備份會儲存在復原檔案系統 (ReFS) 磁碟上。
- MBS 使用 ReFS 區塊複製備份更快速和更有效率地使用儲存空間。
- 當您新增到本機 DPM/MABS 存放集區的磁碟區時，您會設定它們使用磁碟機代號。 隨後，您可以在不同的磁碟區上設定工作負載儲存體。
- 當您建立用來將資料備份至 DPM/MABS 的保護群組時，您可以選取所要使用的磁碟機。 比方說，您可能會儲存備份的 SQL 或其他高 IOPS 工作負載的高效能磁碟機，並儲存備份的頻率較低的效能磁碟機上的工作負載。


## <a name="supported-backups-to-mabs"></a>MABS 支援的備份

下表摘要說明可從內部部署機器和 Azure VM 備份至 MABS 的項目。


**備份** | **版本** | **MABS** | **詳細資料** |
--- | --- | --- | --- |
**Windows 10<br/>Windows 8.1<br/>Windows 8<br/>Windows 7**<br/><br/>(32/64 位元) | MABS v3、v2、V1 | 內部部署。 | 磁碟區/共用/資料夾/檔案。<br/><br/> 支援已刪除重複資料的磁碟區。<br/><br/> 磁碟區至少必須是 1 GB 和 NTFS。 |
**Windows Server 2016 (Datacenter、Standard、非 Nano)**<br/><br/> 64/32 位元 | MABS v3、v2 | 內部部署/Azure VM。| 磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> 支援已刪除重複資料的磁碟區。 |
**Windows Server 2012 R2 (Datacenter 和 Standard)**<br/><br/> 64/32 位元 | MABS v3、v2、v1 | 內部部署/Azure VM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VMprotection**：磁碟區/共用/資料夾/檔案。<br/><br/> 支援已刪除重複資料的磁碟區。 |
**Windows Server 2012 SP1 (Datacenter 和 Standard)**<br/><br/> 64/32 位元 | MABS v3、v2、v1<br/><br/> 必須安裝 [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855)。 | 內部部署/Azure VM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。<br/><br/> 支援已刪除重複資料的磁碟區。 |
**Windows Server 2012 (Datacenter 和 Standard)**<br/><br/> 64/32 位元 | MABS v1 | 內部部署/Azure VM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。<br/><br/> 支援已刪除重複資料的磁碟區。 |
**Windows 2008 R2 SP1 (Standard 和 Enterprise)**<br/><br/> 64/32 位元 | 受 MABS v3、v2、v1 支援。<br/><br/> 必須安裝 [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855)。 | 內部部署/Azure VM。 |   **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。<br/><br/> 支援已刪除重複資料的磁碟區。 |
**Windows 2008 R2 (Standard 和 Enterprise)**<br/><br/> 64/32 位元 | MABS v1。 針對 MABS v2/v3，OS 必須執行 SP1。 | 內部部署/Azure VM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。<br/><br/> 支援已刪除重複資料的磁碟區。 |
**Windows Server 2008 SP2**<br/><br/> 64/32 位元 | MABS v1、v2、v3 | 將 MABS 部署為內部部署實體機器或 Hyper-V VM 時，僅支援 v1。<br/><br/> 將 MABS 部署為 VMware VM 時，支援 MABS v1、2、3。<br/><br/> 不支援在 Azure VM 上執行的 MABS。 | 磁碟區/共用/資料夾/檔案；系統狀態/裸機。 |
**Windows Storage Server 2008** | MABS v1、v2、v3 | MABS 如在內部部署實體伺服器/HYPER-V VM。 <br/><br/> 不支援在 Azure VM 上執行的 MABS。 | 磁碟區/共用/資料夾/檔案；系統狀態/裸機。
**SQL Server 2017** | MABS v3 | 內部部署/Azure VM。| 備份 SQL Server 資料庫。<br/><br/> 不支援 SQL Server 叢集備份。<br/><br/>不支援儲存在 CSV 上的資料庫。 |
**SQL Server 2016/2016 SP1** | MABS v3、v2 | 內部部署/Azure VM。| 備份 SQL Server 資料庫。<br/><br/> 不支援 SQL Server 叢集備份。<br/><br/>不支援儲存在 CSV 上的資料庫。 |
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | MABS v3、v2、v1 | 內部部署/Azure VM。| 備份 SQL Server 資料庫。<br/><br/> 不支援 SQL Server 叢集備份。<br/><br/>不支援儲存在 CSV 上的資料庫。 |
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | MABS v3、v2、V1 | 內部部署。 | 備份獨立 Exchange 伺服器、 DAG 下的資料庫。<br/><br/> 復原信箱、DAG 下的信箱資料庫。<br/><br/> 不支援 ReFS。<br/><br/> 備份非共用磁碟叢集。<br/><br/> 備份已設定連續複寫的 Exchange Server。 |
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | MABS v3、v2、v1 | 內部部署/Azure VM。 | 備份伺服器陣列、 前端網頁伺服器。<br/><br/> 復原伺服器陣列、 資料庫、 web 應用程式、 檔案或清單項目、 SharePoint 搜尋、 前端網頁伺服器。<br/><br/> 您無法將備份的內容資料庫使用 SQL Server AlwaysOn 的伺服器陣列。 |
**Windows Server 2016 上的 Hyper-V**<br/><br/> **Windows Server 2012 R2/2012** (Datacenter/Standard)<br/><br/> **Windows Server 2008 R2 (含 SP1)** | MABS v3、v2、v1 | 內部部署。 | **Hyper-V 主機上的 MABS 代理程式**：備份整個 VM 和主機資料檔案。 備份使用本機儲存體的 VM、叢集中使用 CSV 儲存體的 VM，使用 SMB 檔案伺服器儲存體的 VM。<br/><br/> **客體 VM 上的 MABS 代理程式**：備份在 VM 上執行的工作負載。 CSV。<br/><br/> **復原**：VM、VHD/磁碟區/資料夾/檔案的項目層級復原。<br/><br/> **Linux VM**：HYPER-V 執行 Windows Server 2012 R2 和更新版本時，請將備份。 Linux VM 的復原會對整個機器執行。 |
**VMware VM：vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3、v2、v1<br/><br/> MABS v1 需要更新彙總套件 1 | 內部部署。 | 備份 Csv、 NFS 和 SAN 儲存體中的 VMware Vm。<br/><br/> 復原整個 VM。<br/><br/> Windows/Linux 備份。<br/><br/> 檔案/資料夾的項目層級復原 (僅適用於 Windows)。<br/><br/> 不支援 VMware vApps。<br/><br/> Linux VM 的復原會對整個機器執行。 | 



## <a name="supported-backups-to-dpm"></a>DPM 支援的備份

下表摘要說明可從內部部署機器和 Azure VM 備份至 DPM 的項目。



**備份** | **DPM** | **詳細資料**
--- | --- | --- 
**Windows 10<br/>Windows 8.1<br/>Windows 8<br/>Windows 7**<br/><br/>(32/64 位元) | 僅限內部部署。<br/><br/> 備份與 DPM 2012 R2 的 Windows 10，建議您安裝[Update 11](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager)。 | 磁碟區/共用/資料夾/檔案。<br/><br/> 支援已刪除重複資料的磁碟區。<br/><br/> 磁碟區至少必須是 1 GB 和 NTFS。
**Windows Server 2016 (Datacenter、Standard、非 Nano)**<br/><br/> 64/32 位元 | 內部部署/Azure VM。<br/><br/> 僅限 DPM 2016。| 磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> 支援已刪除重複資料的磁碟區。
**Windows Server 2012 R2 (Datacenter 和 Standard)**<br/><br/> 64/32 位元 | 內部部署/Azure VM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。<br/><br/> 使用 DPM 2012 R2 和更新版本支援的重複資料刪除磁碟區。
**Windows Server 2012 SP1 (Datacenter 和 Standard)**<br/><br/> 64/32 位元 | 內部部署/Azure VM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。<br/><br/> 使用 DPM 2012 R2 和更新版本支援的重複資料刪除磁碟區。
**Windows Server 2012 (Datacenter 和 Standard)**<br/><br/> 64/32 位元 |  內部部署/Azure VM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。<br/><br/> 使用 DPM 2012 R2 和更新版本支援的重複資料刪除磁碟區。
**Windows 2008 R2 SP1 (Standard 和 Enterprise)**<br/><br/> 64/32 位元 | 內部部署/Azure VM。<br/><br/> 必須安裝 [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855)。 |   **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。<br/><br/> **Azure VM 保護**：磁碟區/共用/資料夾/檔案。
**Windows 2008 R2 (Standard 和 Enterprise)**<br/><br/> 64/32 位元 | 內部部署。<br/><br/> DPM 無法安裝為 VMware VM。<br/><br/> 不支援在 Azure VM 上執行的 DPM。 | **內部部署保護**：磁碟區/共用/資料夾/檔案；系統狀態/裸機。
**Windows Server 2008 SP2**<br/><br/> 64/32 位元 | 僅限內部部署。<br/><br/> 支援作為 VMware VM 執行的 DPM。 不支援以實體伺服器或 Hyper-V VM 的形式執行。 | 磁碟區/共用/資料夾/檔案；系統狀態/裸機。
**Windows Storage Server 2008** | 作為實體伺服器或 Hyper-V VM 執行的內部部署 DPM。 | 磁碟區/共用/資料夾/檔案；系統狀態/裸機。
**SQL Server 2017** | DPM SAC;執行更新彙總 5 或更新版本的 DPM 2016。<br/><br/> 內部部署/Azure VM。| 備份 SQL Server 資料庫。<br/><br/> 不支援 SQL Server 叢集備份。<br/><br/>不支援儲存在 CSV 上的資料庫。
**SQL Server 2016 SP1** | 不支援 DPM 2012 R2；支援 DPM SAC，以及執行更新彙總套件 4 或更新版本的 DPM 2016。<br/><br/> 內部部署/Azure VM。| 備份 SQL Server 資料庫。<br/><br/> 不支援 SQL Server 叢集備份。<br/><br/>不支援儲存在 CSV 上的資料庫。
**SQL Server 2016** | 不支援 DPM 2012 R2。 DPM SAC，從更新彙總套件 2 和更新版本的 DPM 2016 的支援。<br/><br/> 內部部署/Azure VM。| 備份 SQL Server 資料庫。<br/><br/> 不支援 SQL Server 叢集備份。<br/><br/>不支援儲存在 CSV 上的資料庫。
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | 使用 DPM 2012 R2 執行更新彙總套件 4 及更新版本的 SQL Server 2014。<br/><br/> 內部部署/Azure VM。| 備份 SQL Server 資料庫。<br/><br/> 不支援 SQL Server 叢集備份。<br/><br/>不支援儲存在 CSV 上的資料庫。
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | 使用 Exchange 2016 時，DPM 2012 R2 需要更新彙總套件 9 或更新版本。<br/><br/> 內部部署 | 備份獨立 Exchange 伺服器、 DAG 下的資料庫。<br/><br/> 復原信箱、DAG 下的信箱資料庫。<br/><br/> 不支援 ReFS。<br/><br/> 備份非共用磁碟叢集。<br/><br/> 備份已設定連續複寫的 Exchange Server。
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | DPM 2016 和更新版本的 SharePoint 2016。<br/><br/>內部部署/Azure VM。 | 備份伺服器陣列、 前端網頁伺服器。<br/><br/> 復原伺服器陣列、 資料庫、 web 應用程式、 檔案或清單項目、 SharePoint 搜尋、 前端網頁伺服器。<br/><br/> 您無法將備份的內容資料庫使用 SQL Server AlwaysOn 的伺服器陣列。
**Windows Server 2016 上的 Hyper-V**<br/><br/> **Windows Server 2012 R2/2012** (Datacenter/Standard)<br/><br/> **Windows Server 2008 R2 (含 SP1)** | Hyper-v 上 2016 DPM 2016 和更新版本支援。<br/><br/> 內部部署。 | **Hyper-V 主機上的 MABS 代理程式**：備份整個 VM 和主機資料檔案。 備份使用本機儲存體的 VM、叢集中使用 CSV 儲存體的 VM，使用 SMB 檔案伺服器儲存體的 VM。<br/><br/> **客體 VM 上的 MABS 代理程式**：備份在 VM 上執行的工作負載。 CSV。<br/><br/> **復原**：VM、VHD/磁碟區/資料夾/檔案的項目層級復原。<br/><br/> **Linux VM**：HYPER-V 執行 Windows Server 2012 R2 和更新版本時，請將備份。 Linux VM 的復原會對整個機器執行。
**VMware VM：vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3、v2、v1<br/><br/> DPM 2012 R2 需要 System Center 更新彙總套件 1 <br/><br/>內部部署。 | 備份 Csv、 NFS 和 SAN 儲存體中的 VMware Vm。<br/><br/> 復原整個 VM。<br/><br/> Windows/Linux 備份。<br/><br/> 檔案/資料夾的項目層級復原 (僅適用於 Windows)。<br/><br/> 不支援 VMware vApps。<br/><br/> Linux VM 的復原會對整個機器執行。


- 請注意，備份的 DPM/MABS 的叢集工作負載應該與 DPM/MABS 位於相同網域或子/信任的網域。 
- 您可以使用 NTLM/憑證驗證，在不受信任的網域或工作群組中備份資料。



## <a name="next-steps"></a>後續步驟

- [深入了解](backup-architecture.md#architecture-back-up-to-dpmmabs) MABS 架構。
- [檢閱](backup-support-matrix-mars-agent.md) MARS 代理程式支援的作業。
- [設定](backup-azure-microsoft-azure-backup.md) MABS 伺服器。
- [設定 DPM](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-180)。

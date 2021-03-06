---
title: Azure 虛擬機器上的大型主機重新裝載 |Microsoft Docs
description: 重新裝載您的大型主機工作負載，例如 IBM Z 為基礎的系統，使用 Microsoft Azure 上的虛擬機器 (Vm)。
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 8be0ebc486739f8826e8a1d5a5307a219ba71b6f
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192712"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Azure 虛擬機器上的大型主機重新裝載

移轉工作負載從大型主機環境，以雲端可讓您現代化您的基礎結構，並經常節省成本。 許多工作負載只要稍微變更程式碼 (如更新資料庫的名稱) 就可以傳輸至 Azure。

詞彙*大型主機*通常是指大型電腦系統，但目前已部署的大部分 IBM 系統 Z 伺服器或執行 MVS、 DOS、 VSE、 os/390 或 z/OS 的 IBM plug-compatible 系統。

Azure 虛擬機器 (VM) 用來隔離及管理特定的應用程式的單一執行個體的資源。 大型主機，例如 IBM z/OS 會用於此用途的邏輯資料分割 (LPARS)。 大型主機可能會使用一個 LPAR CICS 區域相關聯的 COBOL 程式，並個別 LPAR IBM Db2 資料庫。 典型[在 Azure 上的多層式架構應用程式](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server)到可以分割成每個層級的子網路的虛擬網路中部署 Azure Vm。

Azure Vm 可以執行的模擬大型主機環境支援隨即轉移案例的編譯器。 開發和測試通常是從大型主機移轉至 Azure 的開發/測試環境的第一個工作負載之間。 一般的伺服器元件，您可以模擬包括線上交易處理 (OLTP)、 批次和資料擷取系統，如下列圖所示。

![在 Azure 上的模擬環境可讓您執行 z/OS 為基礎的系統。](media/01-overview.png)

相對很容易，可以某些大型主機工作負載移轉至 Azure，而其他人可以使用協力廠商解決方案在 Azure 上重新裝載。 如需選擇協力廠商解決方案，詳細指引[Azure 大型主機移轉中心](https://azure.microsoft.com/migration/mainframe/)可以幫助。

## <a name="set-up-devtest-environment-using-a-micro-focus-rehosting-platform"></a>設定使用 「 Micro Focus 重新裝載的平台的開發/測試環境

Micro Focus Enterprise Server 是其中一個最大重新裝載平台可用的大型主機。 您可以使用它在成本較低的 x86 上執行 z/OS 的工作負載在 Azure 上的平台。

若要開始，請參閱下列文章：

- [在 Azure 上安裝 Micro Focus Enterprise Server 4.0 和企業開發人員 4.0](./microfocus/set-up-micro-focus-on-azure.md)
- [設定 Micro 焦點 CICS BankDemo Micro 焦點企業在 Azure 中的開發人員 4.0](./microfocus/demo.md)

## <a name="tmaxsoft-openframe-on-azure"></a>在 Azure 上的 TmaxSoft OpenFrame

TmaxSoft OpenFrame 是受歡迎的大型主機重新裝載的解決方案，方便您現有的大型主機資產原形移轉它們至 Azure。 在 Azure 上的 OpenFrame 環境適合用來開發、 示範、 測試或生產環境工作負載。

若要開始，下載[在 Azure 上安裝 TmaxSoft OpenFrame](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-on-azure/)電子書。

## <a name="set-up-a-devtest-environment-using-ibm-z-devtest-120"></a>設定使用 IBM Z 開發/測試 12.0 的開發/測試環境

IBM Z 開發和測試環境 (IBM zD & T) 設定可用於開發、 測試和示範的 z/OS 型的應用程式的 Azure 上的非生產環境。

在 Azure 上的模擬環境可以裝載各種不同的 Z 透過應用程式開發人員控制散發套件 (ADCDs) 的執行個體。 您可以在 Azure 和 Azure Stack 上執行 zD T Personal Edition、 zD T 平行 Sysplex，和 zD & T Enterprise Edition。

若要開始，請參閱下列文章：

-   [設定 IBM zD 在 Azure 上的 T 12.0](./ibm/install-ibm-z-environment.md)
-   [設定 ADCD 上 zD (& T)](./ibm/demo.md)

## <a name="migrate-ibm-db2-purescale-to-azure"></a>將 IBM DB2 pureScale 移轉至 Azure

IBM DB2 pureScale 環境提供適用於 Azure 的資料庫叢集，可在 Linux 作業系統上提供高可用性與延展性。 Linux 上的 IBM DB2 pureScale 雖然與原始環境不同，但針對在大型主機上之 Parallel Sysplex 設定中執行的 z/OS 提供與 IBM DB2 類似的高可用性與延展性。

若要開始，請參閱[在 Azure 上的 IBM DB2 pureScale](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)。

## <a name="considerations"></a>考量

當您將大型主機工作負載移轉至 Azure 基礎結構即服務 (IaaS) 時，您可以選擇數種類型的隨選、 可調整運算資源，包括 Azure Vm。 Azure 提供多種[Linux](https://docs.microsoft.com/azure/virtual-machines/linux/overview)並[Windows](https://docs.microsoft.com/azure/virtual-machines/windows/overview) Vm。

### <a name="high-availability-and-failover"></a>高可用性和容錯移轉

Azure 提供的承諾用量型服務等級協定 (Sla)，其中多個 9 可用性是預設值，已使用的服務的本機或異地複寫進行最佳化。 完整 [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) 說明保證的 Azure 整體可用性。

Vm 之類的 Azure iaas、 容錯移轉依賴特定系統功能，例如容錯移轉叢集執行個體和[可用性設定組](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability#availability-sets)。 當您使用 Azure 平台即服務 (PaaS) 資源，例如[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)並[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)，平台會自動處理容錯移轉。

### <a name="scalability"></a>延展性

大型主機通常相應放大，在雲端環境的向外延展時。Azure 提供多種[Linux](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)並[Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)大小，以符合您的需求。 向上或向下比對確切的使用者規格，也會調整的雲端。 計算能力、 儲存體和服務[擴展](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling)依使用量為基礎的計費模式下的需求。

### <a name="storage"></a>儲存體

在雲端中，您有一系列的彈性、 可擴充的儲存體選項，以及您只需支付您所需要。 [Azure 儲存體](https://docs.microsoft.com/azure/storage/common/storage-introduction)提供可大幅調整的資料物件存放區、雲端檔案系統服務、可靠的訊息存放區，以及 NoSQL 存放區。 針對 VM，受控和非受控磁碟可提供安全的永續性磁碟儲存體。

### <a name="backup-and-recovery"></a>備份與復原

維護您自己的災害復原站台可能會耗費成本的作法。 Azure 有容易實作且符合成本效益的選項，如[備份](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup)，[復原](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)，並[備援](https://docs.microsoft.com/azure/storage/common/storage-redundancy)在本機或區域層級，或透過異地備援。

## <a name="azure-government-for-mainframe-migrations"></a>適用於大型電腦移轉 azure Government

許多公共部門實體希望能夠將他們更現代、 有彈性的平台的大型主機應用程式。 Microsoft Azure Government 是全域的 Microsoft Azure 平台為基礎，但封裝適用於美國聯邦、 州及地方政府系統的雲端服務的實際分開執行個體。 它提供世界級的安全性、 保護及合規性服務，專為美國政府機關和協力廠商。

Azure Government 所獲得的操作 (P-ATO) 取得暫時性的授權單位的 FedRAMP 高影響系統需要這種類型的環境。 

若要開始，下載[大型電腦應用程式的 Microsoft Azure Government 雲端](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/)。

## <a name="learn-more"></a>深入了解

如果您考慮的大型電腦移轉，我們廣泛[夥伴](partner-workloads.md)生態系統是可協助您。 如需選擇合作夥伴解決方案的詳細指引，請參閱 [Platform Modernization Alliance](https://www.platformmodernization.org/pages/mainframe.aspx)。

另請參閱：

-   [大型主機移轉](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
-   [疑難排解](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
-   [揭開大型主機移轉至 Azure 的神秘面紗](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md

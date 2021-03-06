---
title: 設定使用者設定檔共用 Windows 虛擬桌面預覽主應用程式集區-Azure
description: 如何設定 Windows 虛擬桌面預覽主應用程式集區 FSLogix 設定檔容器。
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: c9c2ca2cc27c5fa757b8ff6846e0a6a8f7087875
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403709"
---
# <a name="set-up-a-user-profile-share-for-a-host-pool"></a>設定主應用程式集區的使用者設定檔共用

Windows 虛擬桌面預覽服務提供建議的使用者設定檔方案 FSLogix 設定檔的容器。 我們不建議使用使用者設定檔磁碟 (UPD) 方案，並將 Windows 虛擬桌面的未來版本中被取代。

本節會告訴您如何設定主應用程式集區的 FSLogix 設定檔容器共用。

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>建立新的虛擬機器會做為檔案共用

建立虛擬機器時，請務必將它放上是相同的虛擬網路的主應用程式集區虛擬機器或虛擬網路可連線至主應用程式集區的虛擬機器上。 您可以透過多種方式來建立虛擬機器：

- [從 Azure 映像庫映像建立虛擬機器](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [從受控映像建立虛擬機器](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [從非受控映像建立虛擬機器](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

建立虛擬機器之後, 將它加入網域可以執行下列動作：

1. [連接到虛擬機器](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine)時建立虛擬機器所提供的認證。
2. 在虛擬機器上啟動**控制台中**，然後選取**系統**。
3. 選取 **電腦名稱**，選取**變更設定**，然後選取 **變更...**
4. 選取 **網域**，然後輸入虛擬網路上的 Active Directory 網域。
5. 驗證程式的電腦加入網域的權限的網域帳戶。

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>準備虛擬機器以做為使用者設定檔的檔案共用

以下是有關如何準備虛擬機器的一般指示，以做為使用者設定檔的檔案共用：

1. 加入工作階段主機的虛擬機器，以[Active Directory 安全性群組](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-security-groups)。 此安全性群組將用來驗證工作階段主機的虛擬機器，以您剛建立檔案共用的虛擬機器中。
2. [連接到檔案共用的虛擬機器](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine)。
3. 在檔案共用的虛擬機器，請上建立資料夾**C 磁碟機**，用以做為設定檔共用。
4. 以滑鼠右鍵按一下新的資料夾中，選取**屬性**，選取**共用**，然後選取**進階共用...**.
5. 選取 **共用此資料夾**，選取**權限...**，然後選取**加入...**.
6. 搜尋您要加入工作階段主機虛擬機器的安全性群組，然後確定該群組具有**完全控制**。
7. 之後新增的安全性群組，以滑鼠右鍵按一下該資料夾中，選取**屬性**，選取**共用**，然後向下複製**網路路徑**供稍後使用。

如需權限的最佳作法，請參閱下面的[FSLogix 文件](https://support.fslogix.com/index.php/forum-main/faqs/84-best-practices#120)。

## <a name="configure-the-fslogix-profile-container"></a>設定 FSLogix 設定檔的容器

若要使用 FSLogix 軟體中設定虛擬機器，請依下列方式向主應用程式集區的每部機器上：

1. [連接到虛擬機器](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine)時建立虛擬機器所提供的認證。
2. 啟動網際網路瀏覽器並瀏覽至下列[連結](https://go.microsoft.com/fwlink/?linkid=2084562)下載 FSLogix 代理程式。 Windows 虛擬桌面的公開預覽的一部分，您會收到啟用 FSLogix 軟體的授權金鑰。 關鍵在於 FSLogix 代理程式.zip 檔案中所包含的 LicenseKey.txt 檔。
3. 安裝 FSLogix 代理程式。
4. 瀏覽至**Program Files** > **FSLogix** > **應用程式**以確認安裝的代理程式。
5. 從 [開始] 功能表中，執行**RegEdit**身為系統管理員。 瀏覽至**電腦\\HKEY_LOCAL_MACHINE\\軟體\\FSLogix\\設定檔**
6. 建立下列值：

| 名稱                | 類型               | 資料/值                        |
|---------------------|--------------------|-----------------------------------|
| 已啟用             | DWORD              | 1                                 |
| VHDLocations        | 多字串值 | 「 檔案共用的網路路徑 」 |
| VolumeType          | 字串             |  VHDX                              |
| SizeInMBs           | DWORD              | "的整數表示設定檔的大小 」     |
| IsDynamic           | DWORD              | 1                                 |
| LockedRetryCount    | DWORD              | 1                                 |
| LockedRetryInterval | DWORD              | 0                                 |

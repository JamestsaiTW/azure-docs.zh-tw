---
title: 準備將 VMware 複寫至 Azure 的目標環境 | Microsoft Docs
description: 本文說明如何準備您的目標 Azure 環境，用於將 VMware 虛擬機器複寫至 Azure。
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/03/2019
ms.author: mayg
ms.openlocfilehash: e75d4b1701944e206fcf6ded2dcb6d1e1fbc77cb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57900551"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>準備目標環境以進行從 VMware VM 或實體伺服器至 Azure 的災害復原

此文章將說明如何準備您的目標 Azure 環境，以開始將 VMware 虛擬機器或實體伺服器複寫至 Azure。

## <a name="prerequisites"></a>必要條件

本文假設：
- 您已經在 [Azure 入口網站](https://portal.azure.com "Azure 入口網站")上建立復原服務保存庫以保護您的來源機器
- 您已經將內部部署環境設定為將來源 [VMware 虛擬機器](vmware-azure-set-up-source.md)或[實體伺服器](physical-azure-set-up-source.md)複寫至 Azure。

## <a name="prepare-target"></a>準備目標

完成**步驟 1：選取保護目標**和**步驟 2：準備來源**之後，您會前往**步驟 3：目標**

![準備目標](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **訂用帳戶：** 從下拉式功能表中選取您想要的訂用帳戶，以將虛擬機器或實體伺服器複寫至該訂用帳戶。
2. **部署模型：** 選取部署模型 (傳統或資源管理員)

根據所選的部署模型，以確保您複寫及容錯移轉到目標訂用帳戶中有至少一個虛擬網路，您的虛擬機器或實體伺服器至執行驗證。

驗證成功完成後，按一下 [確定] 以移至下一個步驟。

如果您沒有虛擬網路，您可以建立一個依序按一下 **+ 網路**在頁面頂端的按鈕。

## <a name="next-steps"></a>後續步驟
[設定複寫設定](vmware-azure-set-up-replication.md)。

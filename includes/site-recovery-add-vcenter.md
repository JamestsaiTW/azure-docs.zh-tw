---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 926fb3e9a2c09d30da549330842d8b7e185674ae
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57908447"
---
在 [新增 vCenter] 中，指定 vSphere 主機或 vCenter 伺服器的易記名稱，然後指定伺服器的 IP 位址或 FQDN。 除非已将 VMware 服务器配置为在不同的端口上侦听请求，否则请保留 443 作为端口号。 選取將用於連接至 VMware vCenter 或 vSphere ESXi 伺服器的帳戶。 按一下 [確定]。

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > 如果在添加 VMware vCenter 服务器或 VMware vSphere 主机时使用的帐户没有对 vCenter 或主机服务器的管理员权限，请确保为帐户启用了以下权限：数据中心、数据存储、文件夹、主机、网络、资源、虚拟机和 vSphere 分布式交换机。 此外，VMware vCenter 伺服器需要啟用儲存體檢視權限。

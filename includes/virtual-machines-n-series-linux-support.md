---
title: 包含檔案
description: 包含檔案
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: b8aba410cd447f14fcce89fee93c5f6a253a34ce
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2019
ms.locfileid: "56101817"
---
## <a name="supported-distributions-and-drivers"></a>支援的散發套件和驅動程式

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA 驅動程式

只有下表所列的 Linux 發行版本才支援 NC、NCv2、NCv3、ND 及 NDv2 系列 VM (針對 NV 系列為選擇性) 的 NVIDIA CUDA 驅動程式。 CUDA 驅動程式為本文章發行時的最新資訊。 如需最新的 CUDA 驅動程式，請瀏覽 [NVIDIA](https://developer.nvidia.com/cuda-zone) 網站。 請確定您已安裝或升級為散發套件所適用的最新 CUDA 驅動程式。 

> [!TIP]
> 在 Linux VM 上手動安裝 CUDA 驅動程式的替代方案，就是部署 Azure [資料科學虛擬機器](../articles/machine-learning/data-science-virtual-machine/overview.md)映像。 適用於 Ubuntu 16.04 LTS 或 CentOS 7.4 的 DSVM 版本會預先安裝 NVIDIA CUDA 驅動程式、CUDA 深度類神經網路程式庫和其他工具。

| 配送映像 | 驅動程式 |
| --- | -- | 
| Ubuntu 16.04 LTS、18.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3、7.4、7.5、7.6<br/><br/> CentOS 型 7.3、7.4、7.5、7.6、CentOS 型 7.4 HPC | NVIDIA CUDA 10.0，驅動程式分支 R410 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID 驅動程式

Microsoft 會針對用來作為虛擬工作站的 NV 和 NVv2 系列虛擬機器或虛擬應用程式，重新發佈 NVIDIA GRID 驅動程式安裝程式。 請僅將這些 GRID 驅動程式安裝在 Azure NV 虛擬機器上，且僅安裝在下表所列的作業系統上。 這些驅動程式包含在 Azure 的 GRID 虛擬 GPU 軟體的授權中。 您不需要設定 NVIDIA vGPU 軟體授權伺服器。

| 配送映像 | 驅動程式 |
| --- | -- |
| Ubuntu 16.04 LTS、18.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3、7.4、7.5、7.6<br/><br/>CentOS 型 7.3、7.4、7.5、7.6 | NVIDIA GRID 7.1，驅動程式分支 R410|

> [!WARNING] 
> 在 Red Hat 產品上安裝第三方軟體可能會影響 Red Hat 支援條款。 請參閱 [Red Hat 知識庫文件 (英文)](https://access.redhat.com/articles/1067)。
>

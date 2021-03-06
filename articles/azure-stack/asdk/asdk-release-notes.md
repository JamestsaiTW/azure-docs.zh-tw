---
title: Microsoft Azure Stack 開發套件版本資訊 | Microsoft Docs
description: Azure Stack 開發套件的增強功能、修正及已知問題。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 02/09/2019
ms.openlocfilehash: 485d88a4765d7cedcb171a5b325fe5f366fff1f9
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004741"
---
# <a name="asdk-release-notes"></a>ASDK 版本資訊

本文提供 Azure Stack 開發套件 (ASDK) 中變更、修正及已知問題的相關資訊。 如果您不確定所執行的版本，可以使用[入口網站來進行檢查](../azure-stack-updates.md#determine-the-current-version)。

請訂閱 [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [摘要](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)，以便隨時收到 ASDK 的最新訊息。

## <a name="build-11901095"></a>組建 1.1901.0.95

### <a name="changes"></a>變更

此組建包含下列適用於 Azure Stack 的改良功能：

- BGP 和 NAT 元件現在會部署於實體主機上。 這樣就不需要使用兩個公用或公司 IP 位址來部署 ASDK，而且也可簡化部署。
- Azure Stack 整合系統備份現在可以使用 **asdk-installer.ps1** PowerShell 指令碼來[驗證](asdk-validate-backup.md)。

### <a name="new-features"></a>新功能

- 如需此版本中新功能的清單，請參閱 Azure Stack 版本資訊的[這個小節](../azure-stack-update-1901.md#new-features)。

### <a name="fixed-and-known-issues"></a>已修正和已知問題

- 如需此版本中已修正問題的清單，請參閱 Azure Stack 版本資訊的[這個小節](../azure-stack-update-1901.md#fixed-issues)。 如需已知問題的清單，請參閱[這個小節](../azure-stack-update-1901.md#known-issues-post-installation)。
- 請注意，[可用的 Azure Stack Hotfix](../azure-stack-update-1901.md#azure-stack-hotfixes) 不適用 Azure Stack ASDK。

## <a name="build-118110101"></a>組建 1.1811.0.101

### <a name="changes"></a>變更

此組建包含下列適用於 Azure Stack 的改良功能：  

- 針對 ASDK，有一組新的最基本且建議的硬體與軟體需求。 這些新的建議規格記載於 [Azure Stack 部署規劃考量](asdk-deploy-considerations.md)。 由於 Azure Stack 平台已經演進，因此現在有更多服務可供使用，也可能需要更多資源。 增加的規格皆反映在這些已修訂的建議事項中。

### <a name="new-features"></a>新功能

如需此版本中新功能的清單，請參閱 Azure Stack 版本資訊的[這個小節](../azure-stack-update-1811.md#new-features)。

### <a name="fixed-and-known-issues"></a>已修正和已知問題

如需此版本中已修正問題的清單，請參閱 Azure Stack 版本資訊的[這個小節](../azure-stack-update-1811.md#fixed-issues)。 如需已知問題的清單，請參閱[這個小節](../azure-stack-update-1811.md#known-issues-post-installation)。

## <a name="build-11809090"></a>組建 1.1809.0.90

### <a name="new-features"></a>新功能

如需此版本中新功能的清單，請參閱 Azure Stack 版本資訊的[這個小節](../azure-stack-update-1809.md#new-features)。

### <a name="fixed-issues"></a>已修正的問題

如需此版本中已修正問題的清單，請參閱[這個小節](../azure-stack-update-1809.md#fixed-issues)。

### <a name="known-issues"></a>已知問題

如需此版本中已知問題的清單，請參閱[這個小節](../azure-stack-update-1809.md#known-issues-post-installation)。
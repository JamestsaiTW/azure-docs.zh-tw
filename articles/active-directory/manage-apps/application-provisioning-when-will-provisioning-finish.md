---
title: 將使用者佈建至 Azure AD 資源庫應用程式花費數小時以上 | Microsoft Docs
description: 如何查明佈建至應用程式為什麼比您預期的更久
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 834e2532354b91410943f5fe2b2e78bca0bd0922
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173040"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>將使用者佈建至 Azure AD 資源庫應用程式花費數小時以上

當第一次啟用應用程式的自動佈建時，初始同步處理可能花費 20 分鐘到數小時，根據 Azure AD 目錄大小和佈建範圍中的使用者數目而定。 

初始同步處理之後的後續同步處理會更快，因為佈建服務會儲存浮水印，代表兩個系統在初始同步處理之後的狀態，所以會改善後續同步處理的效能。

## <a name="how-to-improve-provisioning-performance"></a>如何改善佈建的效能

如果初始同步處理花費好幾個小時，還有一個作法可以改善效能︰

-   **使用者範圍設定篩選條件。** 範圍設定篩選條件可讓您根據特定的屬性值篩選出使用者，以微調佈建服務從 Azure AD 擷取的資料。 如需範圍設定篩選條件的詳細資訊，請參閱[根據範圍設定篩選條件以屬性為基礎的應用程式佈建](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters)。

## <a name="next-steps"></a>後續步驟
[自動化使用 Azure Active Directory 對於 SaaS 應用程式的使用者佈建和解除佈建](user-provisioning.md)


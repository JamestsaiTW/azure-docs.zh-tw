---
title: 新增或更新使用者的設定檔資訊 - Azure Active Directory | Microsoft Docs
description: 有關如何向 Azure Active Directory 中的使用者設定檔新增資訊的指示，包括圖片和作業的詳細資料。
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: b11c71f7f5a329a836d379a16afe66c08572ccde
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177983"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>使用 Azure Active Directory 新增或更新使用者的設定檔資訊
使用 Azure Active Directory (Azure AD) 新增使用者設定檔資訊，包括個人資料圖片、工作特定資訊，以及一些設定。 如需新增使用者的詳細資訊，請參閱[如何在 Azure Active Directory 中新增或刪除使用者](add-users-azure-active-directory.md)。

## <a name="add-or-change-profile-information"></a>新增或變更設定檔資訊
如您所見，使用者設定檔中所提供資訊比您在使用者建立期間可以新增的資訊還多。 所有這些額外資訊都是選擇性，可以依據您的組織需求新增。

## <a name="to-add-or-change-profile-information"></a>新增或變更設定檔資訊
1. 以目錄的全域系統管理員或使用者系統管理員身分登入 [Azure 入口網站](https://portal.azure.com/)。

2. 選取 [Azure Active Directory] 並選取 [使用者]，然後選取一位使用者。 例如 _Alain Charon_。

    [Alain Charon - 設定檔] 頁面隨即出現。

    ![包括可編輯資訊的使用者設定檔頁面](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. 選取 [編輯]，選擇性地新增或更新各個可用區段中包含的資訊。

    ![顯示可編輯區域的使用者設定檔頁面](media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **個人資料圖片。** 選取使用者帳戶的縮圖影像。 此圖會出現在 Azure Active Directory 和使用者的個人頁面上，例如 myapps.microsoft.com 頁面。

    - **身分識別。** 新增任何帳戶相關資訊，例如已婚姓氏或變更的使用者名稱。 

    - **工作資訊。** 新增任何工作相關資訊，例如使用者的職稱、部門或經理。

    - **設定。** 決定使用者是否可以登入 Azure Active Directory 租用戶。 您也可以指定使用者的全域位置。

    - **。** 新增使用者的任何相關連絡資訊。 例如，街道地址或行動電話號碼。

    - **驗證連絡資訊。** 驗證這項資訊，以確定具有使用者的有效電話號碼和電子郵件地址。 Azure Active Directory 會使用這項資訊，在登入期間確定使用者是真正的使用者。 只有全域系統管理員才能更新驗證連絡資訊。

4. 選取 [ **儲存**]。

    系統將為使用者儲存您的所有變更。

    >[!Note]
    >您必須使用 Windows Server Active Directory 來更新使用者(其授權來源是 Windows Server Active Directory) 的身分識別、連絡資訊或工作資訊。 完成更新之後，您必須等候下一次同步處理循環完成，才能看到您所做的變更。

## <a name="next-steps"></a>後續步驟
更新使用者設定檔之後，您可以執行下列基本程序：

- [新增或刪除使用者](add-users-azure-active-directory.md)

- [將角色指派給使用者](active-directory-users-assign-role-azure-portal.md)

- [建立基本群組並新增成員](active-directory-groups-create-azure-portal.md)

或者，您也可以執行其他使用者管理工作，例如指派委派、使用原則及共用使用者帳戶。 如需其他可用動作的詳細資訊，請參閱 [Azure Active Directory 使用者管理文件](../users-groups-roles/index.yml)。

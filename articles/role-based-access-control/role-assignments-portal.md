---
title: 管理 Azure 資源使用 RBAC 和 Azure 入口網站的存取權 |Microsoft Docs
description: 了解如何使用角色型存取控制 (RBAC) 與 Azure 入口網站來管理使用者、群組、服務主體與受控識別針對 Azure 資源的存取權。 這包括如何列出存取權、授與存取權以及移除存取權。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/24/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: bb23cbc275e01eab5361504c547c020b0a29f4c3
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/25/2019
ms.locfileid: "56805285"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-azure-portal"></a>使用 RBAC 和 Azure 入口網站管理對 Azure 資源的存取

[角色型存取控制 (RBAC)](overview.md) 是您管理對 Azure 資源存取的機制。 本文將告訴您如何管理使用 Azure 入口網站的存取。 如果您需要管理 Azure Active Directory 存取權，請參閱[Azure Active Directory 中的檢視，並指派系統管理員角色](../active-directory/users-groups-roles/directory-manage-roles-portal.md)。

## <a name="prerequisites"></a>必要條件

若要新增及移除角色指派，您必須：

- `Microsoft.Authorization/roleAssignments/write` 並`Microsoft.Authorization/roleAssignments/delete`權限，例如[使用者存取系統管理員](built-in-roles.md#user-access-administrator)或[擁有者](built-in-roles.md#owner)

## <a name="overview-of-access-control-iam"></a>存取控制 (IAM) 的概觀

**存取控制 (IAM)** 是您用來管理 Azure 資源的存取權 刀鋒視窗。 它亦稱為身分識別和存取管理，並顯示在 Azure 入口網站中的數個位置。 下面顯示某訂用帳戶之 [存取控制 (IAM)] 刀鋒視窗的範例。

![訂用帳戶的存取控制 (IAM) 刀鋒視窗](./media/role-assignments-portal/access-control-numbers.png)

下表說明的某些元素是適用於：

| # | 元素 | 您將它用於 |
| --- | --- | --- |
| 1 | 資源的存取控制 (IAM) 開啟的位置 | 識別範圍 （在此範例中的訂閱） |
| 2 | **新增**按鈕 | 新增角色指派 |
| 3 | **檢查存取** 索引標籤 | 檢視單一使用者的角色指派 |
| 4 | **角色指派** 索引標籤 | 檢視目前範圍的角色指派 |
| 5 | **角色** 索引標籤 | 檢視所有的角色和權限 |

若要最有效的存取控制 (IAM) 刀鋒視窗，將有所助益時想要管理存取權，您可以回答下列三個問題：

1. **誰需要存取？**

    誰是指使用者、 群組、 服務主體或受管理的身分識別。 這也稱為*安全性主體*。

1. **他們需要什麼權限？**

    權限會分組放入角色。 您可以從數個內建角色清單中選取。

1. **他們需要在其中存取？**

    其中是指一組存取權會套用到的資源。 其中可以是管理群組、 訂用帳戶、 資源群組或單一資源，例如儲存體帳戶。 這就叫做*範圍*。

## <a name="open-access-control-iam"></a>開啟存取控制 (IAM)

您必須決定的第一件事就是開啟存取控制 (IAM) 刀鋒視窗的位置。 這取決於您想要何種的資源管理的存取權。 若要管理存取權管理群組、 訂用帳戶、 資源群組或單一資源中的所有項目中的所有項目中的所有項目嗎？

1. 在 Azure 入口網站中，按一下**所有服務**，然後選取範圍。 例如，您可以選取 [管理群組]、[訂用帳戶]、[資源群組]或資源。

1. 按一下特定的資源。

1. 按一下 [存取控制 (IAM)]。

    下面顯示某訂用帳戶之 [存取控制 (IAM)] 刀鋒視窗的範例。 如果您在任何存取控制此變更，它們會套用到整個訂用帳戶。

    ![訂用帳戶的存取控制 (IAM) 刀鋒視窗](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>檢視角色與權限

角色定義是您用於角色指派的權限集合。 Azure 具有超過 70 個[適用於 Azure 資源的內建角色](built-in-roles.md)。 請遵循下列步驟來檢視可用的角色和權限。

1. 開啟**存取控制 (IAM)** 在任何範圍。

1. 按一下 [角色] 索引標籤以查看所有內建與自訂角色清單。

   您可以看到使用者和群組指派給在目前的範圍內的每個角色的數目。

   ![角色清單](./media/role-assignments-portal/roles-list.png)

1. 按一下個別角色以檢視誰已獲指派此角色，以及檢視該角色的權限。

   ![角色指派](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>檢視角色指派

管理存取權，當您想要知道誰可以存取，什麼是其權限，以及在何種範圍內。 清單存取使用者、 群組、 服務主體或受控身分識別，以檢視其角色指派。

### <a name="view-role-assignments-for-a-single-user"></a>檢視單一使用者的角色指派

依照這些步驟檢視特定範圍中單一使用者、群組、服務主體或受控識別的存取權。

1. 針對您要檢視存取權的範圍 (例如管理群組、訂用帳戶、資源群組或資源) 開啟 [存取控制 (IAM)]。

1. 按一下 [檢查存取權] 索引標籤。

    ![存取控制 - [檢查存取權] 索引標籤](./media/role-assignments-portal/access-control-check-access.png)

1. 在 [尋找] 清單中，選取您要檢查其存取權的安全性主體類型。

1. 在搜尋方塊中，輸入字串以在目錄中搜尋顯示名稱、電子郵件地址或物件識別碼。

    ![檢查存取權選取清單](./media/role-assignments-portal/check-access-select.png)

1. 按一下安全性主體以開啟 [指派] 窗格。

    ![[指派] 窗格](./media/role-assignments-portal/check-access-assignments.png)

    在此窗格上，您可以看到已指派所選安全性主體的角色與範圍。 若此範圍中有任何拒絕指派，或有任何拒絕指派繼承到此範圍，它們將會被列出。

### <a name="view-all-role-assignments-at-a-scope"></a>檢視某範圍的角色指派

1. 針對您要檢視存取權的範圍 (例如管理群組、訂用帳戶、資源群組或資源) 開啟 [存取控制 (IAM)]。

1. 按一下 [角色指派] 索引標籤以檢視此範圍中的所有角色指派。

   ![存取控制︰[角色指派] 索引標籤](./media/role-assignments-portal/access-control-role-assignments.png)

   在 [角色指派] 索引標籤上，您可以看到誰在此範圍中有存取權。 請注意，某些角色的範圍限於**此資源**，而有些角色則是來自 **(繼承自)** 另一個範圍。 存取權不是特別指派給此資源群組，就是繼承自父範圍的指派。

## <a name="add-a-role-assignment"></a>新增角色指派

在 RBAC 中，若要授與存取權，您必須將角色指派給使用者、群組、服務主體或受控識別。 依照下列步驟來授與不同範圍的存取權。

### <a name="assign-a-role-at-a-scope"></a>在某個範圍中指派角色

1. 針對您要授與存取權的範圍 (例如管理群組、訂用帳戶、資源群組或資源) 開啟 [存取控制 (IAM)]。

1. 按一下 [角色指派] 索引標籤以檢視此範圍中的所有角色指派。

1. 按一下 [新增] > [新增角色指派]，以開啟 [新增角色指派] 窗格。

   若您沒有指派角色的權限，[新增角色指派] 選項將會被停用。

   ![[新增] 功能表](./media/role-assignments-portal/add-menu.png)

   ![[新增角色指派] 窗格](./media/role-assignments-portal/add-role-assignment.png)

1. 在 [角色] 下拉式清單中選取角色，例如 [虛擬機器參與者]。

1. 在 [選取] 清單中，選取使用者、群組、服務主體或受控識別。 如果在清單中未看到安全性主體，您可以在 [選取] 方塊中輸入，以在目錄中搜尋顯示名稱、電子郵件地址和物件識別碼。

1. 按一下 [儲存] 以指派角色。

   在幾分鐘之後，即會在所選範圍中指派安全性主體的角色。

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>指派使用者做為訂用帳戶的系統管理員

若要將使用者設定為 Azure 訂用帳戶的系統管理員，請在訂用帳戶範圍為其指派[擁有者](built-in-roles.md#owner)角色。 「擁有者」角色可授與使用者訂用帳戶中所有資源的完整存取權，包括將存取權委派給其他人的權限。 針對任何其他角色指派，這些步驟都相同。

1. 在 Azure 入口網站中，按一下 [所有服務]，然後按一下 [訂用帳戶]。

1. 選擇您想要授與存取權的訂用帳戶。

1. 按一下 [存取控制 (IAM)]。

1. 按一下 [角色指派] 索引標籤以檢視此訂用帳戶的所有角色指派。

1. 按一下 [新增] > [新增角色指派]，以開啟 [新增角色指派] 窗格。

   若您沒有指派角色的權限，[新增角色指派] 選項將會被停用。

   ![[新增] 功能表](./media/role-assignments-portal/add-menu.png)

   ![[新增角色指派] 窗格](./media/role-assignments-portal/add-role-assignment.png)

1. 在 [角色] 下拉式清單中，選取 [擁有者] 角色。

1. 在 [選取] 清單中，選取使用者。 如果在清單中未看到使用者，您可以在 [選取] 方塊中輸入，以在目錄中搜尋顯示名稱與電子郵件地址。

1. 按一下 [儲存] 以指派角色。

   在幾分鐘之後，即會在訂用帳戶範圍將「擁有者」角色指派給使用者。

## <a name="remove-role-assignments"></a>移除角色指派

在 RBAC 中，若要移除存取權，您可以移除角色指派。 依照下列步驟來移除存取權。

1. 針對您要移除存取權的範圍 (例如管理群組、訂用帳戶、資源群組或資源) 開啟 [存取控制 (IAM)]。

1. 按一下 [角色指派] 索引標籤以檢視此訂用帳戶的所有角色指派。

1. 在角色指派清單中，在具有您要移除的角色指派安全性主體旁加上核取記號。

   ![移除角色指派訊息](./media/role-assignments-portal/remove-role-assignment-select.png)

1. 按一下 [移除] 。

   ![移除角色指派訊息](./media/role-assignments-portal/remove-role-assignment.png)

1. 在顯示的移除角色指派訊息中，按一下 [是]。

    繼承的角色指派無法移除。 如果您需要移除繼承的角色指派，您必須在建立角色指派的範圍中進行移除。 在 [範圍] 資料行中 [(繼承)] 的旁邊有一個連結，會將您帶往已指派該角色的範圍。 移至該處所列的範圍來移除角色指派。

   ![移除角色指派訊息](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>後續步驟

* [教學課程：使用 RBAC 與 Azure 入口網站為使用者授與 Azure 資源的存取權](quickstart-assign-role-user-portal.md)
* [教學課程：使用 RBAC 與 Azure PowerShell 為使用者授與 Azure 資源的存取權](tutorial-role-assignments-user-powershell.md)
* [針對適用於 Azure 資源的 RBAC 進行疑難排解](troubleshooting.md)
* [使用 Azure 管理群組來組織資源](../governance/management-groups/index.md)

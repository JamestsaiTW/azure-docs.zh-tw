---
title: 如何使用 PowerShell 在 Azure VM 上設定 Azure 資源受控識別
description: 使用 PowerShell 在 Azure VM 上設定 Azure 資源受控識別的逐步指示。
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: priyamo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28f9c17e21db5a46ad01fd1b318c52a3a721f8b9
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226958"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>使用 PowerShell 在 Azure VM 上設定 Azure 資源受控識別

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure 資源受控識別會在 Azure Active Directory 中為 Azure 服務提供自動受控識別。 您可以使用此身分識別來向任何支援 Azure AD 驗證的服務進行驗證，不需要任何您程式碼中的認證。 

在本文中，藉由使用 PowerShell，您將了解如何在 Azure VM 上執行下列 Azure 資源受控識別作業。

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>必要條件

- 如果您不熟悉 Azure 資源的受控識別，請參閱[概觀一節](overview.md)。 **請務必檢閱[系統指派和使用者指派受控識別之間的差異](overview.md#how-does-it-work)**。
- 如果您還沒有 Azure 帳戶，請先[註冊免費帳戶](https://azure.microsoft.com/free/)，再繼續進行。
- 如果您尚未安裝[最新版的 Azure PowerShell](/powershell/azure/install-az-ps)，請先安裝。

## <a name="system-assigned-managed-identity"></a>系統指派的受控識別

在本節中，您會學習如何使用 Azure PowerShell 啟用和停用系統指派的受控識別。

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>在建立 Azure VM 時，啟用系統指派的受控識別

若要建立已啟用系統指派受控識別的 Azure VM，您的帳戶需要[虛擬機器參與者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)角色指派。  不需要其他 Azure AD 目錄角色指派。

1. 請參閱下列 Azure VM 快速入門，完成必要的章節 （「 登入至 Azure 」，「 建立資源群組 」，「 建立網路群組 」，「 建立 VM 」） 的其中一個。
    
    當您參閱「建立VM」一節時，請稍微修改一下 [New-AzVMConfig](/powershell/module/az.compute/new-azvm) Cmdlet 語法。 請務必新增 `-AssignIdentity:$SystemAssigned` 參數，以佈建已啟用系統所指派身分識別的 VM，例如：
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [使用 PowerShell 建立 Windows 虛擬機器](../../virtual-machines/windows/quick-create-powershell.md)
   - [使用 PowerShell 建立 Linux 虛擬機器](../../virtual-machines/linux/quick-create-powershell.md)

> [!NOTE]
> 您可能會選擇性地佈建 Azure 資源的 VM 擴充功能，管理的身分識別，但很快就會被取代。 我們建議使用 Azure 執行個體中繼資料識別端點進行驗證。 如需詳細資訊，請參閱 <<c0> [ 從 VM 延伸模組移轉至 Azure IMDS 端點進行驗證](howto-migrate-vm-extension.md)。

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>在現有 Azure VM 上啟用系統指派的受控識別

若要在原先佈建的 VM 上啟用系統指派的受控識別，您的帳戶需要[虛擬機器參與者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)角色指派。  不需要其他 Azure AD 目錄角色指派。

1. 使用 `Connect-AzAccount` 登入 Azure。 使用與包含虛擬機器的 Azure 訂用帳戶相關聯的帳戶。

   ```powershell
   Connect-AzAccount
   ```

2. 先使用 `Get-AzVM` Cmdlet 擷取 VM 屬性。 然後在 [Update-AzVM](/powershell/module/az.compute/update-azvm) Cmdlet 上使用 `-AssignIdentity` 參數來啟用系統指派的受控識別：

   ```powershell
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

> [!NOTE]
> 您可能會選擇性地佈建 Azure 資源的 VM 擴充功能，管理的身分識別，但很快就會被取代。 我們建議使用 Azure 執行個體中繼資料識別端點進行驗證。 如需詳細資訊，請參閱 <<c0> [ 從 VM 延伸模組移轉至 Azure IMDS 端點進行驗證](howto-migrate-vm-extension.md)。

### <a name="add-vm-system-assigned-identity-to-a-group"></a>將 VM 系統指派的身分識別新增至群組

您在 VM 上啟用系統指派的身分識別後，就可以將它新增至群組。  下列程序會將 VM 其系統指派的身分識別新增至群組。

1. 使用 `Connect-AzAccount` 登入 Azure。 使用與包含虛擬機器的 Azure 訂用帳戶相關聯的帳戶。

   ```powershell
   Connect-AzAccount
   ```

2. 擷取並記下 VM 其服務主體的 `ObjectID` (如所傳回值的 `Id` 欄位中指定)：

   ```powerhshell
   Get-AzADServicePrincipal -displayname "myVM"
   ```

3. 擷取並記下群組的 `ObjectID` (如所傳回值的 `Id` 欄位中指定)：

   ```powershell
   Get-AzADGroup -searchstring "myGroup"
   ```

4. 將 VM 的服務主體新增至群組：

   ```powershell
   Add-AzureADGroupMember -ObjectId "<objectID of group>" -RefObjectId "<object id of VM service principal>"
   ```

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>停用來自 Azure VM 的系統指派受控識別

若要在 VM 上停用系統指派的受控識別，您的帳戶需要[虛擬機器參與者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)角色指派。  不需要其他 Azure AD 目錄角色指派。

如果您的虛擬機器已不再需要系統指派的受控識別，但仍需要使用者指派的受控識別，請使用下列 Cmdlet：

1. 使用 `Connect-AzAccount` 登入 Azure。 使用與包含虛擬機器的 Azure 訂用帳戶相關聯的帳戶。

   ```powershell
   Connect-AzAccount
   ```

2. 使用 `Get-AzVM` Cmdlet 來擷取 VM 屬性，並將 `-IdentityType` 參數設定為 `UserAssigned`：

   ```powershell   
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 
   Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

如果您的虛擬機器不再需要系統指派的受控識別，而且沒有使用者指派的受控識別，請使用下列命令：

```powershell
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

> [!NOTE]
> 如果您已佈建 Azure 資源 （要被取代） 的 VM 延伸模組的受管理身分識別，您需要先使用 [移除 AzVMExtension](/powershell/module/az.compute/remove-azvmextension)。 如需詳細資訊，請參閱 <<c0> [ 從 VM 延伸模組移轉至 Azure 進行驗證的 IMDS](howto-migrate-vm-extension.md)。

## <a name="user-assigned-managed-identity"></a>使用者指派的受控識別

本節中，您將了解如何使用 Azure PowerShell 在 VM 中新增和移除使用者指派的受控識別。

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>在 VM 建立期間，將使用者指派的受控識別指派給 VM

若要將使用者指派的身分識別指派給 VM，您的帳戶需要[虛擬機器參與者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)和[受控識別操作者](/azure/role-based-access-control/built-in-roles#managed-identity-operator)角色指派。 不需要其他 Azure AD 目錄角色指派。

1. 請參閱下列 Azure VM 快速入門，完成必要的章節 （「 登入至 Azure 」，「 建立資源群組 」，「 建立網路群組 」，「 建立 VM 」） 的其中一個。 
  
    當您參閱「建立 VM」一節時，請稍微修改一下 [`New-AzVMConfig`](/powershell/module/az.compute/new-azvm)Cmdlet 語法。 新增 `-IdentityType UserAssigned` 和 `-IdentityID ` 參數，以佈建具有使用者所指派身分識別的 VM。  以您自己的值取代 `<VM NAME>`、`<SUBSCRIPTION ID>`、`<RESROURCE GROUP>` 和 `<USER ASSIGNED IDENTITY NAME>`。  例如︰
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [使用 PowerShell 建立 Windows 虛擬機器](../../virtual-machines/windows/quick-create-powershell.md)
    - [使用 PowerShell 建立 Linux 虛擬機器](../../virtual-machines/linux/quick-create-powershell.md)

> [!NOTE]
> 您可能會選擇性地佈建 Azure 資源的 VM 擴充功能，管理的身分識別，但很快就會被取代。 我們建議使用 Azure 執行個體中繼資料識別端點進行驗證。 如需詳細資訊，請參閱 <<c0> [ 從 VM 延伸模組移轉至 Azure IMDS 端點進行驗證](howto-migrate-vm-extension.md)。

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>將使用者指派的受控識別指派至現有 Azure VM

若要將使用者指派的身分識別指派給 VM，您的帳戶需要[虛擬機器參與者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)和[受控識別操作者](/azure/role-based-access-control/built-in-roles#managed-identity-operator)角色指派。 不需要其他 Azure AD 目錄角色指派。

1. 使用 `Connect-AzAccount` 登入 Azure。 使用與包含虛擬機器的 Azure 訂用帳戶相關聯的帳戶。

   ```powershell
   Connect-AzAccount
   ```

2. 請使用 [New-AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) Cmdlet 來建立使用者指派的受控識別。  請記下輸出中的 `Id`，因為下一個步驟 會需要此項目。

   > [!IMPORTANT]
   > 建立使用者指派給受控身分識別只支援英數字元、 底線和連字號 (0-9、a-z 或 A-Z、0-9\_或-) 字元。 此外，名稱應該是 3 指派給 VM/VMSS 才能正常運作的 128 字元長度限制。 如需詳細資訊，請參閱[常見問題集和已知問題](known-issues.md)

   ```powershell
   New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. 使用 `Get-AzVM` Cmdlet 擷取 VM 屬性。 然後使用 [Update-AzVM](/powershell/module/az.compute/update-azvm) Cmdlet 上的 `-IdentityType` 和 `-IdentityID` 參數，將使用者指派的受控識別新增至 Azure VM。  `-IdentityId` 參數的值是您在上一個步驟中記下的 `Id`。  以您自己的值取代 `<VM NAME>`、`<SUBSCRIPTION ID>`、`<RESROURCE GROUP>` 和 `<USER ASSIGNED IDENTITY NAME>`。

   > [!WARNING]
   > 若要保留任何使用者先前指派給 VM 的受控識別，請查詢 `Identity`VM 物件的屬性 (例如 `$vm.Identity`)。  如果已傳回任何使用者指派的受控識別，請在下列命令中包含這些受控識別，以及您想要指派給 VM 的使用者所指派受控識別。

   ```powershell
   $vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

> [!NOTE]
> 您可能會選擇性地佈建 Azure 資源的 VM 擴充功能，管理的身分識別，但很快就會被取代。 我們建議使用 Azure 執行個體中繼資料識別端點進行驗證。 如需詳細資訊，請參閱 <<c0> [ 從 VM 延伸模組移轉至 Azure IMDS 端點進行驗證](howto-migrate-vm-extension.md)。

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>從 Azure VM 移除使用者指派的受控識別

若要將使用者指派的身分識別移除至 VM，您的帳戶需要[虛擬機器參與者](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor)角色指派。

如果您的 VM 具有多個使用者指派的受控識別，則您可以使用下列命令移除所有身分識別，但請留下最後一個。 請務必以您自己的值取代 `<RESOURCE GROUP>` 和 `<VM NAME>` 參數的值。 `<USER ASSIGNED IDENTITY NAME>` 是使用者指派受控識別的名稱屬性，它應該保留在 VM 上。 查詢 VM 物件的 `Identity` 屬性，即可找到此資訊。  例如，`$vm.Identity`：

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
如果您的虛擬機器沒有系統指派的受控識別，而且您想要從其中移除所有使用者指派的受控識別，請使用下列命令：

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
如果您的虛擬機器同時具有系統指派和使用者指派的受控識別，您可以藉由切換為僅使用系統指派的受控識別，來移除所有使用者指派的受控識別。

```powershell 
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>後續步驟

- [Azure 資源受控識別概觀](overview.md)
- 如需完整的 Azure VM 建立快速入門，請參閱：
  
  - [使用 PowerShell 建立 Windows 虛擬機器](../../virtual-machines/windows/quick-create-powershell.md) 
  - [使用 PowerShell 建立 Linux 虛擬機器](../../virtual-machines/linux/quick-create-powershell.md) 

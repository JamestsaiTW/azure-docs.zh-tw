---
title: 使用 PowerShell 管理 Azure Analysis Services | Microsoft Docs
description: 使用 PowerShell 的 Azure Analysis Services 管理。
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: reference
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 177d74a54e4ab4de698cbb63091656cc8b584e2b
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010679"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>使用 PowerShell 管理 Azure Analysis Services

本文說明用來執行 Azure Analysis Services 伺服器和資料庫管理工作的 PowerShell Cmdlet。 

伺服器管理工作 (例如建立或刪除伺服器、暫停或繼續伺服器作業，或是變更服務等級 (層級)) 會使用 Azure Resource Manager (資源) Cmdlet 和 Analysis Services (伺服器) Cmdlet。 其他資料庫管理工作 (例如新增或移除角色成員、處理或資料分割) 會使用與 SQL Server Analysis Services 相同之 SqlServer 模組內含的 Cmdlet。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>權限

大部分的 PowerShell 工作需要您在您管理的 Analysis Services 伺服器上具備系統管理員權限。 排定的 PowerShell 工作都是自動的作業。 執行排程器的帳戶和服務主體必須具有 Analysis Services 伺服器上的管理員權限。 

使用 Azure PowerShell cmdlet 的伺服器作業，您的帳戶或執行排程器的帳戶也必須屬於擁有者角色中的資源[azure 角色型存取控制 (RBAC)](../role-based-access-control/overview.md)。 

## <a name="resource-management-operations"></a>資源管理作業 

Module - [Az.AnalysisServices](/powershell/module/az.analysisservices)

|Cmdlet|描述| 
|------------|-----------------| 
|[Get-AzAnalysisServicesServer](/powershell/module/az.analysisservices/get-azanalysisservicesserver)|取得伺服器執行個體的詳細資料。|  
|[New-AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver)|建立伺服器執行個體。|   
|[New-AzAnalysisServicesFirewallConfig](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallconfig)|建立新的 Analysis Services 防火牆設定。|   
|[New-AzAnalysisServicesFirewallRule](/powershell/module/az.analysisservices/new-azanalysisservicesfirewallrule)|建立新的 Analysis Services 防火牆規則。|   
|[Remove-AzAnalysisServicesServer](/powershell/module/az.analysisservices/remove-azanalysisservicesserver)|移除伺服器執行個體。|  
|[Resume-AzAnalysisServicesServer](/powershell/module/az.analysisservices/resume-azanalysisservicesserver)|繼續伺服器執行個體。|  
|[Suspend-AzAnalysisServicesServer](/powershell/module/az.analysisservices/suspend-azanalysisservicesserver)|暫停伺服器執行個體。| 
|[Set-AzAnalysisServicesServer](/powershell/module/az.analysisservices/set-azanalysisservicesserver)|修改伺服器執行個體。|   
|[Test-AzAnalysisServicesServer](/powershell/module/az.analysisservices/test-azanalysisservicesserver)|測試伺服器執行個體的存在。| 

## <a name="server-management-operations"></a>伺服器管理作業

模組 - [Azure.AnalysisServices](https://www.powershellgallery.com/packages/Azure.AnalysisServices)

|Cmdlet|描述| 
|------------|-----------------| 
|[Add-AzAnalysisServicesAccount](/powershell/module/az.analysisservices/add-AzAnalysisServicesaccount)|新增已驗證的帳戶以用於 Azure Analysis Services 伺服器 Cmdlet 要求。| 
|[Export-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/export-AzAnalysisServicesinstancelog)|從目前登入環境新增 AzAnalysisServicesAccount 命令中所指定的 Analysis Services 伺服器執行個體匯出記錄檔|  
|[Restart-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|目前已登入的環境中，以重新啟動 Analysis Services 伺服器執行個體新增 AzAnalysisServicesAccount 命令中指定。|  
|[Sync-AzAnalysisServicesInstance](/powershell/module/az.analysisservices/restart-AzAnalysisServicesinstance)|同步處理到所有的查詢向外延展執行個體目前已登入環境新增 AzAnalysisServicesAccount 命令中所指定的 Analysis Services 伺服器指定的執行個體上指定的資料庫|  

## <a name="database-operations"></a>資料庫作業

Azure Analysis Services 資料庫作業會使用與 SQL Server Analysis Services 相同的 [SqlServer 模組](https://www.powershellgallery.com/packages/SqlServer)。 不過，Azure Analysis Services 並不支援所有的 Cmdlet。 若要深入了解，請參閱 [SQL Server PowerShell](https://docs.microsoft.com/sql/powershell/sql-server-powershell)。

SqlServer 模組提供特定工作的資料庫管理 Cmdlet，以及接受「表格式模型指令碼語言」(TMSL) 查詢或指令碼的一般用途 Invoke-ASCmd Cmdlet。 Azure Analysis Services 支援 SqlServer 模組中的下列 Cmdlet。

  
|Cmdlet|描述|
|------------|-----------------| 
|[Add-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/Add-RoleMember)|將成員新增到資料庫角色。| 
|[Backup-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/backup-asdatabase)|備份 Analysis Services 資料庫。|  
|[Remove-RoleMember](https://docs.microsoft.com/powershell/module/sqlserver/remove-rolemember)|從資料庫角色移除成員。|   
|[Invoke-ASCmd](https://docs.microsoft.com/powershell/module/sqlserver/invoke-ascmd)|执行 TMSL 脚本。|
|[Invoke-ProcessASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processasdatabase)|處理資料庫。|  
|[Invoke-ProcessPartition](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processpartition)|處理資料分割。| 
|[Invoke-ProcessTable](https://docs.microsoft.com/powershell/module/sqlserver/invoke-processtable)|處理資料表。|  
|[Merge-Partition](https://docs.microsoft.com/powershell/module/sqlserver/merge-partition)|合併資料分割。|  
|[Restore-ASDatabase](https://docs.microsoft.com/powershell/module/sqlserver/restore-asdatabase)|還原 Analysis Services 資料庫。| 
  

## <a name="related-information"></a>相關資訊

* [下載 SQL Server PowerShell 模組](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [下載 SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell 資源庫中的 SqlServer 模組](https://www.powershellgallery.com/packages/SqlServer)    
* [相容性層級 1200 和更高層級的表格式模型程式設計](https://msdn.microsoft.com/library/mt712541.aspx)

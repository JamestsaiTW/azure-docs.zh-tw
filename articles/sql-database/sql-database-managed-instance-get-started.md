---
title: Azure 入口網站：建立 SQL 受控執行個體 | Microsoft Docs
description: 建立 SQL 受控執行個體、網路環境，以及用於存取的用戶端 VM。
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: Carlrab
manager: craigg
ms.date: 02/18/2019
ms.openlocfilehash: 481c82eb74bcf80c3d0546324009ec0bf6495cfb
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2019
ms.locfileid: "56587050"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>快速入門：建立 Azure SQL Database 受控執行個體

本快速入門逐步解說如何在 Azure 入口網站中建立 Azure SQL Database [受控執行個體](sql-database-managed-instance.md)。

如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="sign-in-to-the-azure-portal"></a>登入 Azure 入口網站

登入 [Azure 入口網站](https://portal.azure.com/)。

## <a name="create-a-managed-instance"></a>建立受控執行個體

下列步驟說明如何建立受控執行個體。

1. 選擇 Azure 入口網站左上角的 [建立資源]。
2. 找出 [受控執行個體]，然後選取 [Azure SQL 受控執行個體]。
3. 選取 [建立] 。

   ![建立受控執行個體](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. 使用下表中的資訊，填妥 **SQL 受控執行個體**表單中所要求的資訊：

   | 設定| 建議的值 | 說明 |
   | ------ | --------------- | ----------- |
   | **訂用帳戶** | 您的訂用帳戶 | 您有權限在其中建立新資源的訂用帳戶 |
   |**受控執行個體名稱**|任何有效名稱|如需有效的名稱，請參閱[命名規則和限制](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)。|
   |**受控執行個體系統管理員登入**|任何有效的使用者名稱|如需有效的名稱，請參閱[命名規則和限制](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)。 請勿使用 "serveradmin"，因為這是保留的伺服器層級角色。|
   |**密碼**|任何有效密碼|密碼長度至少必須有 16 個字元，而且符合[定義的複雜度需求](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm)。|
   |**定序**|您要用於受控執行個體的定序|如果您從 SQL Server 遷移資料庫，請使用 `SELECT SERVERPROPERTY(N'Collation')` 來檢查來源定序並使用該值。 如需定序的相關資訊，請參閱[伺服器層級的定序](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation)。|
   |**位置**|要在其中建立受控執行個體的位置|如需有關區域的資訊，請參閱 [Azure 區域](https://azure.microsoft.com/regions/)。|
   |**虛擬網路**|選取 [建立新的虛擬網路] 或有效的虛擬網路與子網路。| 如果網路/子網路呈現灰色，您必須先[修改成符合網路需求](sql-database-managed-instance-configure-vnet-subnet.md)後，才能選取它作為新的受控執行個體目標。 如需關於設定受控執行個體網路環境的需求資訊，請參閱[設定受控執行個體的 VNet](sql-database-managed-instance-connectivity-architecture.md)。 |
   |**資源群組**|新的或現有的資源群組|如需有效的資源群組名稱，請參閱[命名規則和限制](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)。|

   ![受控執行個體表單](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. 若要以受控執行個體作為次要執行個體容錯移轉群組，請選取 [簽出]，然後指定 DnsAzurePartner 受控執行個體。 這是預覽版功能，不會顯示在隨附的螢幕擷取畫面中。
6. 選取 [定價層] 來調整計算和儲存體資源的大小，以及檢閱定價層選項。 預設值為一般用途定價層，其具也 32 GB 記憶體和 16 個虛擬核心。
7. 使用滑桿或文字方塊來指定儲存體數量和虛擬核心數目。
8. 完成時，選擇 [套用] 以儲存您的選取項目。  
9. 選取 [建立] 以部署受控執行個體。
10. 選取 [通知] 圖示以檢視部署的狀態。

    ![受控執行個體部署進度](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. 選取 [部署正在進行中] 來開啟受控執行個體視窗，以進一步監視部署進度。

> [!IMPORTANT]
> 如果是子網路中的第一個執行個體，部署時間通常遠超過後續執行個體的部署時間。 請勿取消部署作業，因為它持續的時間超過您的預期。 在子網路中建立第二個受控執行個體只需要幾分鐘的時間。

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>檢閱資源並擷取完整伺服器名稱

順利完成部署後，請檢閱所建立的資源，並擷取完整伺服器名稱以供稍後的快速入門使用。

1. 開啟受控執行個體的資源群組，並檢視其在[建立受控執行個體](#create-a-managed-instance)快速入門中為您所建立的資源。

   ![受控執行個體資源](./media/sql-database-managed-instance-get-started/resources.png)

2. 選取路由表以檢閱為您建立的使用者定義路由 (UDR) 表。

   ![路由表](./media/sql-database-managed-instance-get-started/route-table.png)

3. 在路由表中，檢閱要從受控執行個體虛擬網路路由傳送流量的項目。 如果您要手動建立或設定路由表，您必須在路由表中建立這些項目。

   ![本機 MI 子網路的項目](./media/sql-database-managed-instance-get-started/udr.png)

4. 回到資源群組，然後選取網路安全性群組以檢閱安全性規則。

   ![網路安全性群組](./media/sql-database-managed-instance-get-started/network-security-group.png)

5. 檢閱輸入和輸出安全性規則。

   ![安全性規則](./media/sql-database-managed-instance-get-started/security-rules.png)

6. 回到資源群組，然後選取您的受控執行個體。

   ![受控執行個體](./media/sql-database-managed-instance-get-started/managed-instance.png)

7. 在 [概觀] 索引標籤上，找出 [主機] 屬性並複製受控執行個體的完整主機位址，以便在下一個快速入門中使用。

   ![主機名稱](./media/sql-database-managed-instance-get-started/host-name.png)

   此名稱類似於 **your_machine_name.a1b2c3d4e5f6.database.windows.net**。

## <a name="next-steps"></a>後續步驟

- 若要了解如何連線至受控執行個體，請參閱：
  - 如需有關應用程式連線選項的概觀，請參閱[將您的應用程式連線至受控執行個體](sql-database-managed-instance-connect-app.md)。
  - 如需說明如何從 Azure 虛擬機器連線至受控執行個體的快速入門，請參閱[設定 Azure 虛擬機器連線](sql-database-managed-instance-configure-vm.md)。
  - 如需說明如何使用點對站連線從內部部署用戶端電腦連線至受控執行個體的快速入門，請參閱[設定點對站連線](sql-database-managed-instance-configure-p2s.md)。
- 若要將現有 SQL Server 資料庫從內部部署還原到受控執行個體，您可以使用 [Azure 資料庫移轉服務 (DMS) 來進行移轉](../dms/tutorial-sql-server-to-managed-instance.md)以從資料庫備份檔案還原，或使用 [T-SQL RESTORE 命令](sql-database-managed-instance-get-started-restore.md)以從資料庫備份檔案還原。
- 若要使用內建的疑難排解智慧對受控執行個體的資料庫效能進行進階監視，請參閱[使用 Azure SQL 分析監視 Azure SQL Database](../azure-monitor/insights/azure-sql.md)

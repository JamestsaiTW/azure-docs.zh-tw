---
title: 教學課程：使用 Azure 資料庫移轉服務在線上狀態下將 MongoDB 移轉至 Azure Cosmos DB 的 Mongo 版 API | Microsoft Docs
description: 了解如何使用 Azure 資料庫移轉服務，在線上狀態下從內部部署 MongoDB 移轉至 Azure Cosmos DB 的 Mongo 版 API。
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 02/27/2019
ms.openlocfilehash: 06e76b8eed283c6ef09f38e876c60b05477cf0ce
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985813"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-online-using-dms-preview"></a>教學課程：使用 DMS 在線上狀態下將 MongoDB 移轉至 Azure Cosmos DB 的 Mongo 版 API (預覽)
您可以使用 Azure 資料庫移轉服務，在線上狀態下 (以最短的停機時間) 將資料庫從內部部署或雲端的 MongoDB 執行個體移轉至 Azure Cosmos DB 的 Mongo 版 API。

在本教學課程中，您了解如何：
> [!div class="checklist"]
> * 建立 Azure 資料庫移轉服務的執行個體。
> * 使用 Azure 資料庫移轉服務來建立移轉專案。
> * 執行移轉。
> * 監視移轉。
> * 在您準備就緒後完成移轉。

在本教學課程中，您會使用 Azure 資料庫移轉服務，以最短的停機時間將資料集從裝載在 Azure 虛擬機器中的 MongoDB 移轉至 Azure Cosmos DB 的 Mongo 版 API。 如果您尚未設定 MongoDB 來源，請參閱[在 Azure 中的 Windows VM 上安裝及設定 MongoDB](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb) 一文。

> [!NOTE]
> 若要使用「Azure 資料庫移轉服務」來執行線上移轉，必須根據「進階」定價層建立執行個體。

> [!IMPORTANT]
> 為了獲得最佳的移轉體驗，Microsoft 建議在目標資料庫所在的同一個 Azure 區域中，建立 Azure 資料庫移轉服務的執行個體。 跨區域或地理位置移動資料可能使移轉程序變慢。

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

本文說明如何在線上狀態下從 MongoDB 移轉至 Azure Cosmos DB 的 Mongo 版 API。 使用離線移轉的相關資訊，請參閱[使用 DMS 在離線狀態下將 MongoDB 移轉至 Azure Cosmos DB 的 Mongo 版 API](tutorial-mongodb-cosmos-db.md)。

## <a name="prerequisites"></a>必要條件
若要完成本教學課程，您需要：
- [建立 Azure Cosmos DB 的 Mongo 版 API 帳戶](https://ms.portal.azure.com/#create/Microsoft.DocumentDB)。
- 使用 Azure Resource Manager 部署模型建立 Azure 資料庫移轉服務的 Azure 虛擬網路 (VNET)，以使用 [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) 或 [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) 為您的內部部署來源伺服器提供站對站連線能力。
- 確定您的 VNET 網路安全性群組規則不會封鎖下列通訊埠：443、53、9354、445 及 12000。 如需 Azure VNET NSG 流量篩選的詳細資訊，請參閱[使用網路安全性群組來篩選網路流量](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)。
- 變更來源伺服器的防火牆，以允許 Azure 資料庫移轉服務存取來源 MongoDB 伺服器 (依預設會使用 TCP 連接埠 27017)。
- 使用來源資料庫前面的防火牆應用裝置時，您可能必須新增防火牆規則，才能讓 Azure 資料庫移轉服務存取來源資料庫，以進行移轉。

## <a name="register-the-microsoftdatamigration-resource-provider"></a>註冊 Microsoft.DataMigration 資源提供者
1. 登入 Azure 入口網站，選取 [所有服務]，然後選取 [訂用帳戶]。

   ![顯示入口網站訂用帳戶](media/tutorial-mongodb-to-cosmosdb-online/portal-select-subscription1.png)
       
2. 選取您要在其中建立 Azure 資料庫移轉服務執行個體的訂用帳戶，然後選取 [資源提供者]。
 
    ![顯示資源提供者](media/tutorial-mongodb-to-cosmosdb-online/portal-select-resource-provider.png)
    
3.  搜尋移轉，然後在 [Microsoft.DataMigration] 的右邊，選取 [註冊]。
 
    ![註冊資源提供者](media/tutorial-mongodb-to-cosmosdb-online/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>建立執行個體
1.  在 Azure 入口網站中，選取 [+ 建立資源]，搜尋「Azure 資料庫移轉服務」，然後從下拉式清單選取 [Azure 資料庫移轉服務]。

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb-online/portal-marketplace.png)

2.  在 [Azure 資料庫移轉服務] 畫面上，選取 [建立]。
 
    ![建立 Azure 資料庫移轉服務執行個體](media/tutorial-mongodb-to-cosmosdb-online/dms-create1.png)
  
3.  在 [建立移轉服務] 畫面上，指定服務的名稱、訂用帳戶，以及新的或現有的資源群組。

4. 選取您要在其中建立 Azure 資料庫移轉服務執行個體的位置。 

5. 選取現有的虛擬網路 (VNET) 或建立新的虛擬網路。

    VNET 會為 Azure 資料庫移轉服務提供來源 MongoDB 執行個體和目標 Azure Cosmos DB 帳戶的存取權。

    如需有關如何在 Azure 入口網站中建立 VNET 的詳細資訊，請參閱[使用 Azure 入口網站建立虛擬網路](https://aka.ms/DMSVnet)一文。

6. 從「進階」定價層選取 SKU。

    > [!NOTE]
    > 只有在使用「進階」層的情況下，才支援線上移轉。 如需成本和定價層的詳細資訊，請參閱[定價分頁](https://aka.ms/dms-pricing)。

    如果您需要選擇適當 Azure 資料庫移轉服務層的相關協助，請在[這裡](https://go.microsoft.com/fwlink/?linkid=861067)參閱部落格文章內的建議。  

     ![設定 Azure 資料庫移轉服務執行個體設定](media/tutorial-mongodb-to-cosmosdb-online/dms-settings3.png)

7.  選取 [建立] 以建立服務。

## <a name="create-a-migration-project"></a>建立移轉專案
建立服務之後，請在 Azure 入口網站中找出該服務，然後建立新的移轉專案。

1. 在 Azure 入口網站中，選取 [所有服務]，搜尋 Azure 資料庫移轉服務，然後選取 [Azure 資料庫移轉服務]。
 
    ![找出 Azure 資料庫移轉服務的所有執行個體](media/tutorial-mongodb-to-cosmosdb-online/dms-search.png)

2. 在 [Azure 資料庫移轉服務] 畫面上，搜尋您建立的 Azure 資料庫移轉服務執行個體名稱，然後選取該執行個體。

    或者，您可以從 Azure 入口網站中的搜尋窗格探索 Azure 資料庫移轉服務執行個體。

    ![使用 Azure 入口網站中的搜尋窗格](media/tutorial-mongodb-to-cosmosdb-online/dms-search-portal.png)

3. 選取 [+ 新增移轉專案]。

4. 在 [新增移轉專案] 畫面上指定專案名稱、在 [來源伺服器類型] 文字方塊中選取 [MongoDB]、在 [目標伺服器類型] 文字方塊中選取 [CosmosDB (MongoDB API)]，然後針對 [選擇活動類型]，選取 [線上資料移轉 (預覽)]。

    ![建立資料庫移轉服務專案](media/tutorial-mongodb-to-cosmosdb-online/dms-create-project1.png)

5.  選取 [儲存]，然後選取 [建立及執行活動]，以建立專案並執行移轉活動。

## <a name="specify-source-details"></a>指定來源詳細資料
1. 在 [來源詳細資料] 畫面上，指定來源 MongoDB 伺服器的連線詳細資料。

    有三種模式可連線至來源：
       * **標準模式**，可接受完整網域名稱或 IP 位址、連接埠號碼和連線認證。
       * **連接字串模式**，可接受[連接字串 URI 格式](https://docs.mongodb.com/manual/reference/connection-string/)一文中說明的 MongoDB 連接字串。
       * **來自 Azure 儲存體的資料**，可接受 Blob 容器 SAS URL。 如果 Blob 容器含有 MongoDB [bsondump 工具](https://docs.mongodb.com/manual/reference/program/bsondump/)所產生的 BSON 傾印，請選取 [Blob 包含 BSON 傾印]，如果容器包含 JSON 檔案，則將其取消選取。

      如果您選取此選項，請確定儲存體帳戶連接字串以下列格式顯示：

    ```
    https://blobnameurl/container?SASKEY
    ```
      此外，根據 Azure 儲存體中的類型傾印資訊，請留意下列詳細資料。

      * 就 BSON 傾印而言，Blob 容器內的資料必須採用 bsondump 格式，使資料檔案以 collection.bson 的格式放入依所屬資料庫命名的資料夾中。 中繼資料檔案 (如果有的話) 則應使用 *collection*.metadata.json 的格式命名。

      * 就 JSON 傾印而言，Blob 容器中的檔案必須放入依所屬資料庫命名的資料夾中。 在每個資料庫資料夾中，資料檔案必須放在名為「資料」的子資料夾中，並使用 *collection*.json 的格式命名。 中繼資料檔案 (如果有的話) 必須放在名為「中繼資料」的子資料夾中，並使用相同的格式 *collection*.json 命名。 中繼資料檔案必須採用 MongoDB bsondump 工具所產生的相同格式。

   如果無法解析 DNS 名稱，您可以使用 IP 位址。

   ![指定來源詳細資料](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-source1.png)

2. 選取 [ **儲存**]。

   > [!NOTE]
   > 如果來源是複本集，則來源伺服器位址應為主要伺服器的位址；如果來源是分區化 MongoDB 叢集，則應為路由器的位址。 如果是分區化 MongoDB 叢集，則 Azure 資料庫移轉服務必須能夠連線至叢集中的個別分區，而這可能需要在更多機器上開啟防火牆。          

## <a name="specify-target-details"></a>指定目標詳細資料
1. 在 [移轉目標詳細資料] 畫面上，對目標 Azure Cosmos DB 帳戶指定連線詳細資料，此帳戶就是要作為 MongoDB 資料遷移目的地的預先佈建 Azure Cosmos DB Mongo 版 API 帳戶。

    ![指定目標詳細資料](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-target1.png)

2. 選取 [ **儲存**]。

## <a name="map-to-target-databases"></a>對應到目標資料庫
1. 在 [Map to target databases] \(對應到目標資料庫\) 畫面上，對應要進行移轉的來源資料庫和目標資料庫。

    如果目標資料庫包含與來源資料庫相同的資料庫名稱，Azure 資料庫移轉服務依預設會選取目標資料庫。

    如果字串 **Create** 出現在資料庫名稱旁邊，則代表 Azure 資料庫移轉服務未找到目標資料庫，且服務會為您建立該資料庫。

    這個移轉階段中，如果資料庫上需要共用輸送量，請指定輸送量 RU。 在 Cosmos DB 中，您可以在資料庫層級或以個別進行的方式，為每個集合佈建輸送量。 輸送量會以[要求單位](https://docs.microsoft.com/azure/cosmos-db/request-units) (RU) 來測量。 深入了解 [Azure Cosmos DB 定價](https://azure.microsoft.com/pricing/details/cosmos-db/)。

    ![對應到目標資料庫](media/tutorial-mongodb-to-cosmosdb-online/dms-map-target-databases1.png)

2. 選取 [ **儲存**]。

3. 在 [集合設定] 畫面上，展開集合清單，然後檢閱要遷移的集合清單。

    請注意，Azure 資料庫移轉服務會自動選取所有存在於來源 MongoDB 執行個體上，卻不存在於目標 Azure Cosmos DB 帳戶上的集合。 如果您想要重新移轉已包含資料的集合，就必須在此畫面上明確地選取集合。

    您可以指定要讓集合使用的 RU 數目。 在大部分情況下，500 (分區化集合的最小值為 1000) 到 4000 之間的值即應足夠。 Azure 資料庫移轉服務會根據集合大小來建議智慧的預設值。

    您也可以指定分區索引鍵以便利用 [Azure Cosmos DB 中的資料分割](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview)，從而獲得最佳延展性。 請務必檢閱[選取分區/資料分割索引鍵的最佳做法](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey)。 如果您沒有分割區索引鍵，您一律可以使用 **_id** 作為分區索引鍵，以達到更理想的輸送量。

    ![選取集合資料表](media/tutorial-mongodb-to-cosmosdb-online/dms-collection-setting1.png)

4. 選取 [ **儲存**]。

5. 在 [移轉摘要] 畫面上的 [活動名稱] 文字方塊中，指定移轉活動的名稱。

    ![移轉摘要](media/tutorial-mongodb-to-cosmosdb-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>執行移轉
- 選取 [執行移轉]。

   [移轉活動] 視窗隨即出現，並顯示活動的 [狀態]。

   ![活動狀態](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>監視移轉
- 在移轉活動畫面上選取 [重新整理] 以更新顯示，直到移轉的 [狀態] 顯示為 [正在重新執行] 為止。

   > [!NOTE]
   > 您可以選取活動來取得資料庫層級和集合層級移轉計量的詳細資料。

   ![活動狀態正在重新執行](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-replaying.png)

## <a name="verify-data-in-cosmos-db"></a>確認 Cosmos DB 中的資料

1. 對來源 MongoDB 資料庫進行變更。
2. 請連線至 COSMOS DB，以確認資料是否複寫自來源 MongoDB 伺服器。

    ![活動狀態正在重新執行](media/tutorial-mongodb-to-cosmosdb-online/dms-verify-data.png)
 
## <a name="complete-the-migration"></a>完成移轉

* 當來源中的所有文件都出現 COSMOS DB 目標上之後，請從移轉活動的操作功能表中選取 [完成]，以完成移轉的。

    此動作會完成所有暫止變更的重新執行，並完成移轉。

    ![活動狀態正在重新執行](media/tutorial-mongodb-to-cosmosdb-online/dms-finish-migration.png)

## <a name="additional-resources"></a>其他資源

 * [Cosmos DB 服務資訊](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>後續步驟
- 在 Microsoft [資料庫移轉指南](https://datamigration.microsoft.com/)中檢閱其他案例的移轉指導方針。
---
title: 教學課程：使用 Azure Databricks 執行 ETL 作業
description: 了解如何將資料從 Data Lake Storage Gen2 擷取至 Azure Databricks 並轉換資料，然後將資料載入 Azure SQL 資料倉儲。
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.workload: Active
ms.date: 02/15/2019
ms.openlocfilehash: 6ec32a40cea4f95d9225134cfb36d4930245d1c5
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/24/2019
ms.locfileid: "56750594"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>教學課程：使用 Azure Databrick 擷取、轉換和載入資料

在此教學課程中，您將使用 Azure Databricks 執行 ETL (擷取、轉換及載入資料) 作業。 您會將資料從 Azure Data Lake Storage Gen2 擷取至 Azure Databricks，並在 Azure Databricks 中執行資料轉換，然後將轉換後的資料載入 Azure SQL 資料倉儲。

本教學課程中的步驟會使用適用於 Azure Databricks 的 SQL 資料倉儲連接器，將資料轉送至 Azure Databricks。 接著，當資料在 Azure Databricks 叢集和 Azure SQL 資料倉儲之間進行轉換時，此連接器會使用 Azure Blob 儲存體作為暫時儲存體。

下圖顯示此應用程式流程：

![搭配 Data Lake Store 與 SQL 資料倉儲的 Azure Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "搭配 Data Lake Store 與 SQL 資料倉儲的 Azure Databricks")

本教學課程涵蓋下列工作：

> [!div class="checklist"]
> * 建立 Azure Databricks 服務。
> * 在 Azure Databricks 中建立 Spark 叢集。
> * 在 Data Lake Storage Gen2 帳戶中建立檔案系統。
> * 將範例資料上傳到 Azure Data Lake Storage Gen2 帳戶。
> * 建立服務主體。
> * 從 Azure Data Lake Storage Gen2 帳戶擷取資料。
> * 轉換 Azure Databricks 中的資料。
> * 將資料載入到 Azure SQL 資料倉儲。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

## <a name="prerequisites"></a>必要條件

開始進行本教學課程前，請先完成下列工作：

* 建立 Azure SQL 資料倉儲、建立伺服器層級的防火牆規則，並以伺服器管理員的身分連線至伺服器。請參閱[快速入門：建立 Azure SQL 資料倉儲](../sql-data-warehouse/create-data-warehouse-portal.md)。

* 建立 Azure SQL 資料倉儲的資料庫主要金鑰。 請參閱[建立資料庫主要金鑰](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key)。

* 建立 Azure Blob 儲存體帳戶和其中所含的容器。 此外，擷取用來存取儲存體帳戶的存取金鑰。 請參閱[快速入門：建立 Azure Blob 儲存體帳戶](../storage/blobs/storage-quickstart-blobs-portal.md)的指示操作。

* 建立 Azure Data Lake Storage Gen2 儲存體帳戶。 請參閱[建立 Azure Data Lake Storage Gen2 帳戶](../storage/blobs/data-lake-storage-quickstart-create-account.md)。

*  建立服務主體。 請參閱[如何：使用入口網站來建立可存取資源的 Azure AD 應用程式和服務主體](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)。

   在執行該文章中的步驟時，您必須執行幾個特定動作。

   * 在執行該文章的[將應用程式指派給角色](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role)一節中的步驟時，請確實將 [儲存體 Blob 資料參與者] 角色指派給服務主體。

     > [!IMPORTANT]
     > 請務必在 Data Lake Storage Gen2 儲存體帳戶的範圍中指派該角色。 您可以將角色指派給父資源群組或訂用帳戶，但在這些角色指派傳播至儲存體帳戶之前，您將會收到與權限有關的錯誤。

   * 在執行該文章的[取得值以便登入](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)一節中的步驟時，請將租用戶識別碼、應用程式識別碼和驗證金鑰值貼到文字檔中。 您很快就會用到這些資料。

* 登入 [Azure 入口網站](https://portal.azure.com/)。

## <a name="gather-the-information-that-you-need"></a>收集您需要的資訊

請確定您已完成本教學課程的必要條件。

   在開始之前，您應該有這幾項資訊：

   :heavy_check_mark:您的 Azure SQL 資料倉儲的資料庫名稱、資料庫伺服器名稱、使用者名稱和密碼。

   :heavy_check_mark:您的 Blob 儲存體帳戶的存取金鑰。

   :heavy_check_mark:您的 Data Lake Storage Gen2 儲存體帳戶的名稱。

   :heavy_check_mark:您的訂用帳戶的租用戶識別碼。

   :heavy_check_mark:您向 Azure Active Directory (Azure AD) 註冊的應用程式的應用程式識別碼。

   :heavy_check_mark:您向 Azure AD 註冊的應用程式的驗證金鑰。

## <a name="create-an-azure-databricks-service"></a>建立 Azure Databricks 服務

在本節中，您將使用 Azure 入口網站建立 Azure Databricks 服務。

1. 在 Azure 入口網站中，選取 [建立資源] > [分析] > [Azure Databricks]。

    ![Azure 入口網站上的 Databricks](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png "Azure 入口網站上的 Databricks")

2. 在 [Azure Databricks 服務] 下方提供下列值，以建立 Databricks 服務：

    |屬性  |說明  |
    |---------|---------|
    |**工作區名稱**     | 提供您 Databricks 工作區的名稱。        |
    |**訂用帳戶**     | 從下拉式清單中選取您的 Azure 訂用帳戶。        |
    |**資源群組**     | 指定您是要建立新的資源群組，還是使用現有資源群組。 資源群組是存放 Azure 方案相關資源的容器。 如需詳細資訊，請參閱 [Azure 資源群組概觀](../azure-resource-manager/resource-group-overview.md)。 |
    |**位置**     | 選取 [美國西部 2]。  如需其他可用的區域，請參閱[依區域提供的 Azure 服務](https://azure.microsoft.com/regions/services/)。      |
    |定價層     |  選取 [標準]。     |

3. 選取 [釘選到儀表板]，然後選取 [建立]。

4. 建立帳戶需要幾分鐘的時間。 建立帳戶期間，入口網站右側會顯示 [提交 Azure Databricks 部署] 圖格。 若要監視作業狀態，請檢視頂端的進度列。

    ![Databricks 部署圖格](./media/databricks-extract-load-sql-data-warehouse/databricks-deployment-tile.png "Databricks 部署圖格")

## <a name="create-a-spark-cluster-in-azure-databricks"></a>在 Azure Databricks 中建立 Spark 叢集

1. 在 Azure 入口網站中，移至您所建立的 Databricks 服務，然後選取 [啟動工作區]。

2. 系統會將您重新導向至 Azure Databricks 入口網站。 在入口網站中選取 [叢集]。

    ![Azure 上的 Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Azure 上的 Databricks")

3. 在 [新增叢集] 頁面上，提供值以建立叢集。

    ![在 Azure 上建立 Databricks Spark 叢集](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "在 Azure 上建立 Databricks Spark 叢集")

4. 填寫下列欄位的值，然後接受其他欄位的預設值：

    * 輸入叢集的名稱。

    * 在本文中，請使用 **5.1** 執行階段建立叢集。

    * 確定您已選取 [在停止活動\_\_分鐘後終止] 核取方塊。 若未使用叢集，請提供據以終止叢集的持續時間 (以分鐘為單位)。

    * 選取 [建立叢集]。 叢集執行後，您就可以將 Notebook 連結至叢集，並執行 Spark 作業。

## <a name="create-a-file-system-in-the-azure-data-lake-storage-gen2-account"></a>在 Azure Data Lake Storage Gen2 帳戶中建立檔案系統

在本節中，您會在 Azure Databricks 工作區中建立 Notebook，然後執行程式碼片段以設定儲存體帳戶

1. 在 [Azure 入口網站](https://portal.azure.com)中，移至您所建立的 Azure Databricks 服務，然後選取 [啟動工作區]。

2. 在左側選取 [工作區]。 從 [工作區] 下拉式清單選取 [建立] > [Notebook]。

    ![在 Databricks 中建立 Notebook](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "在 Databricks 中建立 Notebook")

3. 在 [建立 Notebook] 對話方塊中，輸入 Notebook 的名稱。 選取 [Scala] 作為語言，然後選取您先前建立的 Spark 叢集。

    ![為 Databricks 中的 Notebook 提供詳細資料](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "為 Databricks 中的 Notebook 提供詳細資料")

4. 選取 [建立] 。

5. 複製以下程式碼區塊並貼到第一個資料格中。

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<application-id>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<authentication-key>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
   ```

6. 在此程式碼區塊中，請將此程式碼區塊中的 `application-id`、`authentication-id`、`tenant-id` 和 `storage-account-name` 預留位置值取代為您在執行本教學課程的必要條件時所收集到的值。 請將 `file-system-name` 預留位置值取代為您要為檔案系統指定的任何名稱。

   * `application-id` 和 `authentication-id` 來自於您在建立服務主體時向 Active Directory 註冊的應用程式。

   * `tenant-id` 來自於您的訂用帳戶。

   * `storage-account-name` 是您 Azure Data Lake Storage Gen2 儲存體帳戶的名稱。

7. 按 **SHIFT + ENTER** 鍵以執行此區塊中的程式碼。

## <a name="ingest-sample-data-into-the-azure-data-lake-storage-gen2-account"></a>在 Azure Data Lake Storage Gen2 帳戶中內嵌範例資料

開始本節之前，您必須先完成下列必要條件：

在 Notebook 資料格中輸入下列程式碼：

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

在資料格中按 **SHIFT + ENTER** 執行程式碼。

現在，在此資料格下方的新資料格中輸入下列程式碼，並將括號中的值取代為您稍早使用的相同值：

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://<file-system>@<account-name>.dfs.core.windows.net/")

在資料格中按 **SHIFT + ENTER** 執行程式碼。

## <a name="extract-data-from-the-azure-data-lake-storage-gen2-account"></a>從 Azure Data Lake Storage Gen2 帳戶擷取資料

1. 您現在可以將 json 檔案範例以資料框架的形式載入 Azure Databricks。 在新的資料格中貼上下列程式碼。 請將括號內顯示的預留位置取代為您自己的值。

   ```scala
   val df = spark.read.json("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/small_radio_json.json")
   ```

   * 使用您在 [儲存體總管] 中為檔案系統提供的名稱取代 `file-system-name` 預留位置值。

   * 使用您的儲存體帳戶名稱取代 `storage-account-name` 預留位置。

2. 按 **SHIFT + ENTER** 鍵以執行此區塊中的程式碼。

3. 執行下列程式碼，以查看資料框架的內容：

    ```scala
    df.show()
    ```
   您會看到如下列程式碼片段的輸出：

   ```bash
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
   | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
   | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
   ...
   ...
   ```

   您現在已將資料從 Azure Data Lake Storage Gen2 擷取至 Azure Databricks。

## <a name="transform-data-in-azure-databricks"></a>轉換 Azure Databricks 中的資料

原始範例資料 **small_radio_json.json** 檔案擷取了廣播電台的聽眾，並且有不同資料行。 在本節中，您會將資料轉換為僅從資料集擷取特定資料行。

1. 首先，您僅從已建立的資料框架中擷取 **firstName**、**lastName**、**gender**、**location** 和 **level** 資料行。

   ```scala
   val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
   specificColumnsDf.show()
   ```

   您會取得如下列程式碼片段所示的輸出：

   ```bash
   +---------+----------+------+--------------------+-----+
   |firstname|  lastname|gender|            location|level|
   +---------+----------+------+--------------------+-----+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   +---------+----------+------+--------------------+-----+
   ```

2. 您可以進一步轉換此資料，將 **level** 資料行重新命名為 **subscription_type**。

   ```scala
   val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
   renamedColumnsDF.show()
   ```

   您會取得如下列程式碼片段所示的輸出。

   ```bash
   +---------+----------+------+--------------------+-----------------+
   |firstname|  lastname|gender|            location|subscription_type|
   +---------+----------+------+--------------------+-----------------+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   +---------+----------+------+--------------------+-----------------+
   ```

## <a name="load-data-into-azure-sql-data-warehouse"></a>將資料載入 Azure SQL 資料倉儲

在本節中，您會將已轉換的資料上傳至 Azure SQL 資料倉儲。 您可以使用適用於 Azure Databricks 的 Azure SQL 資料倉儲連接器，直接將資料框架以資料表形式上傳至 SQL 資料倉儲。

如前所述，在 Azure Databricks 和 Azure SQL 資料倉儲之間上傳資料時，SQL 資料倉儲連接器會使用 Azure Blob 儲存體作為暫時儲存體。 因此，一開始您需提供要連線到儲存體帳戶的組態。 您必須已建立屬於本文必要條件的帳戶。

1. 提供可從 Azure Databricks 存取 Azure 儲存體帳戶的組態。

   ```scala
   val blobStorage = "<blob-storage-account-name>.blob.core.windows.net"
   val blobContainer = "<blob-container-name>"
   val blobAccessKey =  "<access-key>"
   ```

2. 指定在 Azure Databricks 與 Azure SQL 資料倉儲之間移動資料時所要使用的暫存資料夾。

   ```scala
   val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"
   ```

3. 執行下列程式碼片段，儲存組態中的 Azure Blob 儲存體存取金鑰。 此動作可確保您無須在 Notebook 中以純文字保留存取金鑰。

   ```scala
   val acntInfo = "fs.azure.account.key."+ blobStorage
   sc.hadoopConfiguration.set(acntInfo, blobAccessKey)
   ```

4. 提供用來連線至 Azure SQL 資料倉儲執行個體的值。 您必須已建立屬於必要條件的 SQL 資料倉儲。

   ```scala
   //SQL Data Warehouse related settings
   val dwDatabase = "<database-name>"
   val dwServer = "<database-server-name>"
   val dwUser = "<user-name>"
   val dwPass = "<password>"
   val dwJdbcPort =  "1433"
   val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
   ```

5. 執行下列程式碼片段，將已轉換的資料框架 **renamedColumnsDF** 以資料表形式載入 SQL 資料倉儲中。 此程式碼片段會在 SQL 資料庫中建立名為 **SampleTable** 的資料表。

   ```scala
   spark.conf.set(
       "spark.sql.parquet.writeLegacyFormat",
       "true")

   renamedColumnsDF.write
       .format("com.databricks.spark.sqldw")
       .option("url", sqlDwUrlSmall) 
       .option("dbtable", "SampleTable")
       .option( "forward_spark_azure_storage_credentials","True")
       .option("tempdir", tempDir)
       .mode("overwrite")
       .save()
   ```

6. 連線至 SQL 資料庫，並確認您看到名為 **SampleTable** 的資料庫。

   ![確認範例資料表](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "確認範例資料表")

7. 執行 SELECT 查詢，以確認資料表的內容。 資料表中應包含與 **renamedColumnsDF** 資料框架相同的資料。

    ![確認範例資料表內容](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "確認範例資料表內容")

## <a name="clean-up-resources"></a>清除資源

完成本教學課程之後，您可以終止叢集。 請從 Azure Databricks 工作區的左側選取 [叢集]。 針對要終止的叢集，在 [動作] 下方指向省略符號 (...)，然後選取 [終止] 圖示。

![停止 Databricks 叢集](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "停止 Databricks 叢集")

如果您不手動終止叢集，叢集會自動停止，但前提是您已在建立叢集時選取 [在停止活動\_\_分鐘後終止] 核取方塊。 在這種情況下，叢集將會在停止運作達指定時間後自動停止。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立 Azure Databricks 服務
> * 在 Azure Databricks 中建立 Spark 叢集
> * 在 Azure Databricks 中建立 Notebook
> * 從 Data Lake Storage Gen2 帳戶擷取資料
> * 轉換 Azure Databricks 中的資料
> * 將資料載入 Azure SQL 資料倉儲

前往下一個教學課程，了解如何使用 Azure 事件中樞將即時資料串流到 Azure Databricks。

> [!div class="nextstepaction"]
>[使用事件中樞將資料串流至 Azure Databricks](databricks-stream-from-eventhubs.md)

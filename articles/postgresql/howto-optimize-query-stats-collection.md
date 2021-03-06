---
title: 在適用於 PostgreSQL 的 Azure 資料庫伺服器中最佳化查詢統計資料集合
description: 本文說明如在「適用於 PostgreSQL 的 Azure 資料庫」伺服器上最佳化查詢統計資料集合。
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 428a22f79008130448bef65f14322d6880b1b367
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814362"
---
# <a name="optimize-query-statistics-collection-on-an-azure-database-for-postgresql-server"></a>在適用於 PostgreSQL 的 Azure 資料庫伺服器中最佳化查詢統計資料集合 
本文說明如何在「適用於 PostgreSQL 的 Azure 資料庫」伺服器中最佳化查詢統計資料集合。

## <a name="use-pgstatsstatements"></a>使用 pg_stats_statements
**Pg_stat_statements** 是適用於 PostgreSQL 的 Azure 資料庫中依預設啟用的 PostgreSQL 擴充功能。 此擴充功能提供了相關機制，可對伺服器執行的所有 SQL 陳述式追蹤執行統計資料。 此模組會在每次執行查詢連結，而產生頗高的效能成本。 啟用 **pg_stat_statements** 會強制將查詢文字寫入磁碟上的檔案。

如果您有查詢文字很長的唯一查詢，或您未主動監視 **pg_stat_statements**，請停用 **pg_stat_statements**，以達到最佳效能。 若要這麼做，請將設定變更為 `pg_stat_statements.track = NONE`。

在某些客戶工作負載上，我們發現停用 **pg_stat_statements** 最多可提升 50% 的效能。 但停用 pg_stat_statements 的代價是無法對效能問題進行疑難排解。

若要設定 `pg_stat_statements.track = NONE`：

- 在 Azure 入口網站中移至 [PostgreSQL 資源管理頁面，然後選取伺服器參數刀鋒視窗](howto-configure-server-parameters-using-portal.md)。

  ![PostgreSQL 伺服器參數刀鋒視窗](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- 若使用 [Azure CLI](howto-configure-server-parameters-using-cli.md)，將 az postgres server configuration set 設為 `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`。

## <a name="use-the-query-store"></a>使用查詢存放區 
在適用於 PostgreSQL 的 Azure 資料庫中，[查詢存放區](concepts-query-store.md)功能提供更有效率的方法來追蹤查詢統計資料。 我們建議使用這項功能替代 pg_stats_statements。 

## <a name="next-steps"></a>後續步驟
考慮在 [Azure 入口網站](howto-configure-server-parameters-using-portal.md)中或使用 [Azure CLI](howto-configure-server-parameters-using-cli.md) 來設定 `pg_stat_statements.track = NONE`。

如需詳細資訊，請參閱 
- [查詢存放區使用案例](concepts-query-store-scenarios.md) 
- [查詢存放區最佳做法](concepts-query-store-best-practices.md) 

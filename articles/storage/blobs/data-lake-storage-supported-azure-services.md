---
title: 支援 Azure Data Lake Storage Gen2 的 Azure 服務 |Microsoft Docs
description: 瞭解哪些 Azure 服務與 Azure Data Lake Storage Gen2 整合
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: c3a5f3a984c95af400c9e0c1543e3c1883290668
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/03/2020
ms.locfileid: "89442951"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>支援 Azure Data Lake Storage Gen2 的 Azure 服務

您可以使用 Azure 服務來內嵌資料、執行分析，以及建立視覺標記法。 本文提供支援的 Azure 服務清單、洩漏其支援層級，並提供可協助您搭配 Azure Data Lake Storage Gen2 使用這些服務的文章連結。

## <a name="supported-azure-services"></a>支援的 Azure 服務

下表列出可搭配 Azure Data Lake Storage Gen2 使用的 Azure 服務。 當支援繼續擴充時，出現在這些資料表中的項目會隨著時間而改變。

> [!NOTE]
> 支援層級僅指 Data Lake Storage Gen 2 支援服務的方式。

|Azure 服務 |支援層級 |Azure AD |共用金鑰| 相關文章 |
|---------------|-------------------|---|---|---|
|Azure Data Factory|正式推出|是|是|[使用 Azure Data Factory 將資料載入 Azure Data Lake Storage Gen2 中](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|正式推出|是|是|[搭配使用 Azure Databricks](https://docs.azuredatabricks.net/data/data-sources/azure/azure-datalake-gen2.html) <br> [快速入門：使用 Azure Databricks 分析 Azure Data Lake Storage Gen2 中的資料](data-lake-storage-quickstart-create-databricks-account.md) <br>[教學課程：使用 Azure Databricks 擷取、轉換和載入資料](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse) (機器翻譯) <br>[教學課程：使用 Spark 以 Azure Databricks 存取 Data Lake Storage Gen2 資料](data-lake-storage-use-databricks-spark.md)|
|Azure 事件中樞|正式推出|否|是|[透過 Azure 事件中樞在 Azure Blob 儲存體或 Azure Data Lake Storage 中擷取事件](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|
|Azure Event Grid|正式推出|是|是|[教學課程：執行 data lake capture 模式以更新 Databricks Delta 資料表](data-lake-storage-events.md)|
|Azure Logic Apps|正式推出|否|是|[概觀 - 什麼是 Azure Logic Apps？](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)|
|Azure Machine Learning|正式推出|是|是|[存取 Azure 儲存體服務中的資料](https://docs.microsoft.com/azure/machine-learning/how-to-access-data)|
|Azure 串流分析|正式推出|是|是|[快速入門：使用 Azure 入口網站建立串流分析作業](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [輸出至 Azure Data Lake Gen2](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|資料箱|正式推出|否|是|[使用 Azure 資料箱將資料從內部部署 HDFS 存放區遷移至 Azure 儲存體](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |正式推出|是|是|[搭配 Azure HDInsight 叢集使用 Data Lake Storage Gen2](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br>[使用 HDFS CLI 搭配 Data Lake Storage Gen2](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[教學課程：在 Azure HDInsight 上使用 Apache Hive 來解壓縮、轉換和載入資料](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT 中樞 |正式推出|否|是|[使用 IoT 中樞訊息路由將裝置到雲端訊息傳送至不同的端點](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|
|Power BI|正式推出|是|是|[使用 Power BI 分析 Data Lake Storage Gen2 中的資料](https://docs.microsoft.com/power-query/connectors/datalakestorage)|
|Azure Synapse Analytics (先前稱為 SQL 資料倉儲)|正式推出|是|是|[搭配先前的 SQL 資料倉儲使用 Azure Synapse Analytics () ](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-sql-data-warehouse-polybase)|
|SQL Server Integration Services (SSIS)|正式推出|是|是|[Azure 儲存體連線管理員](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager?view=sql-server-2017)|
|Azure 資料總管|正式推出|是|是|[使用 Azure 資料總管在 Azure Data Lake 中查詢資料](https://docs.microsoft.com/azure/data-explorer/data-lake-query-data)|
|Azue 認知搜尋|預覽|是|是|[Azure Data Lake Storage Gen2 檔的索引和搜尋 (預覽) ](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|
|Azure 內容傳遞網路|尚不支援|不適用|不適用|[Azure Data Lake Storage Gen2 檔的索引和搜尋 (預覽) ](https://docs.microsoft.com/azure/cdn/cdn-overview)|

## <a name="see-also"></a>另請參閱

- [Azure Data Lake Storage Gen2 的已知問題](data-lake-storage-known-issues.md)
- [Azure Data Lake Storage Gen2 中可用的 Blob 儲存體功能](data-lake-storage-supported-blob-storage-features.md)
- [支援 Azure Data Lake Storage Gen2 的開放原始碼平台](data-lake-storage-supported-open-source-platforms.md)
- [Azure Data Lake Storage 上的多重通訊協定存取](data-lake-storage-multi-protocol-access.md)
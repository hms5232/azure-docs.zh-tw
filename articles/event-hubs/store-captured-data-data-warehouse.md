---
title: 教學課程：將事件資料遷移至 Azure Synapse Analytics - Azure 事件中樞
description: 教學課程：本教學課程說明如何使用由事件方格觸發的 Azure 函式，將資料從事件中樞擷取到 Azure Synapse Analytics 中。
services: event-hubs
ms.date: 06/23/2020
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.openlocfilehash: b2a35647422c91d6859e1889f906ae512ce41a56
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/03/2020
ms.locfileid: "89436607"
---
# <a name="tutorial-migrate-captured-event-hubs-data-to-azure-synapse-analytics-using-event-grid-and-azure-functions"></a>教學課程：使用事件方格和 Azure Functions 將擷取的事件中樞資料遷移至 Azure Synapse Analytics

要將事件中樞內的串流資料自動傳遞至 Azure Blob 儲存體或 Azure Data Lake Store，事件中樞[擷取](./event-hubs-capture-overview.md)是最簡單的方式。 接著，您將可處理這些資料，並將其傳遞至您所選擇的任何其他儲存體目的地，例如 Azure Synapse Analytics 或 Cosmos DB。 在本教學課程中，您將了解如何使用由[事件方格](../event-grid/overview.md)觸發的 Azure 函式，將資料從事件中樞擷取到 Azure Synapse Analytics 中。

![Visual Studio](./media/store-captured-data-data-warehouse/EventGridIntegrationOverview.PNG)

- 首先，您應建立已啟用**擷取**功能的事件中樞，並 Azure Blob 儲存體設定為目的地。 WindTurbineGenerator 所產生的資料會串流至事件中樞，並以 Avro 檔案的形式自動擷取到 Azure 儲存體中。
- 接下來，您應建立以事件中樞命名空間作為來源，並以 Azure 函式端點作為目的地的 Azure 事件格線訂用帳戶。
- 每當事件中樞擷取功能將新的 Avro 檔案傳遞至 Azure 儲存體 Blob 時，事件格線即會以 Blob URI 告知 Azure 函式。 接著，此函式會將資料從 Blob 遷移至 Azure Synapse Analytics。

在本教學課程中，您會執行下列動作：

> [!div class="checklist"]
>
> - 部署基礎結構
> - 將程式碼發佈至 Functions 應用程式
> - 從 Functions 應用程式建立事件格線訂用帳戶
> - 將範例資料串流至事件中樞。
> - 驗證 Azure Synapse Analytics 中擷取的資料

## <a name="prerequisites"></a>必要條件

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- [Visual Studio 2019](https://www.visualstudio.com/vs/)。 在安裝時，請確實安裝下列工作負載：.NET 桌面開發、Azure 開發、ASP.NET 和 Web 開發、Node.js 開發和 Python 開發
- 下載 [Git 範例](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/EventHubsCaptureEventGridDemo)。範例解決方案包含下列元件：

  - *WindTurbineDataGenerator* – 一個簡單的發行者，會將範例風力發電機資料傳送至已啟用擷取功能的事件中樞
  - *FunctionDWDumper* – 一個 Azure 函式，會在 Avro 檔案擷取至 Azure 儲存體 Blob 時收到事件格線通知。 其會接收 Blob 的 URI 路徑、讀取其內容，並將此資料推送至 Azure Synapse Analytics。

  此範例使用最新的 Azure.Messaging.EventHubs 套件。 您可以在[這裡](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo)找到使用 Microsoft.Azure.EventHubs 套件的舊範例。

### <a name="deploy-the-infrastructure"></a>部署基礎結構

使用 Azure PowerShell 或 Azure CLI，利用此 [Azure Resource Manager 範本](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json)部署本教學課程所需要的基礎結構。 此範本會建立下列資源：

- 已啟用擷取功能的事件中樞
- 已擷取的事件資料適用的儲存體帳戶
- 用來裝載 Functions 應用程式的 Azure App Service 方案
- 對已擷取的事件檔案進行處理的函式應用程式
- 用來裝載資料倉儲的邏輯 SQL 伺服器
- 用於儲存已遷移資料的 Azure Synapse Analytics

以下幾節將提供相關 Azure CLI 和 Azure PowerShell 命令，用以部署本教學課程所需的基礎結構。 執行這些命令之前，請先更新下列物件的名稱： 

- Azure 資源群組 
- 資源群組的區域
- 事件中樞命名空間
- 事件中樞
- 邏輯 SQL 伺服器
- SQL 使用者 (和密碼)
- Azure SQL 資料庫
- Azure 儲存體 
- Azure Functions 應用程式

這些指令碼需要一些時間才能建立所有的 Azure 成品。 請靜候指令碼完成，再繼續操作。 如果部署因故失敗，請刪除資源群組並修正回報的問題，然後再重新執行命令。 

#### <a name="azure-cli"></a>Azure CLI

若要使用 Azure CLI 部署範本，請使用下列命令：

```azurecli-interactive
az group create -l westus -n rgDataMigrationSample

az group deployment create `
  --resource-group rgDataMigrationSample `
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json `
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

#### <a name="azure-powershell"></a>Azure PowerShell
若要使用 PowerShell 部署範本，請使用下列命令：

```powershell
New-AzResourceGroup -Name rgDataMigration -Location westcentralus

New-AzResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
```

### <a name="create-a-table-in-azure-synapse-analytics"></a>在 Azure Synapse Analytics 中建立資料表

使用 [Visual Studio](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-query-visual-studio.md)、[SQL Server Management Studio](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-query-ssms.md) 或入口網站中的查詢編輯器執行 [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql) 指令碼，以在 Azure Synapse Analytics 中建立資料表。 

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-code-to-the-functions-app"></a>將程式碼發佈至 Functions 應用程式

1. 在 Visual Studio 2019 中開啟解決方案 EventHubsCaptureEventGridDemo.sln。

1. 在 [方案總管] 中，以滑鼠右鍵按一下 [FunctionEGDWDumper]，然後選取 [發佈]。

   ![發佈函數應用程式](./media/store-captured-data-data-warehouse/publish-function-app.png)

1. 選取 [Azure 函數應用程式] 和 [選取現有的]。 選取 [發佈] 。

   ![目標函數應用程式](./media/store-captured-data-data-warehouse/pick-target.png)

1. 選取透過範本部署的函數應用程式。 選取 [確定]。

   ![選取函數應用程式](./media/store-captured-data-data-warehouse/select-function-app.png)

1. 當 Visual Studio 完成設定檔設定時，選取 [發佈]。

   ![Select publish](./media/store-captured-data-data-warehouse/select-publish.png)

發佈函式之後，您即可訂閱來自事件中樞的擷取事件。


## <a name="create-an-event-grid-subscription-from-the-functions-app"></a>從 Functions 應用程式建立事件格線訂用帳戶
 
1. 移至 [Azure 入口網站](https://portal.azure.com/)。 選取您的資源群組和函數應用程式。

   ![檢視函數應用程式](./media/store-captured-data-data-warehouse/view-function-app.png)

1. 選取函式。

   ![選取函式](./media/store-captured-data-data-warehouse/select-function.png)

1. 選取 [新增事件方格訂用帳戶]。

   ![新增訂用帳戶](./media/store-captured-data-data-warehouse/add-event-grid-subscription.png)

1. 為事件方格訂用帳戶指定名稱。 使用 [事件中樞命名空間] 作為事件類型。 提供值以選取您的事件中樞命名空間執行個體。 將訂閱者端點保留為提供的值。 選取 [建立]。

   ![建立訂用帳戶](./media/store-captured-data-data-warehouse/set-subscription-values.png)

## <a name="generate-sample-data"></a>產生範例資料  
現在，您已設定事件中樞、Azure Synapse Analytics、Azure 函式應用程式和事件方格訂用帳戶。 您可以執行 WindTurbineDataGenerator.exe，以產生在原始程式碼中的事件中樞更新了連接字串和名稱之後會傳至事件中樞的資料流。 

1. 在入口網站中，選取您的事件中樞命名空間。 選取 [連接字串]。

   ![選取連接字串](./media/store-captured-data-data-warehouse/event-hub-connection.png)

2. 選取 [RootManageSharedAccessKey]

   ![選取索引鍵](./media/store-captured-data-data-warehouse/show-root-key.png)

3. 複製 [連接字串 - 主索引鍵]

   ![複製金鑰](./media/store-captured-data-data-warehouse/copy-key.png)

4. 返回 Visual Studio 專案。 在 *WindTurbineDataGenerator* 專案中，開啟 *program.cs*。

5. 以您事件中樞的連接字串和名稱更新 **EventHubConnectionString** 和 **EventHubName** 的值。 

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. 建置解決方案，然後執行 WindTurbineGenerator.exe 應用程式。 

## <a name="verify-captured-data-in-data-warehouse"></a>確認資料倉儲中已擷取的資料
在幾分鐘之後，查詢 Azure Synapse Analytics 中的資料表。 您會看到 WindTurbineDataGenerator 所產生的資料已串流至事件中樞、擷取到 Azure 儲存體容器中，然後由 Azure 函式遷移至 Azure Synapse Analytics 資料表中。  

## <a name="next-steps"></a>後續步驟 
您可以對資料倉儲使用功能強大的資料視覺效果工具，以產生付諸行動的深入解析。

本文說明如何使用 [Power BI 搭配 Azure Synapse Analytics](/power-bi/connect-data/service-azure-sql-data-warehouse-with-direct-connect)
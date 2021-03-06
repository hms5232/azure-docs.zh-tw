---
title: '分析和監視資料集上的資料漂移 (預覽) '
titleSuffix: Azure Machine Learning
description: 建立 Azure Machine Learning 資料集監視器 (預覽) 、監視資料集中的資料漂移，以及設定警示。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: sgilley
ms.author: copeters
author: lostmygithubaccount
ms.date: 06/25/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 4b2b435be2a39b6e31a7f44fa6acbe7e1bc9c2c0
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661678"
---
# <a name="detect-data-drift-preview-on-datasets"></a>偵測資料集的資料漂移 (預覽) 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

> [!IMPORTANT]
> 偵測資料集上的資料漂移目前處於公開預覽狀態。
> 此預覽版本會在沒有服務等級協定的情況下提供，不建議用於實際執行工作負載。 可能不支援特定功能，或可能已經限制功能。 如需詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

瞭解如何監視資料漂移，並設定漂移偏高的警示。  

使用 Azure Machine Learning 資料集監視器 (預覽) 時，您可以：
* **分析資料中的漂移** ，以瞭解其在一段時間內的變化。
* **監視模型資料** ，瞭解定型和服務資料集之間的差異。  [從從已部署的模型收集模型資料](how-to-enable-data-collection.md)開始。
* **監視新的資料** 是否有任何基準和目標資料集之間的差異。
* **分析資料中的功能** ，以追蹤統計屬性隨時間的變更。
* 針對潛在問題的早期警告，**設定資料漂移的警示**。 

使用 [Azure Machine learning 資料集](how-to-create-register-datasets.md) 來建立監視器。 資料集必須包含時間戳記資料行。

您可以使用 Python SDK 或 Azure Machine Learning studio 來查看資料漂移度量。  您可以透過與 Azure Machine Learning 工作區相關聯的 [Azure 應用程式 insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) 資源取得其他計量和見解。

> [!Important]
> 所有版本都提供監視 SDK 的資料漂移。 不過，透過 web 上的 studio 監視資料漂移僅限企業版。

## <a name="prerequisites"></a>必要條件

若要建立及使用資料集監視器，您需要：
* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請在開始前先建立免費帳戶。 立即試用[免費或付費版本的 Azure Machine Learning](https://aka.ms/AMLFree)。
* [Azure Machine Learning 工作區](how-to-manage-workspace.md)。
* [已安裝適用于 Python 的 AZURE MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true)，其中包含 azureml 資料集套件。
* 結構化 (表格式) 資料，並在資料的檔案路徑、檔案名或資料行中指定時間戳記。

## <a name="what-is-data-drift"></a>什麼是資料漂移？

資料漂移是一段時間內的最大原因之一，就是模型精確度會下降。  針對機器學習模型，資料漂移是模型輸入資料中導致模型效能降低的變更。  監視資料漂移有助於偵測這些模型效能問題。

資料漂移的原因包括：

- 上游程式變更（例如更換的感應器，會將度量單位從英寸變更為公分）。 
- 資料品質問題（例如，損毀的感應器）一律會讀取0。
- 資料中的自然漂移，例如以季節變化的平均溫度。
- 功能之間的變更或 covariate shift。 

Azure Machine Learning 藉由計算單一計量來簡化漂移偵測，以抽象化所比較的資料集複雜性。  這些資料集可能有數百個功能和數萬個數據列。 偵測到漂移之後，您可以向下切入到哪些功能造成漂移。  然後，您會檢查功能等級的度量，以進行偵測並找出漂移的根本原因。

這個由上而下的方法可讓您輕鬆地監視資料，而不是傳統的規則技術。 以規則為基礎的技術（例如允許的資料範圍或允許的唯一值）可能相當耗時且容易出錯。

在 Azure Machine Learning 中，您可以使用資料集監視器來偵測資料漂移，併發出警示。
  
### <a name="dataset-monitors"></a>資料集監視器 

使用資料集監視器，您可以：

* 偵測資料集內新資料的資料漂移併發出警示。
* 分析歷程記錄資料的漂移。
* 分析一段時間的新資料。

資料漂移演算法可提供資料變化的整體量值，並指出哪些功能會負責進一步調查。 資料集監視器會藉由分析資料集中的新資料來產生一些其他度量 `timeseries` 。 

您可以透過 [Azure 應用程式 Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)，在監視器產生的所有度量上設定自訂警示。 您可以使用資料集監視器來快速攔截資料問題，並藉由找出可能的原因，縮短偵測問題的時間。  

在概念上，在 Azure Machine Learning 中設定資料集監視器有三個主要案例。

案例 | 描述
---|---
監視模型的服務資料，以防止來自定型資料的漂移 | 此案例的結果可以針對模型的精確度，將其視為監視 proxy，因為當服務資料從定型資料偏離時，模型精確度會降低。
監視時間序列資料集，使其與上一段時間的漂移。 | 此案例更為一般，而且可用來監視模型建立之上游或下游相關的資料集。  目標資料集必須有時間戳記資料行。 基準資料集可以是具有與目標資料集共通之功能的任何表格式資料集。
對過去的資料執行分析。 | 您可以使用此案例來瞭解歷程記錄資料，並在資料集監視器的設定中通知決策。

資料集監視器相依于下列 Azure 服務。

|Azure 服務  |描述  |
|---------|---------|
| *資料集* | 漂移會使用 Machine Learning 資料集來取得定型資料，並比較資料以進行模型定型。  產生資料的設定檔是用來產生一些回報的度量，例如 min、max、相異值、相異值計數。 |
| *Azureml 管線和計算* | 漂移計算工作裝載于 azureml 管線中。  作業會視需要觸發，或依排程在漂移監視器建立時所設定的計算上執行。
| *Application insights*| 漂移會發出計量給屬於機器學習服務工作區的 Application Insights。
| *Azure Blob 儲存體*| 漂移會以 json 格式將計量發出至 Azure blob 儲存體。

## <a name="how-dataset-monitors-data"></a>資料集如何監視資料

使用 Machine Learning 資料集來監視資料漂移。 指定基準資料集-通常是模型的訓練資料集。 目標資料集-通常是模型輸入資料-會經過一段時間後再與您的基準資料集比較。 這種比較表示您的目標資料集必須指定時間戳記資料行。

## <a name="create-target-dataset"></a>建立目標資料集

目標資料集需要 `timeseries` 在其上設定特性，方法是從資料的資料行或從檔案路徑模式衍生的虛擬資料行指定時間戳記資料行。 透過 [PYTHON SDK](#sdk-dataset) 或 [Azure Machine Learning studio](#studio-dataset)建立具有時間戳記的資料集。 必須指定代表 "timestamp" 的資料行，才能將特性新增 `timeseries` 至資料集。 如果您的資料分割成具有時間資訊的資料夾結構，例如 ' {yyyy/MM/dd} '，請透過路徑模式設定建立虛擬資料行，並將其設定為「分割時間戳記」，以改善時間序列功能的重要性。

### <a name="python-sdk"></a><a name="sdk-dataset"></a>Python SDK

[`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#&preserve-view=truewith-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-)類別 [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#&preserve-view=truewith-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) 方法會定義資料集的時間戳記資料行。

```python 
from azureml.core import Workspace, Dataset, Datastore

# get workspace object
ws = Workspace.from_config()

# get datastore object 
dstore = Datastore.get(ws, 'your datastore name')

# specify datastore paths
dstore_paths = [(dstore, 'weather/*/*/*/*/data.parquet')]

# specify partition format
partition_format = 'weather/{state}/{date:yyyy/MM/dd}/data.parquet'

# create the Tabular dataset with 'state' and 'date' as virtual columns 
dset = Dataset.Tabular.from_parquet_files(path=dstore_paths, partition_format=partition_format)

# assign the timestamp attribute to a real or virtual column in the dataset
dset = dset.with_timestamp_columns('date')

# register the dataset as the target dataset
dset = dset.register(ws, 'target')
```

如需使用 `timeseries` 資料集特性的完整範例，請參閱 [範例筆記本](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb) 或 [資料集 SDK 檔](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#&preserve-view=truewith-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-)。

### <a name="azure-machine-learning-studio"></a><a name="studio-dataset"></a>Azure Machine Learning studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

如果您使用 Azure Machine Learning studio 建立資料集，請確定您的資料路徑包含時間戳記資訊、包含包含資料的所有子資料夾，以及設定分割區格式。

在下列範例中，會採用 *NoaaIsdFlorida/2019* 子資料夾下的所有資料，而資料分割格式會指定時間戳記的年、月和日。

[![資料分割格式](./media/how-to-monitor-datasets/partition-format.png)](media/how-to-monitor-datasets/partition-format-expand.png)

在 [ **架構** 設定] 中，從指定資料集的虛擬或實際資料行中指定時間戳記資料行：

:::image type="content" source="media/how-to-monitor-datasets/timestamp.png" alt-text="設定時間戳記":::

如果您的資料是依日期進行分割，就像這裡的情況一樣，您也可以指定 partition_timestamp。  這可讓您以更有效率的方式處理日期。

:::image type="content" source="media/how-to-monitor-datasets/timeseries-partitiontimestamp.png" alt-text="分割區時間戳記":::


## <a name="create-dataset-monitors"></a>建立資料集監視

建立資料集監視器來偵測新資料集的資料漂移併發出警示。  請使用 [PYTHON SDK](#sdk-monitor) 或 [Azure Machine Learning studio](#studio-monitor)。

### <a name="python-sdk"></a><a name="sdk-monitor"></a>Python SDK

如需完整的詳細資料，請參閱 [PYTHON SDK 參考檔中的資料漂移](/python/api/azureml-datadrift/azureml.datadrift) 。 

下列範例顯示如何使用 Python SDK 建立資料集監視

```python
from azureml.core import Workspace, Dataset
from azureml.datadrift import DataDriftDetector
from datetime import datetime

# get the workspace object
ws = Workspace.from_config()

# get the target dataset
dset = Dataset.get_by_name(ws, 'target')

# set the baseline dataset
baseline = target.time_before(datetime(2019, 2, 1))

# set up feature list
features = ['latitude', 'longitude', 'elevation', 'windAngle', 'windSpeed', 'temperature', 'snowDepth', 'stationName', 'countryOrRegion']

# set up data drift detector
monitor = DataDriftDetector.create_from_datasets(ws, 'drift-monitor', baseline, target, 
                                                      compute_target='cpu-cluster', 
                                                      frequency='Week', 
                                                      feature_list=None, 
                                                      drift_threshold=.6, 
                                                      latency=24)

# get data drift detector by name
monitor = DataDriftDetector.get_by_name(ws, 'drift-monitor')

# update data drift detector
monitor = monitor.update(feature_list=features)

# run a backfill for January through May
backfill1 = monitor.backfill(datetime(2019, 1, 1), datetime(2019, 5, 1))

# run a backfill for May through today
backfill1 = monitor.backfill(datetime(2019, 5, 1), datetime.today())

# disable the pipeline schedule for the data drift detector
monitor = monitor.disable_schedule()

# enable the pipeline schedule for the data drift detector
monitor = monitor.enable_schedule()
```

如需設定 `timeseries` 資料集和資料漂移偵測器的完整範例，請參閱我們的 [範例筆記本](https://aka.ms/datadrift-notebook)。

### <a name="azure-machine-learning-studio"></a><a name="studio-monitor"></a> Azure Machine Learning studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku-inline.md)]

若要在您的資料集監視器上設定警示，包含您要為其建立監視器之資料集的工作區必須具有 Enterprise edition 功能。

確認工作區功能之後，請流覽至 [studio 首頁](https://ml.azure.com) ，然後選取左側的 [ **資料集** ] 索引標籤。 選取 [ **資料集監視器**]。

![監看清單](./media/how-to-monitor-datasets/monitor-list.png)

按一下 [ **+ 建立監視** ] 按鈕，然後按 **[下一步**] 繼續進行嚮導。  

:::image type="content" source="media/how-to-monitor-datasets/wizard.png" alt-text="建立監視器嚮導":::

* **選取 [目標資料集**]。  目標資料集是具有指定時間戳記資料行的表格式資料集，將會針對資料漂移進行分析。 目標資料集必須具有與基準資料集共通的功能，而且應該是將 `timeseries` 新資料附加至其中的資料集。 您可以分析目標資料集中的歷程記錄資料，也可以監視新的資料。

* **選取 [基準資料集]。**  選取要當做一段時間的目標資料集比較基準使用的表格式資料集。  基準資料集必須具有與目標資料集共通的功能。  選取時間範圍以使用目標資料集的配量，或指定要做為基準的個別資料集。

* **監視設定**。  這些設定適用于已排程的資料集監視器管線，將會加以建立。 

    | 設定 | 描述 | 提示 | 可變動 | 
    | ------- | ----------- | ---- | ------- |
    | 名稱 | 資料集監視的名稱。 | | 否 |
    | 功能 | 將分析一段時間內的資料漂移的功能清單。 | 設定為模型的輸出功能 (s) 來測量概念漂移。 請勿包含在一段時間內自然漂移的功能， (月份、年份、索引等 ) 。 您可以在調整功能的清單之後，回填和現有的資料漂移監視器。 | 是 | 
    | 計算目標 | Azure Machine Learning 計算目標來執行資料集監視作業。 | | 是 | 
    | 啟用 | 啟用或停用資料集監視器管線上的排程 | 停用排程，以使用回填設定來分析歷程記錄資料。 您可以在建立資料集監視之後啟用它。 | 是 | 
    | 頻率 | 當執行回填時，將用來排程管線作業以及分析歷程記錄資料的頻率。 選項包括 [每日]、[每週] 或 [每月]。 | 每次執行都會根據頻率來比較目標資料集中的資料： <li>每日：比較目標資料集中最新的完整日期與基準 <li>每週：比較最新的整周 (目標資料集內的星期一-星期日) 和基準 <li>每月：比較目標資料集中的最新完整月份與基準 | 否 | 
    | Latency | 資料抵達資料集所需的時間（以小時為單位）。 比方說，如果資料需要三天的時間抵達 SQL DB 中的資料集封裝，請將延遲設定為72。 | 建立資料集監視之後，無法變更 | 否 | 
    | 電子郵件地址 | 根據資料漂移百分比閾值的缺口來發出警示的電子郵件地址。 | 電子郵件會透過 Azure 監視器傳送。 | 是 | 
    | 閾值 | 電子郵件警示的資料漂移百分比閾值。 | 您可以在工作區相關聯的 Application Insights 資源中，針對許多其他計量設定進一步的警示和事件。 | 是 |

完成嚮導之後，所產生的資料集監視器將會出現在清單中。 選取它以移至該監視器的詳細資料頁面。

## <a name="understand-data-drift-results"></a>瞭解資料漂移結果

本節說明監視資料集的結果，可在 Azure studio 的 [**資料**集  /  **監視器**] 頁面中找到。  您可以在此頁面上更新設定，以及分析現有資料的特定時間週期。  

從最上層深入解析到資料漂移的程度，以及要進一步調查的特徵醒目提示。

:::image type="content" source="media/how-to-monitor-datasets/drift-overview.png" alt-text="漂移總覽":::


| 計量 | 描述 | 
| ------ | ----------- | 
| 資料漂移的大小 | 基準和目標資料集一段時間之間的漂移百分比。 範圍從0到100，0表示相同的資料集，而100表示 Azure Machine Learning 的資料漂移模型可以完全分辨兩個資料集。 預期所測量的精確百分比雜訊是因為使用機器學習技術來產生此量值所預期。 | 
| 熱門離開功能 | 顯示資料集內最多漂移的功能，因此會對漂移量度量產生最大影響。 由於 covariate 移位，功能的基礎分佈不一定需要變更為具有相當高的特徵重要性。 |
| 閾值 | 資料漂移量超過設定的臨界值將會觸發警示。 這可以在 [監視] 設定中設定。 | 

### <a name="drift-magnitude-trend"></a>漂移幅度趨勢

查看資料集在指定的時間週期內與目標資料集的差異。  接近100%，這兩個資料集的差異越多。

:::image type="content" source="media/how-to-monitor-datasets/drift-magnitude.png" alt-text="漂移幅度趨勢":::

### <a name="drift-magnitude-by-features"></a>依功能變化的漂移

本節包含一段時間內所選功能散發的變更，以及其他統計資料的功能等級見解。

目標資料集也會在一段時間內進行分析。 每項功能的基準分佈之間的統計距離會與目標資料集的經過時間比較。  就概念而言，這類似于資料漂移的程度。  不過，此統計距離適用于個別功能，而不是所有功能。 最小值、最大值和平均值也可供使用。

在 Azure Machine Learning studio 中，按一下圖表中的橫條以查看該日期的功能層級詳細資料。 根據預設，您會看到基準資料集的散發，以及相同功能的最新執行分佈。

:::image type="content" source="media/how-to-monitor-datasets/drift-by-feature.gif" alt-text="依功能變化的漂移":::

您也可以透過物件上的方法，在 Python SDK 中取出這些計量 `get_metrics()` `DataDriftDetector` 。

### <a name="feature-details"></a>功能詳細資料

最後，向下滾動以查看每個個別功能的詳細資料。  使用圖表上方的下拉式清單來選取功能，並另外選取您要查看的度量。

:::image type="content" source="media/how-to-monitor-datasets/numeric-feature.gif" alt-text="數值特徵圖形和比較":::

圖表中的計量取決於功能的類型。

* 數值特徵

    | 計量 | 描述 |  
    | ------ | ----------- |  
    | Wasserstein 距離 | 將基準分佈轉換成目標分佈的最小工作量。 |
    | 平均值 | 功能的平均值。 |
    | 最小值 | 功能的最小值。 |
    | 最大值 | 功能的最大值。 |

* 類別功能
    
    | 計量 | 描述 |  
    | ------ | ----------- |  
    | Euclidian 距離     |  分類資料行的計算。Euclidean 距離是以兩個向量計算，從兩個資料集的相同類別資料行的實際分佈所產生。0表示經驗分佈沒有任何差異。它偏離0，此資料行的漂移愈多。您可以從這個計量的時間序列圖觀察趨勢，並在發現離開功能時很有説明。  |
    | 唯一值 | 功能 (基數) 的唯一值數目。 |

在此圖表上，選取單一日期來比較目標與這個日期之間的功能分佈，以顯示的功能。 針對數值功能，這會顯示兩個機率分佈。  如果功能是數值，則會顯示橫條圖。

:::image type="content" source="media/how-to-monitor-datasets/select-date-to-compare.gif" alt-text="選取要與目標比較的日期":::

## <a name="metrics-alerts-and-events"></a>計量、警示和事件

您可以在與您的 machine learning 工作區相關聯的 [Azure 應用程式 Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) 資源中查詢計量。 您可以存取 Application Insights 的所有功能，包括設定自訂警示規則和動作群組來觸發動作，例如電子郵件/SMS/Push/Voice 或 Azure Function。 如需詳細資訊，請參閱完整的 Application Insights 檔。 

若要開始使用，請流覽至 [Azure 入口網站](https://portal.azure.com) ，然後選取您工作區的 **[總覽** ] 頁面。  相關聯的 Application Insights 資源位於最右邊：

[![Azure 入口網站概觀](./media/how-to-monitor-datasets/ap-overview.png)](media/how-to-monitor-datasets/ap-overview-expanded.png)

在左窗格的 [監視] 底下，選取 [記錄 (分析]) ：

![Application insights 總覽](./media/how-to-monitor-datasets/ai-overview.png)

資料集監視器計量會儲存為 `customMetrics` 。 您可以在設定資料集監視器之後撰寫和執行查詢來加以查看：

[![Log analytics 查詢](./media/how-to-monitor-datasets/simple-query.png)](media/how-to-monitor-datasets/simple-query-expanded.png)

識別要設定警示規則的計量之後，請建立新的警示規則：

![新增警示規則](./media/how-to-monitor-datasets/alert-rule.png)

您可以使用現有的動作群組，或建立一個新的動作群組，以定義符合設定條件時要採取的動作：

![新增動作群組](./media/how-to-monitor-datasets/action-group.png)

## <a name="next-steps"></a>接下來的步驟

* 前往 [Azure Machine Learning studio](https://ml.azure.com) 或 [Python 筆記本](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datadrift-tutorial/datadrift-tutorial.ipynb) ，以設定資料集監視器。
* 瞭解如何在 [部署到 Azure Kubernetes Service 的模型](how-to-monitor-data-drift.md)上設定資料漂移。
* 使用 [事件方格](how-to-use-event-grid.md)設定資料集漂移監視器。 
* 如果您遇到問題，請參閱這些常見的 [疑難排解秘訣](resource-known-issues.md#data-drift) 。

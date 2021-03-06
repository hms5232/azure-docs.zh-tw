---
title: Azure 監視器記錄查詢中的 Audit 查詢
description: 記錄查詢 audit 記錄檔的詳細資料，可提供在 Azure 監視器中執行之記錄查詢的相關遙測資料。
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/03/2020
ms.openlocfilehash: bfaa9d8908d9401441d8811c3edcd087781b1d89
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2020
ms.locfileid: "89458632"
---
# <a name="audit-queries-in-azure-monitor-logs-preview"></a>Azure 監視器記錄中的 Audit 查詢 (preview) 
記錄查詢 audit 記錄檔提供有關在 Azure 監視器中執行之記錄查詢的遙測。 這包括執行查詢時、執行的物件、使用的工具、查詢文字，以及描述查詢執行的效能統計資料等資訊。


## <a name="configure-query-auditing"></a>設定查詢審核
使用 Log Analytics 工作區上的 [診斷設定](../platform/diagnostic-settings.md) 來啟用查詢審核。 這可讓您將審核資料傳送到目前的工作區或訂用帳戶中的任何其他工作區，以 Azure 事件中樞在 Azure 外部傳送或 Azure 儲存體以進行封存。 

### <a name="azure-portal"></a>Azure 入口網站
在下列任一位置的 Azure 入口網站中，存取 Log Analytics 工作區的診斷設定：

- 在 [ **Azure 監視器** ] 功能表中，選取 [ **診斷設定**]，然後找出並選取工作區。

    [![診斷設定 Azure 監視器 ](media/log-query-audit/diagnostic-setting-monitor.png)](media/log-query-audit/diagnostic-setting-monitor.png#lightbox) 

- 從 **Log Analytics 工作區** 功能表中，選取工作區，然後選取 [ **診斷設定**]。

    [![診斷設定記錄分析工作區 ](media/log-query-audit/diagnostic-setting-workspace.png)](media/log-query-audit/diagnostic-setting-workspace.png#lightbox) 

### <a name="resource-manager-template"></a>Resource Manager 範本
您可以從 [Log Analytics 工作區的診斷設定](../samples/resource-manager-diagnostic-settings.md#diagnostic-setting-for-log-analytics-workspace)取得範例 Resource Manager 範本。

## <a name="audit-data"></a>稽核資料
每次執行查詢時，就會建立一個審核記錄。 如果您將資料傳送至 Log Analytics 工作區，它會儲存在名為 *LAQueryLogs*的資料表中。 下表說明每個審核資料記錄中的屬性。

| 欄位 | 描述 |
|:---|:---|
| TimeGenerated         | 提交查詢的 UTC 時間。 |
| CorrelationId         | 用來識別查詢的唯一識別碼。 當您聯繫 Microsoft 尋求協助時，可以在疑難排解案例中使用。 |
| AADObjectId           | 開始查詢的使用者帳戶 Azure Active Directory 識別碼。  |
| AADTenantId           | 開始查詢之使用者帳戶的租使用者識別碼。  |
| AADEmail              | 啟動查詢之使用者帳戶租使用者的電子郵件。  |
| AADClientId           | 用來啟動查詢之應用程式的識別碼和已解析名稱。 |
| RequestClientApp      | 用來啟動查詢之應用程式的解析名稱。 |
| QueryTimeRangeStart   | 針對查詢所選取的時間範圍開始。 這可能不會在某些情況下填入，例如從 Log Analytics 開始查詢時，以及在查詢中指定時間範圍，而不是在時間選擇器內指定時間範圍。 |
| QueryTimeRangeEnd     | 針對查詢所選取的時間範圍結尾。 這可能不會在某些情況下填入，例如從 Log Analytics 開始查詢時，以及在查詢中指定時間範圍，而不是在時間選擇器內指定時間範圍。  |
| QueryText             | 已執行的查詢文字。 |
| RequestTarget         | API URL 用來提交查詢。  |
| RequestCoNtext        | 要求執行查詢的資源清單。 最多包含三個字串陣列：工作區、應用程式和資源。 以訂用帳戶或資源群組為目標的查詢將會顯示為 *資源*。 包含 RequestTarget 所隱含的目標。<br>如果可以解析每個資源的資源識別碼，就會包含這些資源識別碼。 如果在存取資源時傳回錯誤，則可能無法解決此問題。 在此情況下，將會使用查詢中的特定文字。<br>如果查詢使用不明確的名稱，例如多個訂用帳戶中現有的工作區名稱，則會使用這個不明確的名稱。 |
| RequestCoNtextFilters | 在查詢調用中指定的一組篩選。 最多可包含三個可能的字串陣列：<br>-ResourceTypes-要限制查詢範圍的資源類型<br>-工作區-要限制查詢的工作區清單<br>-WorkspaceRegions-要限制查詢的工作區區域清單 |
| ResponseCode          | 提交查詢時傳回的 HTTP 回應碼。 |
| ResponseDurationMs    | 傳迴響應的時間。  |
| ResponseRowCount     | 查詢傳回的資料列總數。 |
| StatsCPUTimeMs       | 用於計算、剖析和資料提取的總計算時間。 只有在查詢傳回的狀態碼為200時，才會填入。 |
| StatsDataProcessedKB | 處理查詢所存取的資料量。 受目標資料表大小、使用的時間範圍、套用的篩選，以及所參考資料行數目的影響。 只有在查詢傳回的狀態碼為200時，才會填入。 |
| StatsDataProcessedStart | 存取用來處理查詢的最舊資料的時間。 受查詢明確時間範圍和套用的篩選所影響。 這可能會因為資料分割而大於明確的時間範圍。 只有在查詢傳回的狀態碼為200時，才會填入。 |
| StatsDataProcessedEnd  |存取處理查詢的最新資料時間。 受查詢明確時間範圍和套用的篩選所影響。 這可能會因為資料分割而大於明確的時間範圍。 只有在查詢傳回的狀態碼為200時，才會填入。 |
| StatsWorkspaceCount | 查詢所存取的工作區數目。 只有在查詢傳回的狀態碼為200時，才會填入。 |
| StatsRegionCount | 查詢所存取的區域數目。 只有在查詢傳回的狀態碼為200時，才會填入。 |

## <a name="considerations"></a>考量

- 來自 Azure 資料總管 proxy 的查詢無法使用效能統計資料。 這些查詢的所有其他資料仍會填入。
- [混淆字串常](/azure/data-explorer/kusto/query/scalar-data-types/string#obfuscated-string-literals)值之字串的*h*提示不會影響查詢審核記錄。 查詢將會完全依照提交方式來捕捉，而不會將字串模糊化。 您應確定只有具有合規性許可權的使用者可以使用 Log Analytics 工作區中可用的各種 RBAC 模式來查看這項資料。
- 對於包含多個工作區資料的查詢，只會在使用者具有存取權的工作區中捕捉查詢。

## <a name="next-steps"></a>接下來的步驟

- 深入瞭解 [診斷設定](../platform/diagnostic-settings.md)。
- 深入瞭解 [優化記錄查詢](query-optimization.md)。
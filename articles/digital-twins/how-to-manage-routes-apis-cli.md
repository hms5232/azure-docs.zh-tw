---
title: " (Api 和 CLI) 管理端點和路由"
titleSuffix: Azure Digital Twins
description: 瞭解如何設定和管理 Azure 數位 Twins 資料的端點和事件路由。
author: alexkarcher-msft
ms.author: alkarche
ms.date: 6/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 27b745353521a44733c46170a5f5952c194c2343
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89293501"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>管理 Azure 數位 Twins 中的端點和路由 (Api 和 CLI) 

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

在 Azure 數位 Twins 中，您可以將 [事件通知](how-to-interpret-event-data.md) 路由傳送至下游服務或已連線的計算資源。 這是藉由先設定可接收事件的 **端點** 來完成。 然後，您可以建立  [**事件路由**](concepts-route-events.md) ，以指定 Azure 數位 Twins 所產生的哪些事件會傳遞給哪些端點。

您可以使用 [EventRoutes api](how-to-use-apis-sdks.md)、 [.Net (c # ) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)或 [Azure 數位 Twins CLI](how-to-use-cli.md)來管理端點和路由。 本文將逐步引導您完成透過這些機制建立端點和路由的流程。

您也可以透過 [Azure 入口網站](https://portal.azure.com)來管理它們。 如需使用入口網站的文章版本，請參閱 [*如何： (入口網站) 管理端點和路由 *](how-to-manage-routes-portal.md)。

## <a name="prerequisites"></a>必要條件

* 您將需要 **Azure 帳戶** (您可以在 [這裡](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 免費設定一個帳戶) 
* 您將需要 azure 訂用帳戶中的 **Azure 數位 Twins 實例** 。 如果您還沒有實例，可以使用 how [*to：設定實例和驗證*](how-to-set-up-instance-scripted.md)中的步驟來建立一個實例。 設定中的下列值可方便用於本文稍後：
    - 執行個體名稱
    - 資源群組
    
## <a name="create-an-endpoint-for-azure-digital-twins"></a>建立 Azure 數位 Twins 的端點

以下是您可以為您的實例建立的支援端點類型：
* [Event Grid](../event-grid/overview.md)
* [事件中樞](../event-hubs/event-hubs-about.md)
* [服務匯流排](../service-bus-messaging/service-bus-messaging-overview.md)

如需不同端點類型的詳細資訊，請參閱在 [*Azure 訊息服務之間選擇*](../event-grid/compare-messaging-services.md)。

若要將端點連結至 Azure 數位 Twins，您要用於端點的事件方格主題、事件中樞或服務匯流排必須已經存在。 

### <a name="create-an-event-grid-endpoint"></a>建立事件方格端點

下列範例示範如何使用 Azure CLI 建立事件方格型別端點。 您可以使用 [Azure Cloud Shell](https://shell.azure.com)，或 [在本機安裝 CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。

首先，建立事件方格主題。 您可以使用下列命令，或是造訪事件方格*自訂事件*快速入門的[[*建立自訂主題*] 區段](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic)，更詳細地查看步驟。

```azurecli
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> 若要輸出可傳遞至 Azure CLI 命令中的 Azure 區域名稱清單，請執行此命令：
> ```azurecli
> az account list-locations -o table
> ```

建立主題之後，您可以使用下列命令將它連結至 Azure 數位 Twins：

```azurecli
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

現在，事件方格主題可作為 Azure 數位 Twins 內部的端點，在以引數指定的名稱下 `--endpoint-name` 。 您通常會使用該名稱做為 **事件路由**的目標，您 [稍後會在本文中](#event-routes-with-apis-and-the-c-sdk) 使用 AZURE 數位 Twins 服務 API 來建立。

### <a name="create-an-event-hubs-or-service-bus-endpoint"></a>建立事件中樞或服務匯流排端點

建立事件中樞或服務匯流排端點的程式類似于上面所示的事件方格處理常式。

首先，建立您將作為端點使用的資源。 以下是所需的內容：
* 服務匯流排： _服務匯流排命名空間_、 _服務匯流排主題_、 _授權規則_
* 事件中樞： _事件中樞命名空間_、 _事件中樞_、 _授權規則_

然後，使用下列命令在 Azure 數位 Twins 中建立端點： 

* 新增服務匯流排主題端點 (需要預先建立的服務匯流排資源) 
```azurecli 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* 新增事件中樞端點 (需要預先建立的事件中樞資源) 
```azurecli
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

## <a name="event-routes-with-apis-and-the-c-sdk"></a> (Api 和 c # SDK 的事件路由) 

若要實際將資料從 Azure 數位 Twins 傳送至端點，您必須定義 **事件路由**。 Azure 數位 Twins **EventRoutes api** 可讓開發人員連接整個系統和下游服務的事件流程。 深入瞭解事件路由的 [*概念：路由傳送 Azure 數位 Twins 事件*](concepts-route-events.md)。

本節中的範例會使用 c # SDK。

必要條件 **：您**必須如本文稍早所述建立端點，才能繼續建立路由。 當您的端點完成設定之後，您可以繼續建立事件路由。

>[!NOTE]
>如果您最近部署了端點，請先確認它們已完成部署， **再** 嘗試將它們用於新的事件路由。 如果路由部署因為端點未就緒而失敗，請稍候幾分鐘，然後再試一次。
>
> 如果您正在編寫此流程的腳本，您可能會想要在等候結束之前，先在2-3 分鐘內建立端點服務完成部署的等候時間，然後再繼續進行路由設定。

### <a name="create-an-event-route"></a>建立事件路由

事件路由是使用資料平面 Api 定義。 

路由定義可以包含下列元素：
* 您要使用的路由名稱
* 您要使用的端點名稱
* 定義要傳送至端點之事件的篩選準則 

如果沒有路由名稱，則不會在 Azure 數位 Twins 外部路由傳送任何訊息。 如果有路由名稱且篩選為 `true` ，則會將所有訊息路由傳送至端點。 如果有已加入的路由名稱和不同的篩選準則，則會根據篩選準則來篩選訊息。

一個路由應該會允許選取多個通知和事件種類。 

`CreateEventRoute` 是用來新增事件路由的 SDK 呼叫。 以下是其使用方式的範例：

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter("true"); //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

> [!TIP]
> 所有 SDK 函式都有同步和非同步版本。

### <a name="event-route-sample-code"></a>事件路由範例程式碼

下列程式碼範例顯示如何建立、列出和刪除事件路由：
```csharp
try
{
    Console.WriteLine("Create a route: testRoute1");
    EventRoute er = new EventRoute("< your - endpoint - name >");
    // Make a filter that passes everything
    er.Filter = "true";
    client.CreateEventRoute("< your - route - name >", er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable <EventRoute> result = client.GetEventRoutes();
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointId} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
}
catch (RequestFailedException e)
{
    Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
}
```

### <a name="filter-events"></a>篩選事件

若未篩選，端點會從 Azure 數位 Twins 接收各種事件：
* 使用 Azure 數位 Twins 服務 API 的 [數位 twins](concepts-twins-graph.md) 所引發的遙測
* 對應項屬性變更通知，會在 Azure 數位 Twins 實例中任何對應項的屬性變更時引發
* 生命週期事件，在 twins 或關聯性建立或刪除時引發
* 新增或刪除 Azure 數位 Twins 實例 [中設定的模型時](concepts-models.md) 引發的模型變更事件

您可以將端點的 **篩選** 新增至事件路由，以限制傳送的事件。

若要加入篩選，您可以使用 PUT 要求至 HTTPs：/ */{YourHost}/EventRoutes/myNewRoute？ api-version = 2020-05-31-preview-preview* 搭配下列主體：

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

以下是支援的路由篩選。 使用 *篩選文字架構* 資料行中的詳細資料來取代 `<filter-text>` 上述要求主體中的預留位置。

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

## <a name="manage-endpoints-and-routes-with-cli"></a>使用 CLI 管理端點和路由

您也可以使用 Azure 數位 Twins CLI 來管理端點和路由。 如需使用 CLI 和可用命令的詳細資訊，請參閱作法 [*：使用 Azure 數位 TWINS CLI*](how-to-use-cli.md)。

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>接下來的步驟

瞭解您可以接收的不同事件訊息類型：
* [*How to：解讀事件資料*](how-to-interpret-event-data.md)

---
title: Azure 事件中樞-例外狀況
description: 本文提供 Azure 事件中樞傳訊例外狀況和建議的動作清單。
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: a93daa88c468a22838a6f9012f0c4622447f5555
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86512362"
---
# <a name="event-hubs-messaging-exceptions---net"></a>事件中樞訊息例外狀況-.NET
本節列出 .NET Framework Api 所產生的 .NET 例外狀況。 

## <a name="exception-categories"></a>例外狀況類別

事件中樞的 .NET Api 會產生可能屬於下列類別的例外狀況，以及您可以嘗試修正它們的相關聯動作：

 - 使用者程式碼撰寫錯誤： 
 
   - [ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1)
   - [InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1)
   - [System.OperationCanceledException](/dotnet/api/system.operationcanceledexception?view=netcore-3.1)
   - [SerializationException。](/dotnet/api/system.runtime.serialization.serializationexception?view=netcore-3.1)
   
   一般動作：請先嘗試修正程式碼，再繼續進行。
 
 - 設定/組態錯誤： 
 
   - [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception)
   - [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception)
   - [System.UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1)
   
   一般動作：請檢查您的設定，並視需要進行變更。
   
 - 暫時性例外狀況： 
 
   - [MessagingException。](/dotnet/api/microsoft.servicebus.messaging.messagingexception)
   - [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception)
   - [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception)
   - [MessagingCommunicationException。](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)
   
   一般動作：重試此作業或通知使用者。
 
 - 其他例外狀況： 
 
   - [System.transactions.transactionexception](/dotnet/api/system.transactions.transactionexception?view=netcore-3.1)
   - [System.TimeoutException](#timeoutexception)
   - [Microsoft.servicebus.messaging.messagelocklostexception。](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception)
   - [、Microsoft.servicebus.messaging.sessionlocklostexception。](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception)
   
   一般動作：例外狀況類型特有;請參閱下一節中的表格。 

## <a name="exception-types"></a>例外狀況類型
下表列出傳訊例外狀況類型及其原因，並指出您可以採取的建議動作。

| 例外狀況類型 | 描述/原因/範例 | 建議的動作 | 自動/立即重試的注意事項 |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1) |伺服器不會在指定的時間內回應要求的作業，這是由[OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)所控制。 伺服器可能已完成要求的作業。 發生此例外狀況的原因可能是網路或其他基礎結構延遲。 |檢查系統狀態的一致性，並視需要重試。<br /> 請參閱 [TimeoutException](#timeoutexception)。 | 在某些情況下，重試也許有幫助；將重試邏輯新增至程式碼。 |
| [InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1) |伺服器或服務內不允許要求的使用者作業。 如需詳細資訊，請參閱例外狀況訊息。 例如，如果是在 [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) 模式收到訊息， [Complete](/dotnet/api/microsoft.servicebus.messaging.receivemode) 將會產生這個例外狀況。 | 檢查程式碼和文件。 確定要求的作業無效。 | [重試] 將無法提供協助。 |
| [OperationCanceledException](/dotnet/api/system.operationcanceledexception?view=netcore-3.1) | 嘗試在已關閉、中止或處置的物件上叫用作業。 極少數的情況下，環境交易是已處置狀態。 | 檢查程式碼，並確定它不會在已處置的物件上叫用作業。 | [重試] 將無法提供協助。 |
| [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1) | [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider)物件無法取得權杖、權杖無效，或權杖未包含執行作業所需的宣告。 | 確定權杖提供者是以正確的值建立。 檢查存取控制服務的設定。 | 在某些情況下，重試也許有幫助；將重試邏輯新增至程式碼。 |
| [ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1)<br /> [ArgumentNullException](/dotnet/api/system.argumentnullexception?view=netcore-3.1)<br />[ArgumentOutOfRangeException](/dotnet/api/system.argumentoutofrangeexception?view=netcore-3.1) | 提供給方法的一個或多個引數無效。 提供給 [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) 或 [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) 的 URI 包含路徑區段。 提供給 [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) 或 [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) 的 URI 配置無效。 屬性值大於 32 KB。 | 檢查呼叫程式碼，並確定引數正確無誤。 | 重試將無助益。 |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | 與作業相關聯的實體不存在或已被刪除。 | 確定實體已存在。 | 重試將無助益。 |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | 用戶端無法建立事件中樞連線。 |確定提供的主機名稱正確，且主機可以連線。 | 如果有間歇性的連線問題，重試也許有幫助。 |
| [。訊息 ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | 服務目前無法處理要求。 | 用戶端可以等待一段時間，然後再重試作業。 <br /> 請參閱 [ServerBusyException](#serverbusyexception)。 | 用戶端可以在特定間隔後重試。 如果重試產生不同的例外狀況，請檢查該例外狀況的重試行為。 |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | 可能會在下列情況中擲回的一般傳訊例外狀況：利用屬於不同實體類型 (例如主題) 的名稱或路徑嘗試建立 [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) 。 嘗試傳送大於 1 MB 的訊息。 處理要求時伺服器或服務發生錯誤。 如需詳細資訊，請參閱例外狀況訊息。 此例外狀況通常是暫時性例外狀況。 | 查看程式碼，並確定訊息內文只使用可序列化的物件 (或使用自訂序列化程式)。 查看文件來了解支援的屬性值類型，並且只使用支援的類型。 查看 [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) 屬性。 如果該屬性為 **True**，您就可以重試作業。 | 重試行為未定義，而且可能沒有幫助。 |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | 嘗試在該服務命名空間中以另一個實體已在使用的名稱建立實體。 | 刪除現有的實體，或選擇不同的名稱來建立實體。 | 重試將無助益。 |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | 傳訊實體已達到允許的大小上限。 如果在個別取用者群組層級開啟的接收者數目已達到上限 (5)，便可能發生此例外狀況。 | 從實體或其子佇列接收訊息，在實體中建立空間。 <br /> 請參閱[QuotaExceededException](#quotaexceededexception) | 如果在此同時已移除訊息，重試可能會有幫助。 |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | 在停用的實體上要求執行階段作業。 |啟用實體。 | 如實體在過渡期間被啟用，重試可能會有幫助。 |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | 訊息承載超過 1 MB 的限制。 此 1 MB 的限制適用于 total 訊息，其中可能包括系統屬性和任何 .NET 額外負荷。 | 減少訊息裝載大小，然後再重試作業。 |重試將無助益。 |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) 指出已超過某特定實體的配額。

如果在個別取用者群組層級開啟的接收者數目已達到上限 (5)，便可能發生此例外狀況。

### <a name="event-hubs"></a>事件中樞
每一個事件中樞都有 20 個用戶群組的限制。 當您嘗試建立更多時，您會收到 [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception)。 

## <a name="timeoutexception"></a>TimeoutException
[TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1) 表示使用者啟始作業所用的時間長過作業逾時。 

事件中樞的逾時是指定為連接字串的一部分，或透過 [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder)指定。 錯誤訊息本身可能不盡相同，但它一定會包含目前作業的指定逾時值。 

### <a name="common-causes"></a>常見的原因
這個錯誤有兩個常見的原因︰設定不正確或暫時性服務錯誤。

- **設定不正確** ：操作條件的作業逾時可能太小。 用戶端 SDK 的作業逾時預設值為 60 秒。 請檢查程式碼是否將值設定過小。 網路和 CPU 使用量的狀況會影響特定作業完成所需的時間，因此作業超時不應設定為較小的值。
- **暫時性服務錯誤** ：有時事件中樞服務在處理要求時會遇到延遲，例如，高流量的時段。 在這種情況下，您可以在延遲後重試作業，直到作業成功為止。 如果多次嘗試同一作業之後仍然失敗，請瀏覽 [Azure 服務狀態網站](https://azure.microsoft.com/status/)，看看是否有任何已知的服務中斷。

## <a name="serverbusyexception"></a>ServerBusyException

[Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) 或 [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) 表示伺服器已超載。 此例外狀況有兩個相關的錯誤碼。

### <a name="error-code-50002"></a>錯誤碼 50002
此錯誤會因為兩個原因其中之一而發生：

- 負載不會平均分散到事件中樞上的所有分割區，而一個分割區會達到本機輸送量單位限制。
    
    **解決**方式：修改分割區散發策略，或嘗試[EventHubClient （eventDataWithOutPartitionKey）](/dotnet/api/microsoft.servicebus.messaging.eventhubclient)可能會有説明。

- 事件中樞命名空間沒有足夠的輸送量單位（您可以在 [ [Azure 入口網站](https://portal.azure.com)] 的 [事件中樞命名空間] 視窗中檢查 [**計量**] 畫面以確認）。 入口網站會顯示匯總（1分鐘）的資訊，但我們會即時測量輸送量–因此只是估計值。

    **解決**方式：增加命名空間的輸送量單位可能會有説明。 您可以透過入口網站，在 [事件中樞命名空間] 畫面的 [調整]**** 視窗中執行此作業。 您也可以使用[自動擴充](event-hubs-auto-inflate.md)。

### <a name="error-code-50001"></a>錯誤碼 50001

此錯誤應該很少會發生。 它會在執行您命名空間的程式碼之容器的 CPU 不足時發生 – 就在事件中樞負載平衡器開始之前幾秒鐘。

**解決**方式：限制對 GetRuntimeInformation 方法的呼叫。 Azure 事件中樞支援每秒最多50個呼叫，每秒 GetRuntimeInfo。 達到限制之後，您可能會收到類似下列的例外狀況：

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```


## <a name="next-steps"></a>後續步驟

您可以造訪下列連結以深入了解事件中樞︰

* [事件中心概觀](./event-hubs-about.md)
* [建立事件中樞](event-hubs-create.md)
* [事件中樞常見問題集](event-hubs-faq.md)

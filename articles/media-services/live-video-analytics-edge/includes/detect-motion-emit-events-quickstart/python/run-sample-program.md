---
ms.openlocfilehash: ed7be82146f38cc7ae57fd863bb0c1b8e6910fd2
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691022"
---
請遵循下列步驟以執行範例程式碼：

1. 在 Visual Studio Code 中，移至 *src/cloud-to-device-console-app/operations.json*。
1. 在 [GraphTopologySet] 節點上，確定您有看到下列值：

    `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/motion-detection/topology.json"`
1. 在 [GraphInstanceSet] 和 [GraphTopologyDelete] 節點上，確定 `topologyName` 的值符合圖表拓撲中 `name` 屬性的值：

    `"topologyName" : "MotionDetection"`
    
1. 選取 F5 鍵來啟動偵錯工作階段。 [終端機] 視窗將會顯示一些訊息。
1. *operations.json* 檔案首先會呼叫 `GraphTopologyList` 和 `GraphInstanceList`。 如果您在完成先前的快速入門之後清除了資源，則此程序會傳回空的清單，然後暫停。 若要繼續，請選取 Enter 鍵。

    ```
    --------------------------------------------------------------------------
    Executing operation GraphTopologyList
    -----------------------  Request: GraphTopologyList  --------------------------------------------------
    {
        "@apiVersion": "1.0"
    }
    ---------------  Response: GraphTopologyList - Status: 200  ---------------
    {
        "value": []
    }
    --------------------------------------------------------------------------
    Executing operation WaitForInput
    Press Enter to continue
    ```
    
    [終端機] 視窗會顯示下一組直接方法呼叫：
     * 會使用上述 `topologyUrl` 的對 `GraphTopologySet` 呼叫
     * 會使用下列主體的對 `GraphInstanceSet` 呼叫：
         
    ```
    {
      "@apiVersion": "1.0",
      "name": "Sample-Graph",
      "properties": {
        "topologyName": "MotionDetection",
        "description": "Sample graph description",
        "parameters": [
          {
            "name": "rtspUrl",
            "value": "rtsp://rtspsim:554/media/camera-300s.mkv"
          },
          {
            "name": "rtspUserName",
            "value": "testuser"
          },
          {
            "name": "rtspPassword",
            "value": "testpassword"
          }
        ]
      }
    }
    ```
     
    * 呼叫 `GraphInstanceActivate` 以啟動圖表執行個體和影片流程。
    * 第二次呼叫 `GraphInstanceList` 以顯示圖表執行個體處於執行中狀態。
1. [終端機] 視窗中的輸出會在 `Press Enter to continue` 暫停。 還不要選取 Enter 鍵。 請向上捲動，查看您所叫用直接方法的 JSON 回應承載。
1. 切換至 Visual Studio Code 中的 [輸出] 視窗。 您會看到訊息指出 IoT Edge 模組上的 Live Video Analytics 正在傳送到 IoT 中樞。 本快速入門的下一節會討論這些訊息。
1. 媒體圖表會繼續執行並列印結果。 RTSP 模擬器會持續循環播放來源影片。 若要停止媒體圖表，請返回 [終端機] 視窗，然後選取 Enter 鍵。 

    下一系列的呼叫會清除資源：

    * 對 `GraphInstanceDeactivate` 的呼叫會停用圖表執行個體。
    * 對 `GraphInstanceDelete` 的呼叫會刪除執行個體。
    * 對 `GraphTopologyDelete` 的呼叫會刪除拓撲。
    * 最後對 `GraphTopologyList` 的呼叫會顯示清單是空的。

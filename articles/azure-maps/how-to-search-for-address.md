---
title: 使用 Azure 地圖服務搜尋服務搜尋位置
description: 瞭解 Azure 地圖服務搜尋服務。 請參閱如何使用這組適用于地理編碼、反向地理編碼、模糊搜尋和反向交叉街道搜尋的 Api。
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 48dd0168f878a16e2eabe47151d0b09993d9f5f9
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2020
ms.locfileid: "88037774"
---
# <a name="search-for-a-location-using-azure-maps-search-services"></a>使用 Azure 地圖服務搜尋服務搜尋位置

[Azure 地圖服務搜尋服務](https://docs.microsoft.com/rest/api/maps/search)是一組 RESTful api，旨在協助開發人員依名稱、類別和其他地理資訊搜尋位址、地點和商務清單。 除了支援傳統地理編碼，服務也可以根據緯度和經度來反轉地理編碼位址和跨街道。 搜尋所傳回的緯度和經度值可用來做為其他 Azure 地圖服務服務的參數，例如「[路線](https://docs.microsoft.com/rest/api/maps/route)」和「[氣象](https://docs.microsoft.com/rest/api/maps/weather)服務」。

在本文中，您將學會如何：

* 使用[搜尋位址 API]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddress)，為位址 (地理編碼位址位置) 要求緯度和經度座標。
* 使用[模糊搜尋 API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)，搜尋 (POI) 的相關位址或點。
* 進行[反向位址搜尋](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)，將座標位置轉譯為街道位址。
* 使用[搜尋位址反向交叉街道 API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet)，將座標位置轉譯為人類易懂的交叉街道。  最常見的情況是，追蹤從裝置或資產接收 GPS 摘要的應用程式，並想要知道座標所在的位置。

## <a name="prerequisites"></a>Prerequisites

1. [建立 Azure 地圖服務帳戶](quick-demo-map-app.md#create-an-azure-maps-account)
2. [取得主要訂用帳戶金鑰](quick-demo-map-app.md#get-the-primary-key-for-your-account)，也稱為主要金鑰或訂用帳戶金鑰。

本教學課程使用 [Postman](https://www.postman.com/) 應用程式，但您可以選擇不同的 API 開發環境。

## <a name="request-latitude-and-longitude-for-an-address-geocoding"></a> (地理編碼的位址要求緯度和經度) 

在此範例中，我們將使用 Azure 地圖服務[取得搜尋位址 API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) ，將位址轉換成緯度和經度座標。 這個程式也稱為*地理編碼*。 除了傳回座標以外，回應也會傳回詳細的位址屬性，例如街道、郵遞區號、municipality 和國家/地區資訊。

>[!TIP]
>如果您有一組要地理編碼的位址，您可以在單一 API 呼叫中使用[Post 搜尋位址 BATCH API](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatch)來傳送查詢批次。

1. 開啟 Postman 應用程式。 在 Postman 應用程式頂端處，選取 [新增]。 在 [新建] 視窗中，選取 [集合]。  為集合命名，然後選取 [建立] 按鈕。 您將使用此集合來執行本檔中其餘的範例。

2. 若要建立要求，請再次選取 [新增]。 在 [新建] 視窗中，選取 [要求]。 輸入要求的 [要求名稱]。 選取您在先前的步驟中建立的集合，然後選取 [儲存]。

3. 在 [建立器] 索引標籤中選取 [**取得**HTTP] 方法，然後輸入下列 URL。 在此要求中，我們要搜尋特定的位址： `400 Braod St, Seattle, WA 98109` 。

    對於此要求以及本文中提及的其他要求，請將 `{Azure-Maps-Primary-Subscription-key}` 取代為您的主要訂用帳戶金鑰。 要求應會類似於下列 URL：

    ```http
    https://atlas.microsoft.com/search/address/json?&subscription-key={Azure-Maps-Primary-Subscription-key}&api-version=1.0&language=en-US&query=400 Broad St, Seattle, WA 98109
    ```

4. 按一下藍色的 [**傳送**] 按鈕。 回應主體會包含單一位置的資料。

5. 現在，我們將搜尋有一個以上可能位置的位址。 在 [**參數**] 區段中，將 `query` 金鑰變更為 `400 Broad, Seattle` 。 按一下藍色的 [**傳送**] 按鈕。

    :::image type="content" source="./media/how-to-search-for-address/search-address.png" alt-text="搜尋位址":::

6. 接下來，嘗試將 `query` 金鑰設定為 `400 Broa` 。

7. 按一下 [傳送] 按鈕。  您現在可以看到回應包含來自多個國家/地區的回應。 若要將結果 geobias 至使用者的相關區域，請一律盡可能將最多位置詳細資料新增至要求。

## <a name="using-fuzzy-search-api"></a>使用模糊搜尋 API

Azure 地圖服務[模糊搜尋 API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)支援標準的單一行和自由格式的搜尋。 當您不知道搜尋要求的使用者輸入類型時，建議您使用 Azure 地圖服務搜尋模糊 API。  查詢輸入可以是完整或部分的位址。 這也可能是 POI) token 的重點 (，例如 POI 的名稱、POI 類別或品牌名稱。 此外，若要改善搜尋結果的相關性，查詢結果可以受到座標位置和半徑的限制，或定義周框方塊。

>[!TIP]
>大部分的搜尋查詢預設為 maxFuzzyLevel = 1，以取得效能並減少不尋常的結果。 您可以使用或參數來調整顏色等級 `maxFuzzyLevel` `minFuzzyLevel` 。 如需的詳細資訊， `maxFuzzyLevel` 以及所有選擇性參數的完整清單，請參閱[模糊搜尋 URI 參數](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy#uri-parameters)

### <a name="search-for-an-address-using-fuzzy-search"></a>使用模糊搜尋來搜尋地址

在此範例中，我們將使用模糊搜尋來搜尋整個世界 `pizza` 。  然後，我們將示範如何在特定國家/地區的範圍內進行搜尋。 最後，我們將示範如何使用座標位置和半徑來界定特定區域的搜尋範圍，並限制傳回的結果數目。

>[!IMPORTANT]
>若要將結果 geobias 至使用者的相關區域，請一律盡可能新增最多位置詳細資料。 若要深入瞭解，請參閱[搜尋的最佳做法](how-to-use-best-practices-for-search.md#geobiased-search-results)。

1. 開啟 Postman 應用程式，按一下 [**新增**]，然後選取 [**要求**]。 輸入要求的 [要求名稱]。 選取您在上一節中建立的集合，或建立一個新的集合，然後選取 [**儲存**]。

2. 在 [建立器] 索引標籤中選取 [**取得**HTTP] 方法，然後輸入下列 URL。 對於此要求以及本文中提及的其他要求，請將 `{Azure-Maps-Primary-Subscription-key}` 取代為您的主要訂用帳戶金鑰。 要求應會類似於下列 URL：

    ```http
   https://atlas.microsoft.com/search/fuzzy/json?&api-version=1.0&subscription-key={Azure-Maps-Primary-Subscription-key}&language=en-US&query=pizza
    ```

    >[!NOTE]
    >URL 路徑中的 _Json_ 屬性會判斷回應格式。 本文使用 json 來方便使用和可讀性。 若要尋找其他支援的回應格式，請參閱 `format` [URI 參數參考檔](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy#uri-parameters)中的參數定義。

3. 按一下 [傳送]****，然後檢視回應本文。

    「比薩餅」的不明確查詢字串會傳回10個[感興趣的結果](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse)， (POI) 在「比薩」和「餐廳」的類別中。 每個結果都包含 [街道位址]、[緯度] 和 [經度值]、[視圖埠] 和 [位置] 的進入點等詳細資料。 此查詢的結果現在會有所不同，而且不會系結至任何參考位置。
  
    在下一個步驟中，我們將使用 `countrySet` 參數來指定您的應用程式需要涵蓋範圍的國家/地區。 如需支援的國家/地區完整清單，請參閱[搜尋涵蓋範圍](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage)。

4. 預設行為是搜尋整個世界，可能會傳回不必要的結果。 接下來，我們只會搜尋美國境內的比薩餅。 將 `countrySet` 金鑰新增至**Params**區段，並將其值設定為 `US` 。 將 `countrySet` 金鑰設定為 `US` 會將結果系結至美國。

    :::image type="content" source="./media/how-to-search-for-address/search-fuzzy-country.png" alt-text="搜尋美國的比薩餅":::

    結果現在會依國家/地區程式碼繫結，此查詢會傳回美國境內的披薩餐廳。

5. 若要取得更具針對性的搜尋，您可以搜尋 lat 的範圍。/lon。 座標配對。 在此範例中，我們將使用/lon.。 西雅圖空間指標的。 因為我們只想要在 400-計量半徑內傳回結果，所以我們會新增 `radius` 參數。 此外，我們也會新增 `limit` 參數，將結果限制為最接近的五個比薩餅位置。

    在 [**參數**] 區段中，新增下列索引鍵/值組：

     | 機碼 | 值 |
    |-----|------------|
    | lat | 47.620525 |
    | lon | -122.349274 |
    | 半徑 | 400 |
    | limit | 5|

6. 按一下 [ **傳送**]。 回應包含位於西雅圖附近的比薩餅餐廳的結果指標。

## <a name="search-for-a-street-address-using-reverse-address-search"></a>使用反向地址搜尋來搜尋街道地址

Azure 地圖服務[取得搜尋位址反向 API]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)會將座標轉譯為人類可讀取的街道位址。 此 API 通常用於使用 GPS 摘要的應用程式，並想要在特定的座標點探索位址。

>[!IMPORTANT]
>若要將結果 geobias 至使用者的相關區域，請一律盡可能新增最多位置詳細資料。 若要深入瞭解，請參閱[搜尋的最佳做法](how-to-use-best-practices-for-search.md#geobiased-search-results)。

>[!TIP]
>如果您有一組要反向地理編碼的座標位置，您可以在單一 API 呼叫中使用[Post 搜尋位址反向批次 API](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatch)來傳送查詢批次。

在此範例中，我們將使用幾個可用的選擇性參數進行反向搜尋。 如需選擇性參數的完整清單，請參閱[反向搜尋參數](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#uri-parameters)。

1. 在 Postman 應用程式中，按一下 [**新增**]，然後選取 [**要求**]。 輸入要求的 [要求名稱]。 選取您在第一個區段中建立的集合，或建立一個新的集合，然後選取 [**儲存**]。

2. 在 [建立器] 索引標籤中選取 [**取得**HTTP] 方法，然後輸入下列 URL。 對於此要求以及本文中提及的其他要求，請將 `{Azure-Maps-Primary-Subscription-key}` 取代為您的主要訂用帳戶金鑰。 要求應會類似於下列 URL：

    ```http
    https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={Azure-Maps-Primary-Subscription-key}&language=en-US&query=47.591180,-122.332700&number=1
    ```

3. 按一下 [**傳送**]，然後檢查回應主體。 您應該會看到一個查詢結果。 回應會包含關於薩菲柯球場的重要地址資訊。
  
4. 現在，我們將在**Params**區段中新增下列索引鍵/值組：

    | 機碼 | 值 | 傳回
    |-----|------------|------|
    | 數字 | 1 |回應可能包含街道的側邊 (左/右) 以及數位的位移位置。|
    | returnSpeedLimit | true | 傳回位址的速度限制。|
    | returnRoadUse | true | 傳回位址的道路使用類型。 如需所有可能的道路使用類型，請參閱[道路使用類型](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#uri-parameters)。|
    | returnMatchType | true| 傳回符合的類型。 如需所有可能的值，請參閱[反向位址搜尋結果](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#searchaddressreverseresult)

   :::image type="content" source="./media/how-to-search-for-address/search-reverse.png" alt-text="反向搜尋。":::

5. 按一下 [**傳送**]，然後檢查回應主體。

6. 接下來，我們將新增 `entityType` 金鑰，並將其值設定為 `Municipality` 。 `entityType`金鑰將會覆寫 `returnMatchType` 上一個步驟中的金鑰。 我們也需要移除 `returnSpeedLimit` 和， `returnRoadUse` 因為我們要求的是 municipality 的相關資訊。  如需所有可能的實體類型，請參閱[實體類型](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#entitytype)。

    :::image type="content" source="./media/how-to-search-for-address/search-reverse-entity-type.png" alt-text="搜尋反向 entityType。":::

7. 按一下 [ **傳送**]。 將結果與步驟5中所傳回的結果進行比較。  因為要求的實體類型現在是 `municipality` ，回應不會包含街道位址資訊。 此外，傳回的 `geometryId` 可透過 Azure 地圖服務取得[搜尋多邊形 API](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon)來要求界限多邊形。

>[!TIP]
>若要取得這些參數的詳細資訊，以及瞭解其他專案，請參閱[反向搜尋參數一節](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#uri-parameters)。

## <a name="search-for-cross-street-using-reverse-address-cross-street-search"></a>使用反向位址交叉街道搜尋搜尋交叉街道

在此範例中，我們將根據地址的座標來搜尋交叉街道。

1. 在 Postman 應用程式中，按一下 [**新增**]，然後選取 [**要求**]。 輸入要求的 [要求名稱]。 選取您在第一個區段中建立的集合，或建立一個新的集合，然後選取 [**儲存**]。

2. 在 [建立器] 索引標籤中選取 [**取得**HTTP] 方法，然後輸入下列 URL。 對於此要求以及本文中提及的其他要求，請將 `{Azure-Maps-Primary-Subscription-key}` 取代為您的主要訂用帳戶金鑰。 要求應會類似於下列 URL：
  
    ```http
   https://atlas.microsoft.com/search/address/reverse/crossstreet/json?&api-version=1.0&subscription-key={Azure-Maps-Primary-Subscription-key}&language=en-US&query=47.591180,-122.332700
    ```

    :::image type="content" source="./media/how-to-search-for-address/search-address-cross.png" alt-text="搜尋交叉街道。":::
  
3. 按一下 [**傳送**]，然後檢查回應主體。 您會發現回應包含的 `crossStreet` 值為 `Occidental Avenue South` 。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [Azure 地圖服務搜尋服務 REST API](https://docs.microsoft.com/rest/api/maps/search)

> [!div class="nextstepaction"]
> [Azure 地圖服務搜尋服務最佳做法](how-to-use-best-practices-for-search.md)

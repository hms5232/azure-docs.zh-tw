---
title: 包含檔案
description: 包含檔案
services: cognitive-services
author: roy-har
manager: diberry
ms.service: cognitive-services
ms.date: 06/04/2020
ms.subservice: language-understanding
ms.topic: include
ms.custom: include file
ms.author: roy-har
ms.openlocfilehash: 9965e4c856fdef2af17b116264ad5344ebc97eb2
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84466948"
---
建立 Pizza 應用程式。

1. 選取 [pizza-app-for-luis-v6.json](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-app-for-luis-v6.json) 以顯示 `pizza-app-for-luis.json` 檔案的 GitHub 頁面。
1. 以滑鼠右鍵按一下或長點選 [原始] 按鈕，然後選取 [另存連結] 將 `pizza-app-for-luis.json` 儲存到您的電腦。
1. 登入 [LUIS 入口網站](https://www.luis.ai)。
1. 選取[我的應用程式](https://www.luis.ai/applications)。
1. 在 [我的應用程式] 頁面中，選取 [+ 新增對話應用程式]。
1. 選取 [匯入為 JSON]。
1. 在 [匯入新的應用程式] 對話方塊中，選取 [選擇檔案] 按鈕。
1. 選取您下載的 `pizza-app-for-luis.json` 檔案，然後選取 [開啟]。
1. 在 [匯入新的應用程式] 對話方塊的 [名稱] 欄位中，輸入您 Pizza 應用程式的名稱，然後選取 [完成] 按鈕。

應用程式將會匯入。

如果您看到 [如何建立有效的 LUIS 應用程式] 對話方塊，請關閉對話方塊。

## <a name="train-and-publish-the-pizza-app"></a>將 Pizza 應用程式定型並發佈

您應該會看到 [意圖] 頁面，其中包含 Pizza 應用程式中的意圖清單。

[!INCLUDE [How to train](howto-train.md)]

[!INCLUDE [How to publish](howto-publish.md)]

## <a name="add-an-authoring-resource-to-the-pizza-app"></a>將撰寫資源新增至 Pizza 應用程式

1. 選取 [管理]。
1. 選取 [Azure 資源]。
1. 選取 [製作資源]。
1. 選取 [變更製作資源]。

如果您有撰寫資源，請輸入**租用戶名稱**、**訂用帳戶名稱**，以及您撰寫資源的 **LUIS 資源名稱**。

如果您沒有撰寫資源：

1. 選取 [建立新的資源]。
1. 輸入**租用戶名稱**、**資源名稱**、**訂用帳戶名稱**和 **Azure 資源群組名稱**。

您的 Pizza 應用程式現在已準備好可以使用。

## <a name="record-the-access-values-for-your-pizza-app"></a>針對您的 Pizza 應用程式記錄存取值

若要使用您新的 Pizza 應用程式，將需要 Pizza 應用程式的應用程式識別碼、撰寫金鑰和撰寫端點。

若要尋找這些值：

1. 從 [意圖] 頁面上，選取 [管理]。
1. 從 [應用程式設定] 頁面上，記錄 [應用程式識別碼]。
1. 選取 [Azure 資源]。
1. 選取 [製作資源]。
1. 從 [撰寫資源] 索引標籤中，記錄「主索引鍵」。 此值是您的撰寫金鑰。
1. 記錄 [端點 URL]。 此值是您的撰寫端點。

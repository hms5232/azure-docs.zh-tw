---
title: 教學課程：仲裁電子商務產品影像 - Content Moderator
titleSuffix: Azure Cognitive Services
description: 本教學課程說明如何設定應用程式分析產品影像，並使用指定的標籤加以分類 (使用 Azure 電腦視覺和自訂視覺)。 標記令人反感的影像以供進一步地審核 (使用 Azure Content Moderator)。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 06/29/2020
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: 017dabf16384e53d924ed69f36b64050fcacb5bf
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934781"
---
# <a name="tutorial-moderate-e-commerce-product-images-with-azure-content-moderator"></a>教學課程：使用 Azure Content Moderator 仲裁電子商務產品影像

在本教學課程中，您將了解如何使用 Azure 認知服務 (包括 Content Moderator)，將產品影像分類並加以仲裁，以供電子商務案例使用。 您會使用電腦視覺和自訂視覺，將標記 (標籤) 套用至影像，然後建立小組審核 (結合 Content Moderator 的機器學習式技術和人工審核小組)，提供智慧型的仲裁系統。

本教學課程說明如何：

> [!div class="checklist"]
> * 註冊 Content Moderator，並建立審核小組。
> * 使用「內容仲裁」的影像 API 來掃描可能的成人和猥褻內容。
> * 使用電腦視覺服務掃描名人內容 (或其他電腦視覺可偵測的標記)。
> * 使用自訂視覺服務掃描國旗、玩具和筆 (或其他自訂標記) 是否存在。
> * 提出合併的掃描結果以進行人工審核並做出最終決策。

GitHub 上的[範例電子商務目錄仲裁](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)存放庫會提供完整的範例程式碼。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/cognitive-services/)。

## <a name="prerequisites"></a>Prerequisites

- Content Moderator 訂用帳戶金鑰。 請依照[建立認知服務帳戶](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)中的指示，訂閱 Content Moderator 服務並取得金鑰。
- 電腦視覺訂用帳戶金鑰 (與上述相同的指示)。
- 任何 [Visual Studio 2015 或 2017](https://www.visualstudio.com/downloads/) 版本。
- 自訂視覺分類器將會用於每個標籤的一組影像 (在此案例為玩具、筆與美國國旗)。

## <a name="create-a-review-team"></a>建立檢閱小組

如需有關如何註冊 [Content Moderator 審核工具](https://contentmoderator.cognitive.microsoft.com/)並建立審核小組的指示，請參閱[在 Web 上試用 Content Moderator](quick-start.md) 快速入門。 請記下 [認證]  頁面上的 [小組識別碼]  值。

## <a name="create-custom-moderation-tags"></a>建立自訂仲裁標記

接下來，在審核工具中建立自訂標記 (如果您需要協助進行此程序，請參閱[標記](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags)一文)。 在此情況下，我們將新增下列標記：**名人**、**美國**、**國旗**、**玩具**以及**筆**。 並非所有標記都必須是電腦視覺中可偵測的類別 (例如**名人**)；只要您將自訂視覺分類器定型以便在稍後加以偵測，就可以新增您自己的自訂標記。

![設定自訂標記](images/tutorial-ecommerce-tags2.PNG)

## <a name="create-visual-studio-project"></a>建立 Visual Studio 專案

1. 在 Visual Studio 中，開啟 [新增專案] 對話方塊。 依序展開 [已安裝]  和 [Visual C#]  ，然後選取 [主控台應用程式 (.NET Framework)]  。
1. 將應用程式命名為 **EcommerceModeration**，然後按一下 [確定]  。
1. 如果您將此專案新增至現有的方案中，選取此專案作為單一啟始專案。

本教學課程會反白顯示專案的核心程式碼，但是不會涵蓋每一行程式碼。 將 _Program.cs_ 的完整內容從範例專案 ([範例電子商務目錄仲裁](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)) 複製到新專案的 _Program.cs_ 檔案。 接著，逐步執行下列各節以了解專案的運作方式以及如何自行使用該專案。

## <a name="define-api-keys-and-endpoints"></a>定義 API 金鑰與端點

本教學課程使用三個認知服務，因此，它需要三個對應的金鑰和 API 端點。 查看 **Program** 類別中的下列欄位：

[!code-csharp[define API keys and endpoint URIs](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=21-29)]

您必須以訂用帳戶金鑰值來更新 `___Key` 欄位，且必須將 `___Uri` 欄位變更為正確的端點 URL (您稍後將取得自訂視覺金鑰和端點)。 您可以在每個 Azure 資源的 [快速入門]  索引標籤中找到這些值。 使用您稍早建立之審核小組的識別碼填入 `ReviewUri` 欄位的 `YOURTEAMID` 部分。 您稍後將會填入 `CustomVisionUri` 欄位的最後一個部分。

[!INCLUDE [subdomains note](../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="primary-method-calls"></a>主要方法呼叫

請參閱 **Main** 方法中的下列程式碼，此方法會對影像 URL 的清單執行迴圈。 它會使用三個不同的服務分析每個影像、記錄 **ReviewTags** 陣列中套用的標記，然後針對人力仲裁者建立審核 (將影像傳送至 Content Moderator 審核工具)。 您將在接下來的幾節中探索這些方法。 如果您想要，可以控制要傳送哪些影像以供審核，方法是在條件陳述式中使用 **ReviewTags** 陣列檢查已套用的標記。

[!code-csharp[Main: evaluate each image and create review](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=53-70)]

## <a name="evaluateadultracy-method"></a>EvaluateAdultRacy 方法

請參閱 **Program** 類別中的 **EvaluateAdultRacy** 方法。 此方法會採用影像 URL 和金鑰值組陣列作為參數。 它會呼叫 Content Moderator 的影像 API (使用 REST) 來取得成人和猥褻的影像分數。 如果任一個分數大於 0.4 (範圍介於 0 與 1)，它便會將 **ReviewTags** 陣列中對應的值設定為 **True**。

[!code-csharp[define EvaluateAdultRacy method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=73-113)]

## <a name="evaluatecomputervisiontags-method"></a>EvaluateComputerVisionTags 方法

下一個方法會採用影像 URL 和您的電腦視覺訂用帳戶資訊，並分析影像中是否存在名人。 如果找到一個或多個名人，它會將 **ReviewTags** 陣列中對應的值設為 **True**。

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=115-146)]

## <a name="evaluatecustomvisiontags-method"></a>EvaluateCustomVisionTags 方法

接下來，請參閱 **EvaluateCustomVisionTags** 方法，將實際的產品分類，&mdash;在此案例中為國旗、玩具和筆。 請依照[如何建置分類器](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier)指南中的指示，建置您自己的自訂影像分類器，並偵測影像中是否有國旗、玩具和筆 (或您選擇作為自訂標記的任何標記)。 您可以使用 [GitHub 存放庫](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)**sample-images** 資料夾中的映像，以便快速訓練此範例中的一些類別。

![自訂視覺網頁，其中包含筆、玩具和國旗的定型影像](images/tutorial-ecommerce-custom-vision.PNG)

一旦您將分類器定型之後，請取得預測金鑰和預測端點 URL (如果您需要協助擷取它們，請參閱[取得 URL 和預測金鑰](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api#get-the-url-and-prediction-key))，然後將這些值分別指派給您的 `CustomVisionKey` 和 `CustomVisionUri` 欄位。 此方法會使用這些值查詢分類器。 如果分類器在影像中找到一個或多個自訂標記，則此方法會將 **ReviewTags** 陣列中對應的值設為 **True**。

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=148-171)]

## <a name="create-reviews-for-review-tool"></a>為審核工具建立審核

在前面幾節中，您已探索應用程式如何掃描傳入影像中是否有成人和猥褻內容 (Content Moderator)、名人 (電腦視覺) 以及各種其他物件 (自訂視覺) 的方法。 接下來，請參閱 **CreateReview** 方法，它會將影像及其套用的所有標記 (當作_中繼資料_傳入) 上傳到 Content Moderator 審核工具。

[!code-csharp[define CreateReview method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=173-196)]

這些影像將會顯示在 [Content Moderator 審核工具](https://contentmoderator.cognitive.microsoft.com/)的 [審核] 索引標籤中。

![Content Moderator 審核工具的螢幕擷取畫面，其中包含數個影像及其反白顯示的標記](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="submit-a-list-of-test-images"></a>提交測試影像的清單

如同您在 **Main** 方法中所見，此程式會尋找包含 _Urls.txt_ 檔案的 "C:Test" 目錄，其中包含映像 URL 的清單。 建立此檔案和目錄，或將路徑變更為指向您的文字檔案。 然後在此檔案中填入您想要測試的影像 URL。

[!code-csharp[Main: set up test directory, read lines](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=38-51)]

## <a name="run-the-program"></a>執行程式

如果您已遵循上述所有步驟進行，此程式應該會處理每個影像 (查詢所有三個服務中的相關標記)，然後將包含標記資訊的影像上傳至 Content Moderator 審核工具。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您設定程式來分析產品影像、依產品類型加以標記，並允許審核小組對內容仲裁做出明智的決策。 接下來，請深入了解影像仲裁的詳細資料。

> [!div class="nextstepaction"]
> [審查已仲裁的影像](./review-tool-user-guide/review-moderated-images.md)

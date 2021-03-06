---
title: 安裝適用于 Unity 的 Azure 空間錨點
description: 設定 Unity 專案以使用 Azure 空間錨點
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 08/17/2020
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 57ead9636b7218ecfc7d72bb605b469d6a7d1ac6
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536345"
---
# <a name="configuring-azure-spatial-anchors-in-a-unity-project"></a>在 Unity 專案中設定 Azure 空間錨點

本指南將示範如何在 Unity 專案中開始使用 Azure 空間錨點 SDK。

## <a name="requirements"></a>需求

Azure 空間錨點目前支援具有下列設定的 Unity 2019.4 (LTS) 。

* Azure 空間錨點 2.4.0 + 支援 Unity 2019.4 with AR Foundation 3.1。

## <a name="configuring-a-project"></a>設定專案

### <a name="add-the-unity-package-manager-packages-to-your-project"></a>[將 Unity 封裝管理員套件新增至您的專案](#tab/UPMPackage)

適用于 Unity 的 Azure 空間錨點目前使用 Unity 封裝管理員 (UPM) 套件來散發。 您可以在我們的 [NPM](https://bintray.com/microsoft/AzureMixedReality-NPM)登錄中找到這些套件。 若要深入瞭解如何在 Unity 專案中使用範圍套件登錄，請參閱 [這裡](https://docs.unity3d.com/Manual/upm-scoped.html)的官方 Unity 檔。

#### <a name="add-the-registry-to-your-unity-project"></a>將登錄新增至 Unity 專案

1. 在檔案瀏覽器中，流覽至您的 Unity 專案 `Packages` 資料夾。 在文字編輯器中開啟專案資訊清單檔 `manifest.json` 。
2. 在檔案頂端的相同層級 `dependencies` 中，新增下列專案，以將 Azure 空間錨點登錄加入至您的專案。 此 `scopedRegistries` 專案會告知 Unity 要在哪裡尋找 Azure 空間錨點 SDK 套件。

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-19&highlight=2-10)]

#### <a name="add-the-sdk-packages-to-your-unity-project"></a>將 SDK 套件 () 新增至您的 Unity 專案

| 平台 | 套件名稱                                    |
|----------|-------------------------------------------------|
| Android  | .com. azure.-- |
| iOS      | spatial-anchors-sdk. ios 版     |
| HoloLens | .com. 空間-錨點-sdk. windows |

1. 針對您想要在專案中支援的每個平臺 (Android/iOS/HoloLens) ，將套件名稱和套件版本的專案新增至 `dependencies` 專案資訊清單中的區段。 請參閱下面的範例。

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-22&highlight=12-14)]

2. 儲存並關閉 `manifest.json` 檔案。 當您返回 Unity 時，Unity 應該會自動偵測專案資訊清單變更並取出指定的封裝。 您可以展開 `Packages` 專案查看中的資料夾，以確認已匯入正確的套件。

#### <a name="android-only-configure-the-maintemplategradle-file"></a>僅限 Android：設定 >maintemplate.json gradle 檔案

1. 移至 [編輯] > [專案設定] > [播放器]。
2. 在 [**播放程式設定**] 的 [偵測**器] 面板**中，選取 [ **Android** ] 圖示。
3. 在 [ **組建** ] 區段下，選取 [ **自訂主要 Gradle 範本** ] 核取方塊，以在上產生自訂 Gradle 範本 `Assets\Plugins\Android\mainTemplate.gradle` 。
4. 在文字編輯器中開啟您的 `mainTemplate.gradle` 檔案。 
5. 在 `dependencies` 區段中，貼上下列相依性：

    ```gradle
    implementation('com.squareup.okhttp3:okhttp:[3.11.0]')
    implementation('com.microsoft.appcenter:appcenter-analytics:[1.10.0]')
    ```

一切完成後，您的 `dependencies` 區段看起來應該像這樣：

[!code-gradle[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-android-gradle-setup.md?range=9-13&highlight=3-4)]

### <a name="import-the-asset-package"></a>[匯入資產套件](#tab/UnityAssetPackage)

> [!WARNING]
> Azure 空間錨點 SDK 的 Unity 資產套件散發將在 SDK 版本2.5.0 之後淘汰。

1. `AzureSpatialAnchors.unitypackage`針對您想要從[GitHub 版本](https://github.com/Azure/azure-spatial-anchors-samples/releases)取得目標的版本下載檔案。 
2. 遵循 [此處](https://docs.unity3d.com/Manual/AssetPackagesImport.html) 的指示，將 Unity 資產套件匯入您的專案中。    

---

## <a name="next-steps"></a>接下來的步驟

> [!div class="nextstepaction"]
> [如何：在 Unity 中建立及尋找錨點](./create-locate-anchors-unity.md)

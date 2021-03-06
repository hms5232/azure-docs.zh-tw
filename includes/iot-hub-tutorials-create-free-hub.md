---
title: 包含檔案
description: 包含檔案
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/19/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 367a0b1d17f8d5ebe4f46835ace963b00e75354e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "68229343"
---
若要使用 Azure 入口網站建立 IoT 中樞：

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 選取 [建立資源] > [物聯網] > [IoT 中樞]。

    ![選擇安裝 IoT 中樞](media/iot-hub-tutorials-create-free-hub/selectiothub.png)

1. 若要建立免費層 IoT 中樞，請使用下表中的值：

    | 設定 | 值 |
    | ------- | ----- |
    | 訂用帳戶 | 在下拉式清單中選取您的 Azure 訂用帳戶。 |
    | 資源群組 | 新建。 本教學課程使用 **tutorials-iot-hub-rg** 名稱。 |
    | 區域 | 本教學課程使用**美國西部**。 您可以選擇最接近您所在地的區域。 |
    | 名稱 | 下列螢幕擷取畫面會使用 **tutorials-iot-hub** 名稱。 在建立中樞時，您必須選擇您自己的唯一名稱。 |

    ![中樞設定 1](media/iot-hub-tutorials-create-free-hub/hubdefinition-1.png)

    | 設定 | 值 |
    | ------- | ----- |
    | 定價與級別層 | F1 免費。 您的訂用帳戶中只能有一個免費的中樞。 |
    | IoT 中樞單位 | 1 |

    ![中樞設定 2](media/iot-hub-tutorials-create-free-hub/hubdefinition-2.png)

1. 按一下 [建立]。 建立中樞可能需要數分鐘的時間。

    ![中樞設定 3](media/iot-hub-tutorials-create-free-hub/hubdefinition-3.png)

1. 記下您選擇的 IoT 中樞名稱。 您稍後會在本教學課程中使用此值。

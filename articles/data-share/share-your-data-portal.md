---
title: 在您的組織外部共用 (Azure 入口網站) - Azure Data Share 快速入門
description: 在本快速入門中了解如何使用 Azure Data Share 與客戶和合作夥伴共用資料。
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: quickstart
ms.date: 08/19/2020
ms.openlocfilehash: 41598c04af78d4366435259357d8f897ac178942
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489930"
---
# <a name="quickstart-share-data-using-azure-data-share-in-the-azure-portal"></a>快速入門：在 Azure 入口網站中使用 Azure Data Share 來共用資料

在本快速入門中，您將了解如何使用 Azure 入口網站來設定新的 Azure Data Share。

## <a name="prerequisites"></a>先決條件

Azure 訂用帳戶：如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/)。


## <a name="create-a-data-share-account"></a>建立 Data Share 帳戶

在 Azure 資源群組中建立 Azure Data Share 資源。

1. 登入 [Azure 入口網站](https://portal.azure.com/)。

1. 選取入口網站左上角的 [建立資源] 按鈕 (+)。

1. 搜尋「Data Share」**。

1. 選取 [Data Share]，然後選取 [建立]。

1. 使用下列資訊填寫 Azure Data Share 資源的基本詳細資料。 

   **設定** | **建議的值** | **欄位描述**
   |---|---|---|
   | 訂用帳戶 | 您的訂用帳戶 | 選取您要用於資料共用帳戶的 Azure 訂用帳戶。|
   | 資源群組 | *test-resource-group* | 使用現有資源群組，或建立新的資源群組。 |
   | Location | *美國東部 2* | 選取資料共用帳戶的區域。
   | 名稱 | *datashareaccount* | 指定資料共用帳戶的名稱。 |

1. 選取 [檢閱 + 建立]，然後選取 [建立] 來佈建您的資料共用帳戶。 佈建新的資料共用帳戶一般需要大約不到 2 分鐘的時間。

1. 部署完成後，請選取 [移至資源]  。

## <a name="create-a-share"></a>建立共用

1. 瀏覽至 [Data Share 概觀] 頁面。

   ![共用您的資料](./media/share-receive-data.png "共用您的資料") 

1. 選取 [開始共用資料]。

1. 選取 [建立]  。

1. 填寫共用的詳細資料。 指定名稱、共用類型、共用內容的說明和使用規定 (選擇性)。 

   ![EnterShareDetails](./media/enter-share-details.png "輸入共用詳細資料") 

1. 選取 [繼續]。

1. 若要將資料集新增至共用，請選取 [新增資料集]。 

   ![將資料集新增至共用](./media/datasets.png "資料集")

1. 選取您想要新增的資料集類型。 根據您在先前的步驟中選取的共用類型 (快照集或就地)，您將看到不同的資料集類型清單。 如果從 Azure SQL Database 或 Azure Synapse Analytics 共用，系統會提示您提供一些 SQL 認證。 使用您建立為必要條件的使用者進行驗證。

   ![AddDatasets](./media/add-datasets.png "新增資料集")    

1. 瀏覽至您想要共用的物件，然後選取 [新增資料集]。 

   ![SelectDatasets](./media/select-datasets.png "選取資料集")    

1. 在 [收件者] 索引標籤中，藉由選取 [+ 新增收件者] 來輸入資料取用者的電子郵件地址。

   ![AddRecipients](./media/add-recipient.png "新增收件者") 

1. 選取 [繼續]。

1. 如果您已選取快照集共用類型，則可以設定快照集排程，將資料的更新提供給資料取用者。 

   ![EnableSnapshots](./media/enable-snapshots.png "啟用快照集") 

1. 選取開始時間和週期間隔。 

1. 選取 [繼續]。

1. 在 [檢閱 + 建立] 索引標籤中，檢閱 [套件內容]、[設定]、[收件者] 及 [同步處理設定]。 選取 [建立]  。

Azure Data Share 現已建立完成，而且 Data Share 的收件者現已準備好接受邀請。

## <a name="clean-up-resources"></a>清除資源

當不再需要資源時，請移至 [Data Share 概觀] 頁面，然後選取 [刪除] 來移除資源。

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已了解如何建立 Azure Data Share。 若要了解資料取用者要如何接受和接收資料共用，請繼續閱讀[接受和接收資料](subscribe-to-data-share.md)教學課程。 

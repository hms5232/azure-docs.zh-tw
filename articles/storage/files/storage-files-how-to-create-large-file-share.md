---
title: 啟用和建立大型檔案共用-Azure 檔案儲存體
description: 在本文中，您將瞭解如何啟用和建立大型檔案共用。
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 05/29/2020
ms.author: rogarana
ms.subservice: files
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 190aaae81d51434b57b5aaa6817a443dc541d26e
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89069131"
---
# <a name="enable-and-create-large-file-shares"></a>啟用和建立大型檔案共用

當您在儲存體帳戶上啟用大型檔案共用時，您的檔案共用最多可擴大至 100 TiB。 您可以針對現有的檔案共用，在現有的儲存體帳戶上啟用這種調整。

## <a name="prerequisites"></a>必要條件

- 如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/)。
- 如果您想要使用 Azure CLI，請[安裝最新版本](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。
- 如果您想要使用 Azure PowerShell 模組，請 [安裝最新版本](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.6.0)。

## <a name="restrictions"></a>限制

目前，您只能在已啟用大型檔案共用的帳戶上使用本機冗余儲存體 (LRS) 或區域冗余儲存體 (ZRS) 。 您無法使用異地區域冗余儲存體 (GZRS) 、異地冗余儲存體 (GRS) 、讀取權限異地冗余儲存體 (GRS) ，或讀取權限地理區域冗余儲存體 (RA-GZRS) 。

在帳戶上啟用大型檔案共用是無法復原的程式。 啟用之後，您將無法將您的帳戶轉換為 GZRS、GRS、RA-GRS 或 RA-GZRS。

## <a name="create-a-new-storage-account"></a>建立新的儲存體帳戶

# <a name="portal"></a>[入口網站](#tab/azure-portal)

1. 登入 [Azure 入口網站](https://portal.azure.com)。
1. 在 Azure 入口網站中，選取 [所有服務]。 
1. 在資源清單中，輸入 **儲存體帳戶**。 當您輸入時，清單會根據您輸入的文字進行篩選。 選取 [ **儲存體帳戶**]。
1. 在出現的 [ **儲存體帳戶** ] 視窗上，選取 [ **新增**]。
1. 選取您將用來建立儲存體帳戶的訂用帳戶。
1. 在 [資源群組]**** 欄位下方，選取 [新建]****。 為新的資源群組輸入名稱。

    ![示範如何在入口網站中建立資源群組的螢幕擷取畫面](media/storage-files-how-to-create-large-file-share/create-large-file-share.png)

1. 接下來，輸入儲存體帳戶的名稱。 此名稱在整個 Azure 中必須是唯一的。 名稱的長度也必須是3到24個字元，而且只能有數位和小寫字母。
1. 選取儲存體帳戶的位置。
1. 將複寫設定為 **本機多餘的儲存體** 或 **區域多餘的儲存體**。
1. 將這些欄位保留為其預設值：

   |欄位  |值  |
   |---------|---------|
   |部署模型     |Resource Manager         |
   |效能     |標準         |
   |帳戶種類     |StorageV2 (一般用途 v2)         |
   |存取層     |經常性存取層         |

1. 選取 [ **Advanced**]，然後選取 [**大型檔案共用**] 右邊的 [**啟用**] 選項按鈕。
1. 選取 [檢閱 + 建立]  ，以檢閱您的儲存體帳戶設定並建立帳戶。

    ![Azure 入口網站中新儲存體帳戶的 [已啟用] 選項按鈕的螢幕擷取畫面](media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png)

1. 選取 [建立]。

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

首先，請 [安裝最新版本的 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ，讓您可以啟用大型檔案共用。

若要建立已啟用大型檔案共用的儲存體帳戶，請使用下列命令。 `<yourStorageAccountName>`將、 `<yourResourceGroup>` 和取代 `<yourDesiredRegion>` 為您的資訊。

```azurecli-interactive
## This command creates a large file share–enabled account. It will not support GZRS, GRS, RA-GRS, or RA-GZRS.
az storage account create --name <yourStorageAccountName> -g <yourResourceGroup> -l <yourDesiredRegion> --sku Standard_LRS --kind StorageV2 --enable-large-file-share
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

首先，請 [安裝最新版的 PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0) ，讓您可以啟用大型檔案共用。

若要建立已啟用大型檔案共用的儲存體帳戶，請使用下列命令。 `<yourStorageAccountName>`將、 `<yourResourceGroup>` 和取代 `<yourDesiredRegion>` 為您的資訊。

```powershell
## This command creates a large file share–enabled account. It will not support GZRS, GRS, RA-GRS, or RA-GZRS.
New-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -Location <yourDesiredRegion> -SkuName Standard_LRS -EnableLargeFileShare;
```
---

## <a name="enable-large-files-shares-on-an-existing-account"></a>在現有的帳戶上啟用大型檔案共用

您也可以在現有的帳戶上啟用大型檔案共用。 如果您啟用大型檔案共用，您將無法轉換成 GZRS、GRS、RA-GRS 或 RA-GZRS。 在此儲存體帳戶上啟用大型檔案共用是無法復原的。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

1. 開啟 [Azure 入口網站](https://portal.azure.com)，然後移至您要啟用大型檔案共用的儲存體帳戶。
1. 開啟儲存體帳戶， **然後選取 [** 設定]。
1. 選取 [在**大型檔案共用**上**啟用**]，然後選取 [**儲存**]。
1. 選取 **[總覽**]，然後選取 [重新整理 **]。**

![在 Azure 入口網站中的現有儲存體帳戶上選取 [啟用] 選項按鈕](media/storage-files-how-to-create-large-file-share/enable-large-file-shares-on-existing.png)

您現在已在儲存體帳戶上啟用大型檔案共用。 接下來，您必須 [更新現有共用的配額](#expand-existing-file-shares) ，以利用增加的容量和規模。

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要在現有的帳戶上啟用大型檔案共用，請使用下列命令。 `<yourStorageAccountName>`將和取代 `<yourResourceGroup>` 為您的資訊。

```azurecli-interactive
az storage account update --name <yourStorageAccountName> -g <yourResourceGroup> --enable-large-file-share
```

您現在已在儲存體帳戶上啟用大型檔案共用。 接下來，您必須 [更新現有共用的配額](#expand-existing-file-shares) ，以利用增加的容量和規模。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要在現有的帳戶上啟用大型檔案共用，請使用下列命令。 `<yourStorageAccountName>`將和取代 `<yourResourceGroup>` 為您的資訊。

```powershell
Set-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -EnableLargeFileShare
```

您現在已在儲存體帳戶上啟用大型檔案共用。 接下來，您必須 [更新現有共用的配額](#expand-existing-file-shares) ，以利用增加的容量和規模。

---

## <a name="create-a-large-file-share"></a>建立大型檔案共用

在您的儲存體帳戶上啟用大型檔案共用之後，您可以在該帳戶中建立具有較高配額的檔案共用。 

# <a name="portal"></a>[入口網站](#tab/azure-portal)

建立大型檔案共用與建立標準檔案共用幾乎完全相同。 主要差異在於您可以將配額設定為 100 TiB。

1. 從您的儲存體帳戶選取 [檔案 **共用**]。
1. 選取 [ **+ 檔案共用**]。
1. 輸入檔案共用的名稱。 您也可以設定您想要的配額大小，最多可達 100 TiB。 然後選取 [建立]。 

![顯示 [名稱] 和 [配額] 方塊的 Azure 入口網站 UI](media/storage-files-how-to-create-large-file-share/large-file-shares-create-share.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要建立大型檔案共用，請使用下列命令。 `<yourStorageAccountName>`將、 `<yourStorageAccountKey>` 和取代 `<yourFileShareName>` 為您的資訊。

```azurecli-interactive
az storage share create --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName>
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要建立大型檔案共用，請使用下列命令。 `<YourStorageAccountName>`將、 `<YourStorageAccountKey>` 和取代 `<YourStorageAccountFileShareName>` 為您的資訊。

```powershell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
New-AzStorageShare -Name $shareName -Context $ctx
```
---

## <a name="expand-existing-file-shares"></a>展開現有的檔案共用

在您的儲存體帳戶上啟用大型檔案共用之後，您也可以將該帳戶中的現有檔案共用擴充至較高的配額。 

# <a name="portal"></a>[入口網站](#tab/azure-portal)

1. 從您的儲存體帳戶選取 [檔案 **共用**]。
1. 以滑鼠右鍵按一下您的檔案共用，然後選取 [ **配額**]。
1. 輸入您想要的新大小，然後選取 **[確定]**。

![具有現有檔案共用配額的 Azure 入口網站 UI](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要將配額設定為大小上限，請使用下列命令。 `<yourStorageAccountName>`將、 `<yourStorageAccountKey>` 和取代 `<yourFileShareName>` 為您的資訊。

```azurecli-interactive
az storage share update --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName> --quota 102400
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要將配額設定為大小上限，請使用下列命令。 `<YourStorageAccountName>`將、 `<YourStorageAccountKey>` 和取代 `<YourStorageAccountFileShareName>` 為您的資訊。

```powershell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
# update quota
Set-AzStorageShareQuota -ShareName $shareName -Context $ctx -Quota 102400
```
---

## <a name="next-steps"></a>後續步驟

* [在 Windows 上連接並掛接檔案共用](storage-how-to-use-files-windows.md)
* [連接及掛接 Linux 上的檔案共用](storage-how-to-use-files-linux.md)
* [在 macOS 上連接並掛接檔案共用](storage-how-to-use-files-mac.md)

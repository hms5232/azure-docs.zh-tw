---
title: 將 Azure Analysis Services 移至不同的區域 |Microsoft Docs
description: 說明如何將 Azure Analysis Services 資源移至不同的區域。
author: minewiskan
ms.service: azure-analysis-services
ms.topic: how-to
ms.date: 06/09/2020
ms.author: owend
ms.reviewer: minewiskan
ms.custom: references_regions
ms.openlocfilehash: 4844f3e34a6b49559affbb4d4ed7bc5b5e38e538
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87050356"
---
# <a name="move-analysis-services-to-a-different-region"></a>將 Analysis Services 移至不同的區域

本文說明如何將 Analysis Services 伺服器資源移到不同的 Azure 區域。 您可能會因為許多原因而將您的伺服器移至另一個區域，例如，利用更接近使用者的 Azure 區域、僅使用特定區域中支援的服務方案，或符合內部原則和治理需求。 

在此和相關聯的連結文章中，您將瞭解如何：

> [!div class="checklist"]
> 
> * 將來源伺服器模型資料庫備份至[Blob 儲存體](../storage/blobs/storage-blobs-introduction.md)。
> * 匯出來源伺服器[資源範本](../azure-resource-manager/templates/overview.md)。
> * 取得儲存體[共用存取簽章（SAS）](../storage/common/storage-sas-overview.md)。
> * 修改資源範本。
> * 部署範本以建立新的目標伺服器。
> * 將模型資料庫還原到新的目標伺服器。
> * 確認新的目標伺服器和資料庫。
> * 刪除來源伺服器。

本文說明如何使用資源範本，將具有**基本**設定的單一 Analysis Services 伺服器遷移至相同訂用帳戶中的不同區域*和*資源群組。 使用範本會保留設定的伺服器屬性，確保目標伺服器的設定具有與來源伺服器相同的屬性（區域和資源群組除外）。 本文不會說明如何移動可能屬於相同資源群組（例如資料來源、儲存體和閘道資源）的相關資源。 

將伺服器移到不同的區域之前，建議您先建立詳細的方案。 請考慮其他可能也需要移動的資源，例如閘道和儲存體。 有了任何計畫，在移動實際執行伺服器之前，請務必先使用測試伺服器來完成一或多個試用版移動作業。

> [!IMPORTANT]
> 用戶端應用程式和連接字串會使用完整伺服器名稱連接到 Analysis Services，這是包含伺服器所在區域的 Uri。 例如： `asazure://westcentralus.asazure.windows.net/advworks01` 。 將伺服器移到不同的區域時，您實際上是在不同的區域中建立新的伺服器資源，在伺服器名稱 Uri 中會有不同的區域。 腳本中使用的用戶端應用程式和連接字串必須使用新的伺服器名稱 Uri 連接到新的伺服器。 使用[伺服器名稱別名](analysis-services-server-alias.md)可以減輕伺服器名稱 Uri 必須變更的位置數目，但是在區域移動之前，必須先加以執行。

> [!IMPORTANT]
> Azure 區域使用不同的 IP 位址範圍。 如果您已針對您的伺服器和/或儲存體帳戶所在的區域設定防火牆例外狀況，可能需要設定不同的 IP 位址範圍。 若要深入瞭解，請參閱[Analysis Services 網路連線的相關常見問題](analysis-services-network-faq.md)。

> [!NOTE]
> 本文說明如何從來源伺服器區域中的儲存體容器，將資料庫備份還原至目標伺服器。 在某些情況下，從不同的區域還原備份可能會有不佳的效能，特別是針對大型資料庫。 為了在資料庫還原期間獲得最佳效能，請在目標伺服器區域中遷移或建立新的儲存體容器。 將資料庫還原至目標伺服器之前，請先將 .abf 備份檔案從來源區域儲存體容器複製到目的地區域儲存體容器。 在本文範圍外，在某些情況下，特別是使用非常大型的資料庫，從來源伺服器編寫資料庫的腳本，然後重新建立，然後在目標伺服器上進行處理以載入資料庫資料，可能比使用備份/還原更符合成本效益。

> [!NOTE]
> 如果使用內部部署資料閘道連接到資料來源，您也必須將閘道資源移到目標伺服器區域。 若要深入瞭解，請參閱[安裝和設定內部部署資料閘道](analysis-services-gateway-install.md)。

## <a name="prerequisites"></a>必要條件

- **Azure 儲存體帳戶**：儲存 .abf 備份檔案所需。
- **SQL Server Management Studio （SSMS）**：備份和還原模型資料庫時的必要。
- **Azure PowerShell**(英文)。 只有在您選擇使用 PowerShell 完成這項工作時才需要。

## <a name="prepare"></a>準備

### <a name="backup-model-databases"></a>備份模型資料庫

如果尚未為來源伺服器設定**存放裝置設定**，請依照[設定存放裝置設定](analysis-services-backup.md#configure-storage-settings)中的步驟進行。

設定存放裝置設定時，請依照[備份](analysis-services-backup.md#backup)中的步驟來建立模型資料庫。在儲存體容器中 .abf 備份。 您稍後會將 .abf 備份還原到新的目標伺服器。


### <a name="export-template"></a>匯出範本

此範本包含來源伺服器的設定屬性。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

使用 Azure 入口網站匯出範本：

1. 登入 [Azure 入口網站](https://portal.azure.com)。

2. 選取 [**所有資源**]，然後選取您的 Analysis Services 伺服器。

3. 選取 >**設定**] [  >  **匯出範本**]。

4. 在 [**匯出範本**] 分頁中，選擇 [**下載**]。

5. 找出您從入口網站下載的 .zip 檔案，然後將該檔案解壓縮至資料夾。

   Zip 檔案包含組成範本的 json 檔案，以及部署新伺服器所需的參數。


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要使用 PowerShell 匯出範本：

1. 使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) 命令登入 Azure 訂用帳戶，並遵循畫面上的指示操作：

   ```azurepowershell-interactive
   Connect-AzAccount
   ```
2. 如果您的身分識別與多個訂用帳戶相關聯，請將您的使用中訂用帳戶設為您想要移動之伺服器資源的訂用帳戶。

   ```azurepowershell-interactive
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

3. 匯出來源伺服器的範本。 這些命令會將 ResourceGroupName as filename 的 json 範本儲存到您目前的目錄。

   ```azurepowershell-interactive
   $resource = Get-AzResource `
     -ResourceGroupName <resource-group-name> `
     -ResourceName <server-account-name> `
     -ResourceType Microsoft.AnalysisServices/servers
   Export-AzResourceGroup `
     -ResourceGroupName <resource-group-name> `
     -Resource $resource.ResourceId
   ```
---

### <a name="get-storage-shared-access-signature-sas"></a>取得儲存體共用存取簽章（SAS）

從範本部署目標伺服器時，需要使用者委派 SAS 權杖（做為 Uri）來指定包含資料庫備份的儲存體容器。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

若要使用入口網站來取得共用存取簽章：

1. 在入口網站中，選取用來備份伺服器資料庫的儲存體帳戶。

2. 選取 [**儲存體總管**]，然後展開 [ **BLOB 容器**]。 

3. 以滑鼠右鍵按一下儲存體容器，然後選取 [**取得共用存取**簽章]。

    :::image type="content" source="media/move-between-regions/get-sas.png" alt-text="取得 SAS":::

4. 在 [**共用存取**簽章] 中，選取 [**建立**]。 根據預設，SAS 會在24小時內過期。

5. 複製並儲存**URI**。 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要使用 PowerShell 來取得共用存取簽章，請遵循[使用 Powershell 建立容器或 blob 的使用者委派 SAS](../storage/blobs/storage-blob-user-delegation-sas-create-powershell.md#create-a-user-delegation-sas-for-a-blob)中的步驟。

---

### <a name="modify-the-template"></a>修改範本

使用文字編輯器來修改您所匯出之檔案上的 template.js，並變更 [區域] 和 [blob] 容器屬性。 

若要修改範本：

1. 在文字編輯器的 [**位置**] 屬性中，指定新的目的地區域。 在**backupBlobContainerUri**屬性中，貼上儲存體容器 URI 與 SAS 金鑰。 

    下列範例會將伺服器 advworks1 的目的地區域設定為 `South Central US` ，並指定具有共用存取簽章的儲存體容器 Uri： 

    ```json
    "resources": [
        {
            "type": "Microsoft.AnalysisServices/servers",
            "apiVersion": "2017-08-01",
            "name": "[parameters('servers_advworks1_name')]",
            "location": "South Central US",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "capacity": 1
            },
            "properties": {
                "asAdministrators": {
                    "members": [
                        "asadmins@adventure-works.com"
                    ]
                },
                "backupBlobContainerUri": "https://storagenorthcentralus.blob.core.windows.net/backup?sp=rl&st=2020-06-01T19:30:42Z&se=2020-06-02T19:30:42Z&sv=2019-10-10&sr=c&sig=PCQ4s9RujJkxu89gO4tiDTbE3%2BFECx6zAdcv8x0cVUQ%3D",
                "querypoolConnectionMode": "All"
            }
        }
    ]         
    ```
2. 儲存範本。

#### <a name="regions"></a>區域

若要取得 Azure 區域，請參閱[azure 位置](https://azure.microsoft.com/global-infrastructure/locations/)。 若要使用 PowerShell 取得區域，請執行[get-azlocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0)命令。

```azurepowershell-interactive
   Get-AzLocation | format-table 
```

## <a name="move"></a>移動

若要在不同的區域中部署新的伺服器資源，您將會在先前章節中所匯出和修改的檔案**上使用template.js** 。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

1. 在入口網站中，選取 [**建立資源**]。

2. 在 **[搜尋 Marketplace**] 中，輸入**範本部署**，然後按**enter**。

3. 選取 [**範本部署**]。

4. 選取 [建立]。

5. 選取 [在編輯器中組建您自己的範本]。

6. 選取 [**載入**檔案]，然後依照指示在您匯出和修改的檔案**上載入template.js** 。

7. 確認範本編輯器顯示新目標伺服器的正確屬性。

8. 選取 [儲存]。

9. 輸入或選取屬性值：

    - **訂用帳戶**：選取 Azure 訂用帳戶。
    
    - **資源群組**：選取 [**新建**]，然後輸入資源組名。 您可以選取現有的資源群組，但前提是它還沒有包含相同名稱的 Analysis Services 伺服器。
    
    - **位置**：選取您在範本中指定的相同區域。

10. 選取 [**審查並建立**]。

11. 檢查條款和基本概念，然後選取 [**建立**]。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用下列命令來部署您的範本：

```azurepowershell-interactive
   $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
   $location = Read-Host -Prompt "Enter the location (i.e. South Central US)"

   New-AzResourceGroup -Name $resourceGroupName -Location "$location"
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "<local template file>"  
   ```
---

### <a name="get-target-server-uri"></a>取得目標伺服器 Uri

若要從 SSMS 連接到新的目標伺服器以還原 model 資料庫，您必須取得新的目標伺服器 Uri。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

若要在入口網站中取得伺服器 Uri：

1. 在入口網站中，移至新的目標伺服器資源。

2. 在 [**總覽**] 頁面上，複製 [**伺服器名稱**Uri]。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要使用 PowerShell 取得伺服器 Uri，請使用下列命令：

```azurepowershell-interactive
   $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
   $serverName = Read-Host -Prompt "Enter the server name (i.e. advworks2)"

   Get-AzAnalysisServicesServer -ResourceGroupName $resourceGroupName -Name "$serverName"
```
從輸出複製**ServerFullName** 。

---

### <a name="restore-model-database"></a>還原模型資料庫

依照[還原](analysis-services-backup.md#restore)中所述的步驟，將模型資料庫 .abf 備份還原到新的目標伺服器。

選擇性：還原 model 資料庫之後，請處理模型和資料表，以重新整理資料來源中的資料。 若要使用 SSMS 來處理模型和資料表：

1. 在 SSMS 中，以滑鼠右鍵按一下模型資料庫 >**處理資料庫**]。

2. 展開 [**資料表]**，以滑鼠右鍵按一下資料表。 在 **[處理資料表**] 中，選取 [所有資料表]，然後選取 **[確定]**。

## <a name="verify"></a>驗證

1. 在入口網站中，移至新的目標伺服器。

2. 在 [總覽] 頁面上，于**Analysis Services 伺服器上**的 [模型] 中，確認已還原的模型出現。

3. 使用 Power BI 或 Excel 之類的用戶端應用程式連接到新伺服器上的模型。 確認模型物件（例如資料表、量值、階層）是否出現。 

4. 執行任何自動化腳本。 確認它們已順利執行。

選擇性： [ALM](http://alm-toolkit.com/)工具組是一種*開放原始*碼工具，可用於比較和管理 Power BI 資料集*和*Analysis Services 表格式模型資料庫。 使用此工具組來連接到來源和目標伺服器資料庫，並進行比較。 如果您的資料庫移轉成功，模型物件將會是相同的定義。 

:::image type="content" source="media/move-between-regions/alm-toolkit.png" alt-text="ALM 工具組":::

## <a name="clean-up-resources"></a>清除資源

確認用戶端應用程式可以連線到新的伺服器，且所有自動化腳本都正確執行之後，請刪除您的來源伺服器。 

# <a name="portal"></a>[入口網站](#tab/azure-portal)

若要從入口網站刪除來源伺服器：

在來源伺服器的 **[總覽**] 頁面中，選取 [**刪除**]。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

若要使用 PowerShell 刪除來源伺服器，請使用 AzAnalysisServicesServer 命令。

```azurepowershell-interactive
Remove-AzAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

---

> [!NOTE]
> 完成區域移動之後，建議新的目標伺服器使用相同區域中的儲存體容器來進行備份，而不是來源伺服器區域中的儲存體容器。 

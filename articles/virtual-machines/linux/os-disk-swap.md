---
title: 使用 CLI 在 OS 磁片之間交換
description: 使用 CLI 變更 Azure 虛擬機器所使用的作業系統磁碟。
author: cynthn
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: adcbc5b0ba0056c79576e8d4c8ded2150b206513
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87371867"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>使用 CLI 變更 Azure VM 所使用的 OS 磁碟


如果您目前有 VM，但想要交換備份磁碟的磁碟或另一個 OS 磁碟，可以使用 Azure CLI 來交換 OS 磁碟。 您不需要刪除及重新建立虛擬機器。 甚至可以使用另一個資源群組中的受控磁碟，只要該磁碟並非使用中即可。

必須停止\解除配置虛擬機器，然後才能使用不同受控磁碟的資源識別碼取代該受控磁碟的資源識別碼。 

請確定虛擬機器大小和儲存類型能和您想要附加的磁碟相容。 舉例而言，如果您想要使用的磁碟是進階儲存體，虛擬機器就需能支援進階儲存體 (例如 DS 系列的大小)。

本文需要 Azure CLI 2.0.25 版或更高版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI]( /cli/azure/install-azure-cli)。 


使用 [az disk list](/cli/azure/disk) 來取得資源群組中的磁碟清單。

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


使用 [az vm stop](/cli/azure/vm)，在交換磁碟之前停止\解除配置 VM。

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


使用 [az vm update](/cli/azure/vm#az-vm-update)，並搭配含有新磁碟之完整資源識別碼的 `--osdisk` 參數 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
使用 [az vm start](/cli/azure/vm) 來啟動 VM。

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**後續步驟**

若要建立磁碟複本，請參閱[製作磁碟的快照](snapshot-copy-managed-disk.md)。

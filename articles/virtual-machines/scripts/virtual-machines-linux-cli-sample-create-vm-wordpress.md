---
title: Azure CLI 指令碼範例 - 使用 WordPress 建立 Linux VM
description: Azure CLI 指令碼範例 - 使用 WordPress 建立 Linux VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 17bb5a3cc7f612c1d702925ae0551c99386bbd69
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501418"
---
# <a name="create-a-vm-with-wordpress"></a>建立含 WordPress 的 VM

此指令碼會建立虛擬機器，然後使用 Azure 虛擬機器自訂指令碼擴充功能來安裝 WordPress。 執行指令碼之後，您可以在 `http://<public IP of VM>/wordpress` 上存取 WordPress 設定站台。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>範例指令碼

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>清除部署

執行下列命令來移除資源群組、VM 和所有相關資源。

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>指令碼說明

此指令碼使用下列命令來建立資源群組、虛擬機器和所有相關資源。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [az group create](/cli/azure/group) | 建立用來存放所有資源的資源群組。 |
| [az vm create](/cli/azure/vm) | 建立虛擬機器，並將它連線到網路卡、虛擬網路、子網路及 NSG。 此命令也會指定要使用的虛擬機器映像和管理認證。  |
| [az vm open-port](/cli/azure/vm) | 建立網路安全性群組規則以允許輸入流量。 在此範例中，會開放連接埠 80 供 HTTP 流量使用。 |
| [az vm extension set](/cli/azure/vm) | 將自訂指令碼擴充功能新增至會叫用指令碼來安裝 WordPress 的虛擬機器。 |
| [az group delete](/cli/azure/vm/extension) | 刪除資源群組，包括所有的巢狀資源。 |

## <a name="next-steps"></a>後續步驟

如需 Azure CLI 的詳細資訊，請參閱 [Azure CLI 文件](/cli/azure)。

您可以在 [Azure Linux VM 文件](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)中找到其他的虛擬機器 CLI 指令碼範例。

---
title: 使用 PowerShell 將受控識別存取權指派給資源-Azure AD
description: 使用 PowerShell 在一個資源上指派受控識別、存取另一個資源的逐步指示。
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ae2da130f61d31db4904ed2dd5ac18444929950
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89177494"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>使用 PowerShell 為受控識別指派對資源的存取權

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

以受控識別設定 Azure 資源後，您可以為此受控識別提供對另一項資源的存取權，就像任何安全性主體一樣。 此範例說明如何使用 PowerShell 為 Azure 虛擬機器的受控識別提供對 Azure 儲存體帳戶的存取權。

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先決條件

- 如果您不熟悉 Azure 資源的受控識別，請參閱[概觀一節](overview.md)。 **請務必檢閱[系統指派和使用者指派受控識別之間的差異](overview.md#managed-identity-types)**。
- 如果您還沒有 Azure 帳戶，請先[註冊免費帳戶](https://azure.microsoft.com/free/)，再繼續進行。
- 如果您尚未安裝[最新版的 Azure PowerShell](/powershell/azure/install-az-ps)，請先安裝。

## <a name="use-azure-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>使用 Azure RBAC 將受控識別存取權指派給另一個資源

您在 Azure 資源 ([例如 Azure VM](qs-configure-powershell-windows-vm.md)) 上啟用受控識別之後：

1. 請使用 `Connect-AzAccount` Cmdlet 登入 Azure。 使用與已設定受控識別的 Azure 訂用帳戶相關聯的帳戶：

   ```powershell
   Connect-AzAccount
   ```
2. 在此範例中，我們會將 Azure VM 存取權給予儲存體帳戶。 首先我們使用 [Get-AzVM](/powershell/module/az.compute/get-azvm) 取得 VM `myVM` 的服務主體，這是我們在啟用受控識別時所建立的。 然後，請使用 [New-AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment) 提供對儲存體帳戶 `myStorageAcct` 的 VM **讀者**存取權：

    ```powershell
    $spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>後續步驟

- [適用於 Azure 資源的受控識別概觀](overview.md)
- 若要在 Azure VM 上啟用受控識別，請參閱[使用 PowerShell 在 Azure VM 上設定 Azure 資源的受控識別](qs-configure-powershell-windows-vm.md)。

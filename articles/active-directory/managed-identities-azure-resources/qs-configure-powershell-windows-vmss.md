---
title: 使用 PowerShell 在虛擬機器擴展集上設定受控識別 - Azure AD
description: 使用 PowerShell 在虛擬機器擴展集上設定系統和使用者指派受控識別的逐步指示。
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2019
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70296dce5b9dcac738c17a4f2388a7eb37abd66f
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269330"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>使用 PowerShell 在虛擬機器擴展集上設定 Azure 資源的受控識別

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure 資源受控識別會在 Azure Active Directory 中為 Azure 服務提供自動受控識別。 您可以使用此身分識別來向任何支援 Azure AD 驗證的服務進行驗證，不需要任何您程式碼中的認證。 

在本文中，您將了解如何使用 PowerShell，在虛擬機器擴展集上執行 Azure 資源受控識別作業：
- 在虛擬機器擴展集上啟用和停用系統指派的受控識別
- 在虛擬機器擴展集上新增和移除使用者指派的受控識別

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先決條件

- 如果您不熟悉 Azure 資源的受控識別，請參閱[概觀一節](overview.md)。 **請務必檢閱[系統指派和使用者受控指派身分識別之間的差異](overview.md#managed-identity-types)**。
- 如果您還沒有 Azure 帳戶，請先[註冊免費帳戶](https://azure.microsoft.com/free/)，再繼續進行。
- 若要執行本文中的管理作業，您的帳戶需要下列 Azure 角色型存取控制指派：

    > [!NOTE]
    > 不需要其他 Azure AD 目錄角色指派。

    - [虛擬機器參與者](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)，可建立虛擬機器擴展集，並從虛擬機器擴展集啟用和移除系統指派受控和/或使用者指派的受控識別。
    - [受控識別參與者](../../role-based-access-control/built-in-roles.md#managed-identity-contributor)角色，可建立使用者指派的受控識別。
    - [受控識別操作員](../../role-based-access-control/built-in-roles.md#managed-identity-operator)角色，可為虛擬機器擴展集指派和移除使用者指派的受控識別。
- 如果您尚未安裝[最新版的 Azure PowerShell](/powershell/azure/install-az-ps)，請先安裝。 

## <a name="system-assigned-managed-identity"></a>系統指派的受控識別

在本節中，您會學習如何使用 Azure PowerShell 啟用和移除系統指派的受控識別。

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>在 Azure 虛擬機器擴展集建立期間啟用系統指派的受控識別

若要建立已啟用系統指派受控識別的虛擬機器擴展集：

1. 請參閱 [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) Cmdlet 參考一文中的「範例 1」，以建立具有系統指派受控識別的虛擬機器擴展集。  將參數 `-IdentityType SystemAssigned` 新增至 `New-AzVmssConfig` Cmdlet：

    ```powershell
    $VMSS = New-AzVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```



## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>在現有 Azure 虛擬機器擴展集上啟用系統指派的受控識別

如果您需要在現有 Azure 虛擬機器擴展集上啟用系統指派的受控識別：

1. 使用 `Connect-AzAccount` 登入 Azure。 使用與包含虛擬機器擴展集的 Azure 訂用帳戶相關聯的帳戶。 此外，也請確定您的帳戶屬於在虛擬機器擴展集上具有寫入權限的角色，例如「虛擬機器參與者」：

   ```powershell
   Connect-AzAccount
   ```

2. 首先，使用 [`Get-AzVmss`](/powershell/module/az.compute/get-azvmss) Cmdlet 來擷取虛擬機器擴展集屬性。 然後在 [Update-AzVmss](/powershell/module/az.compute/update-azvmss) Cmdlet 上使用 `-IdentityType` 參數來啟用系統指派的受控識別：

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```



### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>從 Azure 虛擬機器擴展集停用系統指派的受控識別

如果您的虛擬機器擴展集已不再需要系統指派的受控識別，但仍需要使用者指派的受控識別，請使用下列 Cmdlet：

1. 使用 `Connect-AzAccount` 登入 Azure。 使用與包含虛擬機器的 Azure 訂用帳戶相關聯的帳戶。 此外，也請確定您的帳戶屬於在虛擬機器擴展集上具有寫入權限的角色，例如「虛擬機器參與者」：

2. 執行下列 Cmdlet：

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

如果您的虛擬機器擴展集不再需要系統指派的受控識別，而且沒有使用者指派的受控識別，請使用下列命令：

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>使用者指派的受控識別

在本節中，您將了解如何使用 Azure PowerShell，從虛擬機器擴展集新增和移除使用者指派的受控識別。

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>在 Azure 虛擬機器擴展集建立期間指派使用者指派的受控識別

目前無法透過 PowerShell 支援建立具有使用者指派受控識別的新虛擬機器擴展集。 請參閱下一節，以了解如何將使用者指派的受控識別新增至現有的虛擬機器擴展集。 請隨時回來查看是否有更新內容。

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>將使用者指派的受控識別指派給現有的 Azure 虛擬機器擴展集

若要將使用者指派的受控識別指派給現有的 Azure 虛擬機器擴展集：

1. 使用 `Connect-AzAccount` 登入 Azure。 使用與包含虛擬機器擴展集的 Azure 訂用帳戶相關聯的帳戶。 此外，也請確定您的帳戶屬於在虛擬機器擴展集上具有寫入權限的角色，例如「虛擬機器參與者」：

   ```powershell
   Connect-AzAccount
   ```

2. 首先，使用 `Get-AzVM` Cmdlet 來擷取虛擬機器擴展集屬性。 然後使用 [Update-AzVmss](/powershell/module/az.compute/update-azvmss) Cmdlet 上的 `-IdentityType` 和 `-IdentityID` 參數，將使用者指派的受控識別新增至虛擬機器擴展集。 以您自己的值取代 `<VM NAME>`、`<SUBSCRIPTION ID>`、`<RESROURCE GROUP>`、`<USER ASSIGNED ID1>`、`USER ASSIGNED ID2`。

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>從 Azure 虛擬機器擴展集移除使用者指派的受控識別

如果您的虛擬機器擴展集具有多個使用者指派的受控識別，則您可以使用下列命令來移除所有識別，但請留下最後一個。 請務必以您自己的值取代 `<RESOURCE GROUP>` 和 `<VIRTUAL MACHINE SCALE SET NAME>` 參數的值。 `<USER ASSIGNED IDENTITY NAME>` 是使用者指派受控識別的名稱屬性，它應該保留在虛擬機器擴展集上。 您可以使用 `az vmss show`，在虛擬機器擴展集的識別區段中找到此資訊：

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
如果您的虛擬機器擴展集沒有系統指派的受控識別，而您想要從其中移除所有使用者指派的受控識別，請使用下列命令：

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
如果您的虛擬機器擴展集同時具有系統指派和使用者指派的受控識別，您可以藉由切換為僅使用系統指派的身分識別，來移除所有使用者指派的受控識別。

```powershell 
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>後續步驟

- [Azure 資源受控識別概觀](overview.md)
- 如需完整的 Azure VM 建立快速入門，請參閱：
  
  - [使用 PowerShell 建立 Windows 虛擬機器](../../virtual-machines/windows/quick-create-powershell.md) 
  - [使用 PowerShell 建立 Linux 虛擬機器](../../virtual-machines/linux/quick-create-powershell.md)
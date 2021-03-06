---
title: 管理群組的 PowerShell V2 範例-Azure AD |Microsoft Docs
description: 此頁面會提供 PowerShell 範例以協助您管理 Azure Active Directory 中的群組
keywords: Azure AD, Azure Active Directory, PowerShell, 群組, 群組管理
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c00c89e5f81bcb142c50e5f3438c1af2d72a9de5
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2020
ms.locfileid: "90056212"
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a>適用於群組管理的 Azure Active Directory 第 2 版 Cmdlet

> [!div class="op_single_selector"]
> - [Azure 入口網站](../fundamentals/active-directory-groups-create-azure-portal.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
> - [PowerShell](groups-settings-v2-cmdlets.md)
>
>

本文包含的範例，會說明如何使用 PowerShell 管理 Azure Active Directory (Azure AD) 中的群組。  其中也會說明如何使用 Azure AD PowerShell 模組來完成設定。 首先，您必須 [下載 Azure AD PowerShell 模組](https://www.powershellgallery.com/packages/AzureAD/)。

## <a name="install-the-azure-ad-powershell-module"></a>安裝 Azure AD PowerShell 模組

若要安裝 AzureAD PowerShell 模組，請使用下列命令︰

```powershell
    PS C:\Windows\system32> install-module azuread
    PS C:\Windows\system32> import-module azuread
```

若要確認此模組已可使用，請使用下列命令︰

```powershell
    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

現在您可以開始在模組中使用 Cmdlet。 如需有關 Azure AD 模組中各式 Cmdlet 的完整描述，請參閱 [Azure Active Directory PowerShell 第 2 版](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0)的線上參考文件。

> [!NOTE]
> Azure AD PowerShell Cmdlet 無法與新的 Powershell 7 搭配使用，因為它是以 .net Core 為基礎。 我們已經知道，這正在進行更新。 目前，我們建議使用 Windows PowerShell 5.x 模組來 Azure AD PowerShell 作業。 


## <a name="connect-to-the-directory"></a>連線至目錄

使用 Azure AD PowerShell Cmdlet 開始管理群組之前，您必須先將 PowerShell 工作階段連線至想要管理的目錄。 使用下列命令：

```powershell
    PS C:\Windows\system32> Connect-AzureAD
```

Cmdlet 會提示您輸入需要用來存取目錄的認證。 在此範例中，我們會使用 karen@drumkit.onmicrosoft.com 存取示範目錄。 Cmdlet 將會傳回確認，表示工作階段已成功連線到目錄︰

```powershell
    Account                       Environment Tenant ID
    -------                       ----------- ---------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…
```

現在您可以開始使用 AzureAD Cmdlet 來管理您目錄中的群組。

## <a name="retrieve-groups"></a>擷取群組

若要從目錄中擷取現有的群組，請使用 Get-AzureADGroups Cmdlet。 

若要擷取目錄中的所有群組，請在使用 Cmdlet 時不要使用參數：

```powershell
    PS C:\Windows\system32> get-azureadgroup
```

Cmdlet 將會傳回所連線目錄中的所有群組。

您可以使用 -objectID 參數來擷取您指定其群組 objectID 的特定群組：

```powershell
    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b
```

現在，Cmdlet 將傳回 objectID 與您所輸入之參數值相符的群組︰

```powershell
    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

您可以使用 -filter 參數搜尋特定群組。 此參數採用 ODATA 篩選子句，並傳回和篩選條件相符的所有群組，如下列範例所示︰

```powershell
    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

> [!NOTE]
> Azure AD PowerShell Cmdlet 實作 OData 查詢標準。 如需詳細資訊，請參閱[使用 OData 端點的 OData 系統查詢選項](/previous-versions/dynamicscrm-2015/developers-guide/gg309461(v=crm.7)#BKMK_filter)中的 **$filter**。

## <a name="create-groups"></a>建立群組

若要在目錄中建立新群組，請使用 New-AzureADGroup Cmdlet。 這個 Cmdlet 會建立名為 “Marketing" 的新安全性群組︰

```powershell
    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"
```

## <a name="update-groups"></a>更新群組

若要更新現有的群組，請使用 Set-AzureADGroup Cmdlet。 在這個範例中，我們要變更 “Intune Administrators” 群組的 DisplayName 屬性。 首先，我們使用 Get-AzureADGroup Cmdlet 找到該群組，然後使用 DisplayName 屬性進行篩選︰

```powershell
    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

接下來，我們要將 Description 屬性變更為新值 “Intune Device Administrators”︰

```powershell
    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"
```

現在，如果我們再次發現群組，就會看到 Description 屬性已更新，以反映新的值：

```powershell
    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"

    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True
```

## <a name="delete-groups"></a>刪除群組

若要從目錄中刪除群組，請使用 Remove-AzureADGroup Cmdlet，如下所示︰

```powershell
    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b
```

## <a name="manage-group-membership"></a>管理群組成員資格

### <a name="add-members"></a>新增成員

若要將新成員新增至群組，請使用 Add-AzureADGroupMember Cmdlet。 此命令會將成員新增至上述範例中所使用的 Intune Administrators 群組︰

```powershell
    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea
```

我們想要新增成員的群組，其 ObjectID 就是 -ObjectId 參數，而我們想要新增為群組成員的使用者，其 ObjectID 為 -RefObjectId。

### <a name="get-members"></a>取得成員

若要取得群組的現有成員，請使用 Get-AzureADGroupMember Cmdlet，如此範例所示︰

```powershell
    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User
```

### <a name="remove-members"></a>移除成員

若要移除先前加入群組的成員，請使用Remove-AzureADGroupMember Cmdlet，如此處所示︰

```powershell
    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea
```

### <a name="verify-members"></a>確認成員

若要驗證使用者的群組成員資格，請使用 Select-AzureADGroupIdsUserIsMemberOf Cmdlet。 這個 Cmdlet 會將要檢查群組成員資格的使用者，其 ObjectId 以及一份要檢查成員資格的群組清單做為參數。 務必要以 “Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck” 複雜變數類型的形式提供群組清單，因此我們必須先使用該類型建立一個變數：

```powershell
    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck
```

接下來，我們提供 groupIds 的值，以簽入此複雜變數的 "GroupIds" 屬性︰

```powershell
    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"
```

現在，如果我們想要檢查 ObjectID 為 72cd4bbd-2594-40a2-935c-016f3cfeeeea 的使用者，是否具有 $g 中任何群組的群組成員資格，我們應該使用︰

```powershell
    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}
```

傳回的值就是成員中有這位使用者的群組清單。 您也可以使用 Select-AzureADGroupIdsContactIsMemberOf、Select-AzureADGroupIdsGroupIsMemberOf 或 Select-AzureADGroupIdsServicePrincipalIsMemberOf，在檢查特定群組清單中的連絡人、群組或服務主體成員資格時，採用這個方法

## <a name="disable-group-creation-by-your-users"></a>停用您使用者的群組建立

您可以避免非管理使用者建立安全性群組。 Microsoft Online 目錄服務 (MSODS) 的預設行為是可讓非管理使用者建立群組，無論是否也啟用自助式群組管理 (SSGM)。 SSGM 設定只會控制 [我的應用程式] 存取面板中的行為。

若要停用非管理使用者的群組建立：

1. 請確認允許非管理使用者建立群組：
   
   ```powershell
   PS C:\> Get-MsolCompanyInformation | fl UsersPermissionToCreateGroupsEnabled
   ```
  
2. 如果它傳回 `UsersPermissionToCreateGroupsEnabled : True`，非管理使用者就可以建立群組。 若要停用這項功能：
  
   ```powershell 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```
  
## <a name="manage-owners-of-groups"></a>管理群組擁有者

若要將擁有者新增到群組，請使用 Add-AzureADGroupOwner Cmdlet︰

```powershell
    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea
```

-ObjectId 參數是我們要新增擁有者的群組 ObjectID，而-RefObjectId 是我們想要新增為群組擁有者的使用者或服務主體的 ObjectID。

若要擷取群組的擁有者，請使用 Get AzureADGroupOwner Cmdlet：

```powershell
    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
```

指令程式會針對指定的群組，傳回 (使用者和服務主體) 的擁有者清單：

```powershell
    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User
```

如果您需要從群組中移除擁有者，請使用 Remove-AzureADGroupOwner Cmdlet：

```powershell
    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67
```

## <a name="reserved-aliases"></a>保留的別名

當群組建立時，某些端點允許終端使用者指定 mailNickname 或別名，以作為群組電子郵件地址的一部分。以下的電子郵件別名具有高度權限，只有 Azure AD 全域管理員才能建立使用這些別名的群組。 
  
* abuse
* admin
* administrator
* hostmaster
* majordomo
* postmaster
* root
* secure
* security
* ssl-admin
* webmaster

## <a name="group-writeback-to-on-premises-preview"></a>將回寫群組至內部部署 (預覽版) 

目前，有許多群組仍在內部部署 Active Directory 中進行管理。 若要回答將雲端群組同步回內部部署的要求，Azure AD 的 Microsoft 365 群組回寫功能現在可供預覽。

在雲端中建立和管理 Microsoft 365 群組。 回寫功能可讓您將 Microsoft 365 群組寫回為已安裝 Exchange 的 Active Directory 樹系的通訊群組。 具有內部部署 Exchange 信箱的使用者可以從這些群組傳送和接收電子郵件。 群組回寫功能不支援 Azure AD 安全性群組或通訊群組。

如需詳細資訊，請參閱 [Azure AD Connect 同步服務](../hybrid/how-to-connect-syncservice-features.md)的檔。

Microsoft 365 群組回寫是 Azure Active Directory (Azure AD) 的公開預覽功能，而且適用于任何付費的 Azure AD 授權方案。 如需有關預覽的一些法律資訊，請參閱 [Microsoft Azure 預覽的補充使用條款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="next-steps"></a>接下來的步驟

您可以在 [Azure Active Directory Cmdlet](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0)中找到更多 Azure Active Directory PowerShell 文件。

* [使用 Azure Active Directory 群組管理資源的存取權](../fundamentals/active-directory-manage-groups.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
* [整合內部部署身分識別與 Azure Active Directory](../hybrid/whatis-hybrid-identity.md?context=azure/active-directory/users-groups-roles/context/ugr-context)
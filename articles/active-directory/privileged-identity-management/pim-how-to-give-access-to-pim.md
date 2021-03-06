---
title: 授與管理 PIM-Azure Active Directory 的存取權 |Microsoft Docs
description: 了解如何授與其他系統管理員存取權以管理 Azure AD Privileged Identity Management (PIM)。
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 08/06/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95f8991d5ba9efb8e3223dd44a8d037acf2de849
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009557"
---
# <a name="delegate-access-to-privileged-identity-management"></a>委派 Privileged Identity Management 的存取權

若要將存取權委派給 Privileged Identity Management (PIM) ，全域管理員可以將其他使用者指派給特殊許可權角色管理員角色。 根據預設，安全性系統管理員和安全性讀取者具有 Privileged Identity Management 的唯讀存取權。 若要授與 Privileged Identity Management 的存取權，第一位使用者可以將其他人指派給特殊**許可權角色管理員**角色。 只有特殊許可權角色管理員角色才需要管理 Azure AD 角色。 不需要特殊許可權角色管理員許可權來管理 Azure 資源的設定。

> [!NOTE]
> 管理 Privileged Identity Management 需要 Azure 多重要素驗證。 因為 Microsoft 帳戶無法註冊 Azure 多重要素驗證，所以使用 Microsoft 帳戶登入的使用者無法存取 Privileged Identity Management。

請確保特殊權限角色管理員角色中永遠至少有兩位使用者，以防一位使用者遭到鎖定或他們的帳戶遭刪除。

## <a name="delegate-access-to-manage-pim"></a>委派管理 PIM 的存取權

1. 登入 [Azure 入口網站](https://portal.azure.com/)。

1. 在 Azure AD 中，開啟**Privileged Identity Management**。

1. 選取 [ **Azure AD 角色**]。

1. 選取 [角色]****。

    ![Privileged Identity Management Azure AD 角色-角色](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. 選取 [特殊**許可權角色管理員**] 角色以開啟 [成員] 頁面。

    ![特殊權限角色管理員 - 成員](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. 選取 [**新增成員**] 以開啟 [**新增受控成員**] 窗格。

1. 選取 [**選取成員**] 以開啟 [**選取成員**] 窗格。

    ![特殊權限角色管理員 - 選取成員](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. 選取成員，然後按一下 [選取]****。

1. 選取 **[確定]** ，讓成員符合特殊**許可權角色管理員**角色的資格。

    當您將新角色指派給 Privileged Identity Management 中的人員時，他們會自動設定為符合啟用角色的**資格**。

1. 若要將成員設為永久，請在 [特殊許可權角色管理員] 成員清單中選取使用者。

1. 選取 [**更多**]，然後選擇 [**永久**]，讓指派成為永久。

    ![特殊權限角色管理員 - 永久符合資格](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. 將[開始使用 Privileged Identity Management](pim-getting-started.md)的連結傳送給使用者。

## <a name="remove-access-to-manage-pim"></a>移除管理 PIM 的存取權

將某人自特殊權限角色管理員角色移除之前，請確保該角色至少仍有兩位指派的使用者。

1. 登入 [Azure 入口網站](https://portal.azure.com/)。

1. 開啟**Azure AD Privileged Identity Management**。

1. 選取 [ **Azure AD 角色**]。

1. 選取 [角色]****。

1. 選取 [特殊**許可權角色管理員**] 角色以開啟 [成員] 頁面。

1. 選取您想要移除之使用者旁的核取方塊，然後選取 [**移除成員**]。

    ![特殊權限角色管理員 - 移除成員](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. 當系統要求您確認是否要從角色中移除成員時，請選取 **[是]**。

## <a name="next-steps"></a>後續步驟

- [開始使用 Privileged Identity Management](pim-getting-started.md)

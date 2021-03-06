---
title: 在 Azure AD 權利管理中變更存取套件的資源角色-Azure Active Directory
description: 瞭解如何在 Azure Active Directory 權利管理中變更現有存取套件的資源角色。
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b6e2ac9d80c1c3bf76b4a3d4c44f0654100670f
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89567413"
---
# <a name="change-resource-roles-for-an-access-package-in-azure-ad-entitlement-management"></a>在 Azure AD 權利管理中變更存取套件的資源角色

您可以使用存取套件管理員隨時變更存取套件中的資源，而不必擔心如何布建使用者對新資源的存取權，或移除先前資源的存取權。 本文說明如何變更現有存取套件的資源角色。

這段影片提供如何變更存取套件的總覽。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3LD4Z]

## <a name="check-catalog-for-resources"></a>查看資源的類別目錄

如果您需要將資源新增至存取套件，您應該檢查目錄中是否有您需要的資源。 如果您是存取套件管理員，則無法將資源新增至目錄，即使您擁有它們也是一樣。 您只能使用目錄中可用的資源。

**必要角色：** 全域管理員、使用者系統管理員、目錄擁有者或存取套件管理員

1. 在 Azure 入口網站中按一下 [Azure Active Directory]  ，然後按一下 [身分識別治理]  。

1. 在左側功能表中，按一下 [ **目錄** ]，然後開啟目錄。

1. 在左側功能表中，按一下 [ **資源** ]，以查看此目錄中的資源清單。

    ![目錄中的資源清單](./media/entitlement-management-access-package-resources/catalog-resources.png)

1. 如果您是存取套件管理員，而且需要將資源新增至目錄，您可以要求目錄擁有者將其新增。

## <a name="add-resource-roles"></a>新增資源角色

資源角色是與資源相關聯的許可權集合。 讓使用者可以要求資源的方式，是將資源角色新增至存取套件。 您可以新增群組、小組、應用程式和 SharePoint 網站的資源角色。

**必要角色：** 全域管理員、使用者系統管理員、目錄擁有者或存取套件管理員

1. 在 Azure 入口網站中按一下 [Azure Active Directory]  ，然後按一下 [身分識別治理]  。

1. 在左側功能表中，按一下 [ **存取套件** ]，然後開啟存取套件。

1. 在左側功能表中，按一下 [ **資源角色**]。

1. 按一下 [ **新增資源角色** ]，以開啟 [新增資源角色以存取套件] 頁面。

    ![存取套件-新增資源角色](./media/entitlement-management-access-package-resources/resource-roles-add.png)

1. 根據您是否要新增群組、小組、應用程式或 SharePoint 網站，請執行下列其中一個資源角色區段中的步驟。

## <a name="add-a-group-or-team-resource-role"></a>新增群組或小組資源角色

您可以讓權利管理在指派存取套件給 Microsoft 小組時，自動將使用者新增至群組或小組。 

- 當群組或小組是存取套件的一部分，且使用者已指派給該存取套件時，就會將使用者新增至該群組或小組（如果還沒有的話）。
- 當使用者的存取套件指派到期時，系統會將他們從群組或小組中移除，除非他們目前有指派給另一個包含相同群組或小組的存取套件。

您可以選取任何 [Azure AD 安全性群組或 Microsoft 365 群組](../fundamentals/active-directory-groups-create-azure-portal.md)。 系統管理員可以將任何群組新增至目錄;目錄擁有者可以將任何群組新增至目錄（如果它們是群組的擁有者）。 選取群組時，請記住下列 Azure AD 條件約束：

- 當使用者（包括來賓）新增為群組或小組的成員時，他們就可以看到該群組或小組的所有其他成員。
- Azure AD 無法使用 Azure AD Connect，或在 Exchange Online 中建立的 Active Directory 群組成員資格變更為通訊群組的成員資格。  
- 您無法藉由新增或移除成員來更新動態群組的成員資格，因此動態群組成員資格不適合搭配權利管理使用。

如需詳細資訊，請參閱 [比較群組](/office365/admin/create-groups/compare-groups) 和 [Microsoft 365 群組和 Microsoft 小組](/microsoftteams/office-365-groups)。

1. 在 [ **將資源角色新增至存取套件** ] 頁面上，按一下 [ **群組和小組** ] 以開啟 [選取群組] 窗格。

1. 選取您要包含在存取套件中的群組和小組。

    ![存取套件-新增資源角色-選取群組](./media/entitlement-management-access-package-resources/group-select.png)

1. 按一下 [選取]  。

    一旦您選取群組或小組，[ **子類型** ] 欄會列出下列其中一個子類型：

    | 子類型 | 描述 |
    | --- | --- |
    | 安全性 | 用來授與資源的存取權。 |
    | 散發 | 用來將通知傳送給一群人。 |
    | Microsoft 365 | 未啟用小組的 Microsoft 365 群組。 用於在公司內部和外部的使用者之間共同作業。 |
    | 小組 | 已啟用團隊的 Microsoft 365 群組。 用於在公司內部和外部的使用者之間共同作業。 |

1. 在 [ **角色** ] 清單中，選取 [ **擁有** 者] 或 [ **成員**]。

    您通常會選取成員角色。 如果您選取 [擁有者] 角色，可讓使用者新增或移除其他成員或擁有者。

    ![存取套件-為群組或小組新增資源角色](./media/entitlement-management-access-package-resources/group-role.png)

1. 按一下 [新增]  。

    具有存取套件之現有指派的任何使用者，在新增時都會自動成為此群組或小組的成員。

## <a name="add-an-application-resource-role"></a>新增應用程式資源角色

您可以讓 Azure AD 自動將 Azure AD 企業應用程式的存取權指派給使用者，包括當使用者獲指派存取套件時，與 Azure AD 同盟的 SaaS 應用程式和組織的應用程式。 針對透過同盟單一登入與 Azure AD 整合的應用程式，Azure AD 會為指派給應用程式的使用者發出同盟權杖。

應用程式可以有多個角色。 將應用程式新增至存取套件時，如果該應用程式有一個以上的角色，則您必須為這些使用者指定適當的角色。 如果您正在開發應用程式，您可以在 [如何：為企業應用程式設定 SAML 權杖中發出的角色](../develop/active-directory-enterprise-app-role-management.md)宣告中，深入瞭解如何將這些角色新增至您的應用程式。

一旦應用程式角色是存取套件的一部分：

- 當使用者獲指派該存取套件時，就會將使用者新增至該應用程式角色（如果還沒有的話）。
- 當使用者的存取套件指派到期時，將會從應用程式中移除其存取權，除非他們有指派給另一個包含該應用程式角色的存取套件。

以下是選取應用程式時的一些考慮：

- 應用程式也可能會將群組指派給他們的角色。  您可以選擇新增群組來取代存取套件中的應用程式角色，不過，在我的存取權入口網站的存取套件中，使用者將看不到該應用程式。

1. 在 [ **將資源角色新增至存取套件** ] 頁面上，按一下 [ **應用程式** ] 以開啟 [選取應用程式] 窗格。

1. 選取您要包含在存取套件中的應用程式。

    ![存取套件-新增資源角色-選取應用程式](./media/entitlement-management-access-package-resources/application-select.png)

1. 按一下 [選取]  。

1. 在 [ **角色** ] 清單中，選取應用程式角色。

    ![存取套件-為應用程式新增資源角色](./media/entitlement-management-access-package-resources/application-role.png)

1. 按一下 [新增]  。

    在新增應用程式時，任何具有存取套件之現有指派的使用者都會自動獲得存取權。

## <a name="add-a-sharepoint-site-resource-role"></a>新增 SharePoint 網站資源角色

當指派存取套件給使用者時，Azure AD 可以自動將 SharePoint Online 網站或 SharePoint Online 網站集合的存取權指派給使用者。

1. 在 [ **將資源角色加入至存取套件** ] 頁面上，按一下 [ **sharepoint 網站** ] 以開啟 [選取 sharepoint Online 網站] 窗格。

1. 選取您要包含在存取套件中的 SharePoint Online 網站。

    ![存取套件-新增資源角色-選取 SharePoint Online 網站](./media/entitlement-management-access-package-resources/sharepoint-site-select.png)

1. 按一下 [選取]  。

1. 在 [ **角色** ] 清單中，選取 SharePoint Online 網站角色。

    ![存取套件-SharePoint Online 網站的新增資源角色](./media/entitlement-management-access-package-resources/sharepoint-site-role.png)

1. 按一下 [新增]  。

    具有存取套件之現有指派的任何使用者，將會在新增時，自動獲得此 SharePoint Online 網站的存取權。

## <a name="remove-resource-roles"></a>移除資源角色

**必要角色：** 全域管理員、使用者系統管理員、目錄擁有者或存取套件管理員

1. 在 Azure 入口網站中按一下 [Azure Active Directory]  ，然後按一下 [身分識別治理]  。

1. 在左側功能表中，按一下 [ **存取套件** ]，然後開啟存取套件。

1. 在左側功能表中，按一下 [ **資源角色**]。

1. 在資源角色清單中，找出您想要移除的資源角色。

1. 按一下省略號 (**...**) 然後按一下 [ **移除資源角色**]。

    任何具有存取套件之現有指派的使用者，都會在移除時，自動將其存取權撤銷至此資源角色。

## <a name="when-changes-are-applied"></a>套用變更時

在 [權利管理] 中，Azure AD 會一天處理存取套件中的指派和資源大量變更。 因此，如果您進行指派，或變更存取套件的資源角色，最多可能需要24小時才會在 Azure AD 中進行這項變更，以及將這些變更傳播到其他 Microsoft 線上服務或已連線的 SaaS 應用程式所需的時間量。 如果您的變更只會影響一些物件，則變更可能只需要幾分鐘的時間就會在 Azure AD 中套用，之後其他 Azure AD 元件就會偵測到變更並更新 SaaS 應用程式。 如果您的變更會影響數以千計的物件，變更將會花費較長的時間。 例如，如果您有一個具有2個應用程式和100個使用者指派的存取套件，且您決定將 SharePoint 網站角色加入至存取套件，則必須等到所有使用者都屬於該 SharePoint 網站角色的一部分時，才會有延遲。 您可以透過 Azure AD 審核記錄、Azure AD 布建記錄和 SharePoint 網站審核記錄來監視進度。

當您移除小組成員時，也會從 Microsoft 365 群組移除這些成員。 移除小組的交談功能方面可能會延遲。 如需詳細資訊，請參閱 [群組成員資格](/microsoftteams/office-365-groups#group-membership)。

## <a name="next-steps"></a>接下來的步驟

- [使用 Azure Active Directory 建立基本群組並新增成員](../fundamentals/active-directory-groups-create-azure-portal.md)
- [操作說明：針對企業應用程式，設定 SAML 權杖中發出的角色宣告](../develop/active-directory-enterprise-app-role-management.md)
- [SharePoint Online 簡介](/sharepoint/introduction)
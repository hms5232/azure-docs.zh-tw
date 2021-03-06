---
title: 教學課程：Azure Active Directory 與 Keeper Password Manager & Digital Vault 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Keeper Password Manager & Digital Vault 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/07/2020
ms.author: jeedes
ms.openlocfilehash: c6bd0c130e860a5700256a54c081cc046219b41a
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88546741"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>教學課程：Azure Active Directory 與 Keeper Password Manager & Digital Vault 整合

在本教學課程中，您將了解如何整合 Keeper Password Manager & Digital Vault 與 Azure Active Directory (Azure AD)。
Keeper Password Manager & Digital Vault 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Keeper Password Manager & Digital Vault 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 Keeper Password Manager & Digital Vault (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>Prerequisites

若要設定與 Keeper Password Manager & Digital Vault 的 Azure AD 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Keeper Password Manager & Digital Vault 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Keeper Password Manager & Digital Vault 支援由 **SP** 起始的 SSO

* Keeper Password Manager & Digital Vault 支援 **Just In Time** 使用者佈建

* 設定 ShipHazmatKeeper Password Manager & Digital Vault 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>從資源庫新增 Keeper Password Manager & Digital Vault

若要設定 Keeper Password Manager & Digital Vault 與 Azure AD 整合，您需要從資源庫將 Keeper Password Manager &amp;amp; Digital Vault 新增到受控 SaaS app 清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory] 服務。
1. 巡覽至 [企業應用程式]，然後選取 [所有應用程式]。
1. 若要新增應用程式，請選取 [新增應用程式]。
1. 在 [從資源庫新增] 區段的搜尋方塊中輸入 **Keeper Password Manager & Digital Vault**。
1. 從結果面板中選取 [Keeper Password Manager & Digital Vault]，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-sso-for-keeper-password-manager--digital-vault"></a>設定和測試適用於 Keeper Password Manager & Digital Vault 的 Azure AD SSO

使用名為 **B.Simon** 的測試使用者，設定及測試與 Keeper Password Manager & Digital Vault 搭配運作的 Azure AD SSO。 若要讓 SSO 運作，您必須在 Azure AD 使用者和 Keeper Password Manager & Digital Vault 中的相關使用者之間建立連結關聯性。

若要設定及測試搭配 Keeper Password Manager & Digital Vault 的 Azure AD SSO，需要完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。

    * **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
    * **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。

1. **[設定 Keeper Password Manager & Digital Vault SSO](#configure-keeper-password-manager--digital-vault-sso)** - 在應用程式端設定單一登入設定。
    * **[建立 Keeper Password Manager & Digital Vault 測試使用者](#create-keeper-password-manager--digital-vault-test-user)** - 使 Keeper Password Manager & Digital Vault 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

### <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Keeper Password Manager & Digital Vault] 應用程式整合頁面上，尋找**管理**區段並選取 [單一登入]。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入] 頁面上，按一下 [基本 SAML 設定] 的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    a. 在 [登入 URL] 文字方塊中，使用下列模式輸入 URL：
    * 針對 **Cloud SSO**：`https://keepersecurity.com/api/rest/sso/saml/sso/<CLOUD_INSTANCE_ID>`
    * 針對**內部部署 SSO**：`https://<KEEPER_FQDN>/sso-connect/saml/login`

    b. 在 [識別碼 (實體識別碼)] 文字方塊中，使用下列模式輸入 URL：
    * 針對 **Cloud SSO**：`https://keepersecurity.com/api/rest/sso/saml/<CLOUD_INSTANCE_ID>`
    * 針對**內部部署 SSO**：`https://<KEEPER_FQDN>/sso-connect`

    c. 在 **[回覆 URL]** 文字方塊中，以下列模式輸入 URL：
    * 針對 **Cloud SSO**：`https://keepersecurity.com/api/rest/sso/saml/sso/<CLOUD_INSTANCE_ID>`
    * 針對**內部部署 SSO**：`https://<KEEPER_FQDN>/sso-connect/saml/sso`

    > [!NOTE]
    > 這些都不是真正的值。 請使用實際的「登入 URL」、「識別碼」和「回覆 URL」來更新這些值。 請連絡 [Keeper Password Manager & Digital Vault 支援小組](https://keepersecurity.com/contact.html)取得這些值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

5. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中按一下 [下載]  ，以依據您的需求從指定選項下載**同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

6. 在 [設定 Keeper Password Manager & Digital Vault]**** 區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者 

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]。
1. 在 [使用者] 屬性中，執行下列步驟：
   1. 在 [名稱] 欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱] 欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。
   1. 按一下 [建立]。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會授與 Keeper Password Manager & Digital Vault 的存取權，讓 B.Simon 能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]，然後選取 [所有應用程式]。
1. 在應用程式清單中，選取 **Keeper Password Manager & Digital Vault**。
1. 在應用程式的概觀頁面中尋找 [管理] 區段，然後選取 [使用者和群組]。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]，然後在 [新增指派] 對話方塊中選取 [使用者和群組]。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組] 對話方塊的 [使用者] 清單中選取 [B.Simon]，然後按一下畫面底部的 [選取] 按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色] 對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取] 按鈕。
1. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。


## <a name="configure-keeper-password-manager--digital-vault-sso"></a>設定 Keeper Password Manager & Digital Vault SSO

若要設定單一登入 [Keeper Password Manager & Digital Vault 設定] 端，請依照 [Keeper 支援指南](https://docs.keeper.io/sso-connect-guide/) (英文) 的指導方針操作。

### <a name="create-keeper-password-manager--digital-vault-test-user"></a>建立 Keeper Password Manager & Digital Vault 測試使用者

若要讓 Azure AD 使用者可以登入 Keeper Password Manager & Digital Vault，則必須將使用者佈建到 Keeper Password Manager & Digital Vault。 應用程式僅在使用者佈建時支援，驗證之後，會在應用程式中自動建立使用者。 如果您要手動設定使用者，請連絡 [Keeper 支援](https://keepersecurity.com/contact.html)。

## <a name="test-sso"></a>測試 SSO

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Keeper Password Manager & Digital Vault] 圖格時，應該會自動登入您已設定 SSO 的 Keeper Password Manager & Digital Vault。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 Keeper Password Manager & Digital Vault](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

---
title: 教學課程：Azure Active Directory 與 Chargebee 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Chargebee 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/08/2019
ms.author: jeedes
ms.openlocfilehash: 6358c41ab6d63baa8ed1fa6960d7d887825a1d78
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88529470"
---
# <a name="tutorial-integrate-chargebee-with-azure-active-directory"></a>教學課程：整合 Chargebee 與 Azure Active Directory

在本教學課程中，您將了解如何整合 Chargebee 與 Azure Active Directory (Azure AD)。 在整合 Chargebee 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Chargebee 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Chargebee。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Chargebee 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Chargebee 支援由 **SP 和 IDP** 起始的 SSO

## <a name="adding-chargebee-from-the-gallery"></a>從資源庫新增 Chargebee

若要進行將 Chargebee 整合到 Azure AD 中的設定，您必須從資源庫將 Chargebee 新增至受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中，輸入 **Chargebee**。
1. 從結果面板中選取 [Chargebee]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-chargebee"></a>設定及測試 Chargebee 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Chargebee 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Chargebee 中相關使用者之間的連結關聯性。

若要設定及測試與 Chargebee 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
2. **[設定 Chargebee SSO](#configure-chargebee-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 Chargebee 測試使用者](#create-chargebee-test-user)** - 使 Chargebee 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
3. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Chargebee]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [選取單一登入方法]  頁面上，選取 [SAML]  。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 設定]  區段上，如果您想要以 **IDP** 起始模式設定應用程式，請輸入下列欄位的值：

    a. 在 [識別碼]  文字方塊中，使用下列模式來輸入 URL：`https://<domainname>.chargebee.com`

    b. 在 [回覆 URL]  文字方塊中，使用下列模式來輸入 URL：`https://app.chargebee.com/saml/<domainname>/acs`

1. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，以下列模式輸入 URL︰`https://<domainname>.chargebee.com`

    > [!NOTE]
    > `<domainname>` 是使用者在宣告帳戶後所建立的網域名稱。 如需其他資訊，請連絡 [Chargebee 用戶端支援小組](mailto:support@chargebee.com)。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

4. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

6. 在 [設定 Chargebee]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Chargebee 的存取權授與 B.Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Chargebee]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-chargebee-sso"></a>設定 Chargebee SSO

1. 開啟新的網頁瀏覽器視窗，並以系統管理員身分登入您的 Chargebee 公司網站。

4. 從功能表左側，按一下 [設定]   > [安全性]   > [管理]  。

    ![Chargebee 組態](./media/chargebee-tutorial/config01.png)

5. 在 [單一登入]  快顯視窗上，執行下列步驟：

    ![Chargebee 組態](./media/chargebee-tutorial/config02.png)

    a. 選取 [SAML]  。

    b. 在 [登入 URL]  文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL]  值。

    c. 在記事本中開啟 Base64 編碼的憑證，並複製其內容，然後貼到 [SAML 憑證]  文字方塊中。

    d. 按一下 [確認]  。

### <a name="create-chargebee-test-user"></a>建立 Chargebee 測試使用者

若要讓 Azure AD 使用者能夠登入 Chargebee，必須將這些使用者佈建到 Chargebee 中。 在 Chargebee 中，佈建是手動工作。

**若要佈建使用者帳戶，請執行下列步驟：**

1. 在不同的網頁瀏覽器視窗中，以安全性系統管理員身分登入 Chargebee。

2. 在功能表左側按一下 [客戶]  ，然後瀏覽至 [建立新的客戶]  。

    ![Freedcamp 組態](./media/chargebee-tutorial/config03.png)

3. 在 [新增客戶]  頁面上填入顯示如下的各個欄位，然後按一下 [建立客戶]  以建立使用者。

    ![Freedcamp 組態](./media/chargebee-tutorial/config04.png)

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Chargebee] 圖格時，應該會自動登入您已設定 SSO 的 Chargebee。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


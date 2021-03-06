---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 FortiGate SSL VPN 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 FortiGate SSL VPN 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 18a3d9d5-d81c-478c-be7e-ef38b574cb88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecb53d661b1171f9c1b18d37d0bb35952645ba7e
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299655"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-fortigate-ssl-vpn"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 FortiGate SSL VPN 整合

在本教學課程中，您會了解如何將 FortiGate SSL VPN 與 Azure Active Directory (Azure AD) 進行整合。 在整合 FortiGate SSL VPN 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 FortiGate SSL VPN 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 FortiGate SSL VPN。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>必要條件

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 FortiGate SSL VPN 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* FortiGate SSL VPN 支援由 **SP** 起始的 SSO
* 設定 FortiGate SSL VPN 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

## <a name="adding-fortigate-ssl-vpn-from-the-gallery"></a>從資源庫新增 FortiGate SSL VPN

若要進行將 FortiGate SSL VPN 整合到 Azure AD 中的設定，您必須從資源庫將 FortiGate SSL VPN 新增至受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory] 服務。
1. 巡覽至 [企業應用程式]，然後選取 [所有應用程式]。
1. 若要新增應用程式，請選取 [新增應用程式]。
1. 在 [從資源庫新增] 區段的搜尋方塊中輸入 **FortiGate SSL VPN**。
1. 從結果面板選取 [FortiGate SSL VPN]，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-sso-for-fortigate-ssl-vpn"></a>設定和測試 FortiGate SSL VPN 的 Azure AD SSO

以名為 **B.Simon** 的測試使用者，設定及測試與 FortiGate SSL VPN 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 FortiGate SSL VPN 中相關使用者之間的連結關聯性。

若要設定及測試與 FortiGate SSL VPN 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 FortiGate SSL VPN SSO](#configure-fortigate-ssl-vpn-sso)** - 在應用程式端設定單一登入設定。
    1. **建立 FortiGate SSL VPN 測試使用者** - 使 FortiGate SSL VPN 中具有與 Azure AD 中的 B.Simon 連結的相對應使用者。
1. **[測試 SSO](#test-single-sign-on)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [FortiGate SSL VPN] 應用程式整合頁面上，尋找 [管理] 區段並選取 [單一登入]。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入] 頁面上，按一下 [基本 SAML 設定] 的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [以 SAML 設定單一登入] 頁面上，輸入下列欄位的值：

    a. 在 [登入 URL] 文字方塊中，使用下列模式輸入 URL：`https://<FQDN>/remote/login`

    b. 在 [識別碼] 文字方塊中，使用下列模式來輸入 URL：`https://<FQDN>/remote/saml/metadata`

    c. 在 [回覆 URL] 文字方塊中，使用下列模式來輸入 URL：`https://><FQDN/remote/saml/login`

    d. 在 [登出 URL] 文字方塊中，以下列模式輸入 URL：`https://<FQDN>/remote/saml/logout`

    > [!NOTE]
    > 這些都不是真正的值。 使用實際的登入 URL、識別碼、回覆 URL 和登出 URL 來更新這些值。 請連絡 [FortiGate SSL VPN 用戶端支援小組](mailto:tac_amer@fortinet.com)以取得這些值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

1. FortiGate SSL VPN 應用程式需要特定格式的 SAML 判斷提示，而您需要將自訂屬性對應新增到 SAML 權杖屬性組態。 以下螢幕擷取畫面顯示預設屬性清單。

    ![image](common/default-attributes.png)

1. 除了上述屬性外，FortiGate SSL VPN 應用程式還需要在 SAML 回應中多傳回幾個屬性，如下所示。 這些屬性也會預先填入，但您可以根據您的需求來檢閱這些屬性。
    
    | 名稱 |  來源屬性|
    | ------------ | --------- |
    | username | user.userprincipalname |
    | 群組 | user.groups |

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 FortiGate SSL VPN] 區段上，根據您的需求複製適當的 URL。

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

在本節中，您會將 FortiGate SSL VPN 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]，然後選取 [所有應用程式]。
1. 在應用程式清單中，選取 [FortiGate SSL VPN]。
1. 在應用程式的概觀頁面中尋找 [管理] 區段，然後選取 [使用者和群組]。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]，然後在 [新增指派] 對話方塊中選取 [使用者和群組]。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組] 對話方塊的 [使用者] 清單中選取 [B.Simon]，然後按一下畫面底部的 [選取] 按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色] 對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取] 按鈕。
1. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-a-security-group-for-the-test-user"></a>建立測試使用者的安全性群組

在本節中，您會在測試使用者的 Azure Active Directory 中建立安全性群組。 FortiGate 會使用此安全性群組透過 VPN 授與使用者的網路存取權。

1. 在 Azure 入口網站的左側窗格中，選取 [Azure Active Directory]，再選取 [群組]。
1. 在畫面頂端選取 [新增群組]。
1. 在 [新增群組] 屬性中，遵循這些步驟：
   1. 在 [群組類型] 欄位中，選取[安全性]。
   1. 在 [名稱] 欄位中，輸入 `FortiGateAccess`。
   1. 在**群組描述**欄位中，輸入 `Group for granting FortiGate VPN access`。
   1. 針對**系統可以將 Azure AD 角色指派到群組 (預覽)** 設定，選取 [否]。
   1. 在**成員資格類型**欄位下，選取 [已指派]。
   1. 在**成員**下，選取 [未選取任何成員]。
   1. 在 [使用者和群組] 對話方塊的 [使用者] 清單中選取 [B.Simon]，然後按一下畫面底部的 [選取] 按鈕。
   1. 選取 [建立]  。
1. 回到 Azure Active Directory 中的**群組**刀鋒視窗後，找出 **FortiGate 存取**群組並記下**物件識別碼**以供稍後使用。

## <a name="configure-fortigate-ssl-vpn-sso"></a>設定 FortiGate SSL VPN SSO

### <a name="upload-the-base64-saml-certificate-to-the-fortigate-appliance"></a>將 Base64 SAML 憑證上傳至 FortiGate 設備

在您的租用戶中完成 FortiGate 應用程式的 SAML 設定後，您已下載 Base64 編碼的 SAML 憑證。 必須將此憑證上傳至 FortiGate 設備：

1. 登入 FortiGate 設備的管理入口網站。
1. 在左側功能表中，按一下 [系統]。
1. 在**系統**下，按一下 [憑證]。
1. 按一下 匯入 -> 遠端憑證。
1. 瀏覽至 Azure 租用戶中從 FortiGate 應用程式部署下載的憑證並加以選取，然後按一下 [確定]

上傳憑證後，記下其在**系統** > **憑證** > **遠端憑證**下的名稱。 根據預設，系統會將憑證命名為 REMOTE_Cert_**N**，其中 **N** 是整數值。

### <a name="perform-fortigate-command-line-configuration"></a>執行 FortiGate 命令列組態

下列步驟需要設定 Azure 登出 URL。 此 URL 包含問號(?)。 必須透過特殊步驟，才能成功提交此字元。 這些步驟無法從 FortiGate CLI 主控台執行。 相反地，請使用 PuTTY 之類的工具，建立 FortiGate 設備的 SSH 工作階段。 如果您的 FortiGate 設備是 Azure 虛擬機器，可以從 Azure 虛擬機器序列主控台執行下列步驟。

若要執行這些步驟，您需要先前記錄的值：

- 實體識別碼
- 回覆 URL
- 登出 URL
- Azure 登入 URL
- Azure AD 識別碼
- Azure 登出 URL
- Base64 SAML 憑證名稱 (REMOTE_Cert_N)

1. 建立 FortiGate 設備的 SSH 工作階段，並使用 FortiGate 系統管理員帳戶登入。
1. 執行下列命令：

   ```console
    config user saml
    edit azure
    set entity-id <Entity ID>
    set single-sign-on-url <Reply URL>
    set single-logout-url <Logout URL>
    set idp-single-sign-on-url <Azure Login URL>
    set idp-entity-id <Azure AD Identifier>
    set idp-single-logout-url <Azure Logout URL>
    set idp-cert <Base64 SAML Certificate Name>
    set user-name username
    set group-name group
    end

   ```

   > [!NOTE]
   > **Azure 登出 URL** 包含 `?` 字元。 您必須輸入特殊的金鑰序列，才能正確提供 FortiGate 序列主控台的 URL。 URL 通常是 `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`。
   >
   > 若要在序列主控台中輸入 Azure 登出 URL，請輸入 `set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation`。
   > 
   > 然後選取 CTRL+V 並貼入 URL 的其餘部分，以完成該行命令：`set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`。

### <a name="configure-fortigate-for-group-matching"></a>設定 FortiGate 以進行群組比對

在本節中，您會將 FortiGate 設定為辨識測試使用者所在之安全性群組的物件識別碼。 這可讓 FortiGate 根據此群組成員資格判斷是否要提供存取權。

若要執行這些步驟，您需要稍早建立的 **FortiGateAccess** 安全性群組物件識別碼

1. 建立 FortiGate 設備的 SSH 工作階段，並使用 FortiGate 系統管理員帳戶登入。
1. 執行下列命令：

   ```
    config user group
    edit FortiGateAccess
    set member azure
    config match
    edit 1
    set server-name azure
    set group-name <Object Id>
    next
    end
    next
    end
   ```

### <a name="create-fortigate-vpn-portals-and-firewall-policy"></a>建立 FortiGate VPN 入口網站和防火牆原則

在本節中，您會設定 FortiGate VPN 入口網站和防火牆原則，以授與先前建立之 **FortiGateAccess** 安全性群組的存取權。

請與 [FortiGate 支援小組](mailto:tac_amer@fortinet.com)合作，將 VPN 入口網站和防火牆原則新增至 FortiGate VPN 平台。 您必須先完成這些步驟，才能使用單一登入。

## <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [FortiGate SSL VPN] 圖格時，應該會自動登入您已設定 SSO 的 FortiGate SSL VPN。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

Microsoft 和 FortiGate 建議您使用 Fortinet VPN 用戶端 FortiClient，以獲得最佳的使用者體驗。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 FortiGate SSL VPN](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

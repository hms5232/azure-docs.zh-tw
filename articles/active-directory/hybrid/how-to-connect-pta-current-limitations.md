---
title: Azure AD Connect：傳遞驗證 - 目前的限制 | Microsoft Docs
description: 本文說明 Azure Active Directory (Azure AD) 傳遞驗證目前的限制
services: active-directory
keywords: Azure AD Connect 傳遞驗證, 安裝 Active Directory, Azure AD 的必要元件, SSO, 單一登入
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/04/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e9c4489f59f72e4d0b5c7a0b911da188eb0828c
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/01/2020
ms.locfileid: "89280191"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory 傳遞驗證：目前的限制

>[!IMPORTANT]
>Azure Active Directory (Azure AD) 傳遞驗證是免費功能，您不需要擁有任何 Azure AD 付費版本即可使用此功能。 傳遞驗證僅適用於 Azure AD 的全球執行個體，不適用於 [Microsoft Azure 德國雲端](https://www.microsoft.de/cloud-deutschland)或 [Microsoft Azure Government 雲端](https://azure.microsoft.com/features/gov/)。

## <a name="supported-scenarios"></a>支援的案例

以下是支援的案例：

- 使用者登入網頁瀏覽器型應用程式。
- 使用者使用傳統通訊協定 (例如 Exchange ActiveSync、EAS、SMTP、POP 及 IMAP) 來登入 Outlook 用戶端。
- 使用者登入支援 [新式驗證](https://www.microsoft.com/en-us/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview)的舊版 Office 用戶端應用程式和 office 應用程式： Office 2013 和2016版本。
- 使用者登入舊版通訊協定應用程式，例如 PowerShell 1.0 版和其他應用程式。
- 適用於 Windows 10 裝置的 Azure AD 聯結。
- 用於多重要素驗證的應用程式密碼。

## <a name="unsupported-scenarios"></a>不支援的案例

下列案例不受支援__︰

- 偵測[認證外洩](../identity-protection/overview-identity-protection.md)的使用者。
- Azure AD 網域服務必須在租用戶上啟用 [密碼雜湊同步處理]。 因此，使用傳遞驗證的租用戶「只有」__ 在需要 Azure AD 網域服務的情況下無法運作。
- 傳遞驗證未與 [Azure AD Connect Health](./whatis-azure-ad-connect.md) 整合。

> [!IMPORTANT]
> 「僅」__ 針對不支援的案例 (Azure AD Connect Health 整合除外)，在 Azure AD Connect 精靈的[選用功能](how-to-connect-install-custom.md#optional-features) 頁面上，啟用密碼雜湊同步處理作為因應措施。
> 
> [!NOTE]
> 啟用密碼雜湊同步處理可讓您選擇在內部部署基礎結構中斷時，將驗證容錯移轉。 這個從傳遞驗證到密碼雜湊同步處理的容錯移轉不會自動進行。 您必須使用 Azure AD Connect，以手動方式切換登入方法。 如果執行 Azure AD Connect 的伺服器關閉，您將需要 Microsoft 支援服務的協助，以關閉傳遞驗證。

## <a name="next-steps"></a>接下來的步驟
- [快速入門](how-to-connect-pta-quick-start.md)：開始使用 Azure AD 傳遞驗證。
- [從 AD FS 遷移到傳遞驗證](https://aka.ms/ADFSTOPTADPDownload) \(英文\) - 從 AD FS (或其他同盟技術) 遷移到傳遞驗證的詳細指南。
- [智慧鎖定](../authentication/howto-password-smart-lockout.md)：了解如何在租用戶中設定智慧鎖定功能以保護使用者帳戶。
- [技術深入探討](how-to-connect-pta-how-it-works.md)：了解傳遞驗證功能的運作方式。
- [常見問答集](how-to-connect-pta-faq.md)：尋找傳遞驗證功能常見問題的解答。
- [疑難排解](tshoot-connect-pass-through-authentication.md)：了解如何解決傳遞驗證功能的常見問題。
- [安全性深入探討](how-to-connect-pta-security-deep-dive.md)：取得傳遞驗證功能上的深入技術資訊。
- [Azure AD 無縫 SSO](how-to-connect-sso.md)：深入了解此互補功能。
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect)：使用 Azure Active Directory 論壇提出新功能要求。

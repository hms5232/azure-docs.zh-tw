---
title: 診斷 Azure AD 連線服務的錯誤 (Visual Studio)
description: Active directory 連線服務偵測到不相容的驗證類型
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: how-to
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.openlocfilehash: 7b437e3117540719c8c0adc5701ac1a5e934340b
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2020
ms.locfileid: "88114467"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>使用 Azure Active Directory 連線服務診斷錯誤

偵測先前的驗證碼時，Azure Active Directory 連線服務偵測到不相容的驗證類型。

必須重新建置專案才能正確偵測該專案中先前的驗證碼。 如果遇到這個錯誤，且專案中沒有先前的驗證碼，請重建並再試一次。

## <a name="project-types"></a>專案類型

連線服務會檢查您正在開發的專案類型，使其可將正確的驗證邏輯插入專案中。 如果專案中有任何衍生自 `ApiController` 的控制器，則該專案會視為 WebAPI 專案。 如果專案中只有衍生自 `MVC.Controller` 的控制器，則該專案會被視為 MVC 專案。 連線服務不支援任何其他專案類型。

## <a name="compatible-authentication-code"></a>相容的驗證碼

連線服務也會檢查先前以此服務設定，或與此服務相容的驗證設定。 如果所有設定皆存在，則會將其視為可重新進入的案例，而連線服務會開啟並顯示設定。  如果只有部分設定存在，則會將其視為錯誤案例。

在 MVC 專案中，連線服務會檢查先前使用此服務產生的以下任何設定：

```xml
<add key="ida:ClientId" value="" />
<add key="ida:Tenant" value="" />
<add key="ida:AADInstance" value="" />
<add key="ida:PostLogoutRedirectUri" value="" />
```

此外，連線服務會在 Web API 專案中檢查先前使用此服務產生的下列任何設定：

```xml
<add key="ida:ClientId" value="" />
<add key="ida:Tenant" value="" />
<add key="ida:Audience" value="" />
```

## <a name="incompatible-authentication-code"></a>不相容的驗證碼

最後，連線服務會嘗試偵測以舊版 Visual Studio 設定的驗證碼版本。 如果收到此錯誤，表示您的專案包含不相容的驗證類型。 連線服務會從舊版 Visual Studio 中偵測下列驗證類型：

* Windows 驗證
* 個別使用者帳戶
* 組織帳戶

為偵測 MVC 專案中的「Windows 驗證」，連線服務會在您的 `web.config` 檔案中尋找 `authentication` 元素。

```xml
<configuration>
    <system.web>
        <authentication mode="Windows" />
    </system.web>
</configuration>
```

為偵測 MVC 專案中的「Windows 驗證」，連線服務會在您的 `.csproj` 檔案中尋找 `IISExpressWindowsAuthentication` 元素：

```xml
<Project>
    <PropertyGroup>
        <IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication>
    </PropertyGroup>
</Project>
```

為偵測「個別使用者帳戶」驗證，連線服務會在您的 `packages.config` 檔案中尋找 package 元素。

```xml
<packages>
    <package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" />
</packages>
```

為偵測舊式「組織帳戶」驗證，連線服務會在 `web.config`中尋找下列元素：

```xml
<configuration>
    <appSettings>
        <add key="ida:Realm" value="***" />
    </appSettings>
</configuration>
```

若要變更驗證類型，請移除不相容的驗證類型，然後嘗試重新新增連線服務。

如需詳細資訊，請參閱 [Azure AD 的驗證案例](./authentication-vs-authorization.md)。
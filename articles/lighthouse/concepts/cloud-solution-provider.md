---
title: 雲端解決方案提供者方案考量
description: 對於 CSP 合作夥伴，Azure 委派的資源管理可藉由啟用細微的權限劃分來協助改善安全性與控管。
ms.date: 07/29/2020
ms.topic: conceptual
ms.openlocfilehash: 0ae7ef0b78f2f37708c6b442a9591fcbcce25646
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2020
ms.locfileid: "87429452"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse 與雲端解決方案提供者計畫

如果您是 [CSP (雲端解決方案提供者)](/partner-center/csp-overview) 合作夥伴，則您可能已使用[管理代表 (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) 功能，存取透過 CSP 計畫為您的客戶建立的 Azure 訂用帳戶。 此存取權可讓您直接支援、設定及管理客戶的訂用帳戶。

透過[Azure 燈塔](../overview.md)，您可以搭配 AOBO 使用 azure 委派的資源管理。 這有助於您為使用者啟用更精細的權限來改善安全性，以及減少不必要的存取。 此外，效率和延展性也得以提高，因為使用者可在您的租用戶中使用單一登入跨多個客戶訂用帳戶工作。

> [!TIP]
> 若要協助保護客戶資源，請務必檢閱並遵循我們的[建議安全做法](recommended-security-practices.md)及[合作夥伴安全性需求](/partner-center/partner-security-requirements)。

## <a name="administer-on-behalf-of-aobo"></a>管理代表 (AOBO)

若使用 AOBO，則您租用戶中有[系統管理代理人](/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles)角色的任何使用者，都能使用 AOBO 來存取您透過 CSP 計畫建立的 Azure 訂用帳戶。 需要存取任何客戶訂用帳戶的任何使用者，都必須是此群組的成員。 AOBO 不允許建立搭配不同客戶來使用的不同群組，或為群組或使用者建立不同角色的彈性設定。

![使用 AOBO 進行租用戶管理](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Azure 委派的資源管理

使用 Azure 燈塔，您可以將不同的群組指派給不同的客戶或角色，如下圖所示。 因為使用者會透過 Azure 委派的資源管理來擁有適當的存取層級，所以您可以減少擁有系統管理代理人角色的使用者人數 (因而擁有完整的 AOBO 存取權)。 這有助於限制對客戶資源的非必要存取，進而提高安全性。 它也可讓您更有彈性地大規模管理多個客戶。

將您透過 CSP 計畫建立的訂用帳戶上線，會遵循將[訂用帳戶上架到 Azure 燈塔](../how-to/onboard-customer.md)中所述的步驟。 您租用戶中有「系統管理代理人」角色的任何使用者都能執行此上線作業。

![使用 AOBO 和 Azure 委派的資源管理進行租用戶管理](../media/csp-2.jpg)

> [!TIP]
> 透過雲端解決方案提供者（CSP）計畫的轉銷商所建立的訂用帳戶，不支援使用私人方案的[受控服務提供](managed-services-offers.md)。 您可以[使用 Azure Resource Manager 範本](../how-to/onboard-customer.md)，將這些訂用帳戶上架到 Azure 燈塔。

> [!NOTE]
> Azure 入口網站中的 [ [**我的客戶**] 頁面](../how-to/view-manage-customers.md)現在包含 [**雲端解決方案提供者（預覽）** ] 區段，其中會顯示已[簽署 Microsoft 客戶合約（MCA）](/partner-center/confirm-customer-agreement)且位於[Azure 方案](/partner-center/azure-plan-get-started)底下之 CSP 客戶的帳單資訊和資源。 如需詳細資訊，請參閱[開始使用您的 Microsoft 合作夥伴合約計費帳戶](../../cost-management-billing/understand/mpa-overview.md)。
>
> CSP 客戶可能會出現在本節中，不論他們是否也已針對 Azure 委派的資源管理上架。 如果有，它們也會出現在 [**客戶**] 區段中，如[查看和管理客戶和委派的資源](../how-to/view-manage-customers.md)中所述。 同樣地，CSP 客戶不需要出現在 [**我的客戶**] 的 [**雲端解決方案提供者（預覽）** ] 區段中，讓您將其上線以進行 Azure 委派的資源管理。

## <a name="next-steps"></a>後續步驟

- 了解[跨租用戶管理體驗](cross-tenant-management-experience.md)。
- 瞭解如何將[訂用帳戶上架到 Azure 燈塔](../how-to/onboard-customer.md)。
- 了解[雲端解決方案提供者計畫](/partner-center/csp-overview)。

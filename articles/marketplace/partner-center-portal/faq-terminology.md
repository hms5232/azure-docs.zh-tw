---
title: 商業市集分析常見問題與術語 - 合作夥伴中心
description: 取得合作夥伴中心的商業市集分析常見問題的解答。 本文包含適用於分析術語的資料字典。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/17/2020
author: shganesh-dev
ms.author: shganesh
ms.openlocfilehash: f0f14bf24bd867344ec72c86a6fd517085b66d1f
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87317547"
---
# <a name="commercial-marketplace-analytics-terminology-and-common-questions"></a>商業市集分析的術語和常見問題

本文說明合作夥伴中心裡分析訊息的常見問題，也提供分析術語的字典。

## <a name="common-questions"></a>常見問題

**我無法在合作夥伴中心檢視我的分析資料。當我存取這些頁面時，會看到下列訊息。這是為什麼？**

![您的供應項目尚未提供任何資料](./media/analytics-faq-no-data.png)

為什麼您會收到此訊息：

- 您已在市集中發佈的供應項目目前沒有任何下載數。 這可能表示您的供應項目已在市集上架，且在產品顯示頁面中也有客戶檢視，但客戶尚未採取行動購買及部署您的供應項目。
- 您的供應項目發佈可能正在進行中，尚未上架。 客戶只能取得上架的供應項目。 若要檢查供應項目的狀態，請查看[分析儀表板](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)中的 [概觀]。 如需詳細資訊，請參閱[商業市集分析中的摘要儀表板](./summary-dashboard.md)。
- 您的供應項目可能列為**與我連絡**，這是僅供列出的供應項目，客戶無法在市集中購買。 雖然這些供應項目會產生潛在客戶並分享給您，但無法為這些供應項目建立訂單，因為無法購買。 若要檢查您的供應項目列出類型，請移至設定頁面。

**我知道我有分析資料，但出現下列訊息：**

![指定的日期範圍沒有資料](./media/analytics-faq-data-range.png)

如果您收到此訊息，表示您有分析資料，但您選取的日期範圍沒有資料。 請選取不同的日期範圍或自訂日期範圍，以檢視 2010 之後的任何資料。 如需詳細資訊，請參閱[商業市集分析中的摘要儀表板](./summary-dashboard.md)的「日期範圍」一節。

## <a name="dictionary-of-data-terms"></a>資料字詞字典

| 屬性名稱 | 報表 | 定義|
|---|---|---|
| Azure 授權類型 | 客戶、訂單 | 客戶用來購買 Azure 的授權合約類型。 也稱為「通道」。 |
| Azure 授權類型：雲端解決方案提供者 | 客戶、訂單 | 終端客戶透過其雲端解決方案提供者 (作為您的轉銷商) 取得 Azure 和您的 Marketplace 供應項目。|
| Azure 授權類型：Enterprise | 客戶、訂單 | 終端客戶透過 Enterprise 合約 (直接與 Microsoft 簽署) 取得 Azure 和您的 Marketplace 供應項目。|
| Azure 授權類型：企業透過轉銷商  | 客戶、訂單 | 終端客戶透過協助他們與 Microsoft 簽署「Enterprise 合約」的轉銷商取得 Azure 和您的 Marketplace 供應項目。|
| Azure 授權類型：隨用隨付| 客戶、訂單 | 終端客戶透過「隨用隨付」合約 (直接與 Microsoft 簽署) 取得 Azure 和您的 Marketplace 供應項目。|
| 雲端執行個體名稱| 單| 發生 VM 部署的 Microsoft Cloud。|
| 雲端執行個體名稱：Azure 全域| 單| 公用的全域 Microsoft 雲端。|
| 雲端執行個體名稱：Azure Government | 單| 政府特有的 Microsoft 雲端，適用於下列其中一種政府：中國、德國或美國地區。| |
| 客戶縣/市| 客戶| 客戶提供的城市名稱。 縣/市可能與客戶 Azure 訂用帳戶中的縣/市不同。|
| 客戶通訊語言  | 客戶| 客戶慣用於通訊的語言。|
| 客戶公司名稱 | 客戶、訂單 | 客戶提供的公司名稱。 名稱可能與客戶 Azure 訂用帳戶中的名稱不同。|
| 客戶國家/地區 | 客戶、訂單 | 客戶提供的國家/地區名稱。 國家/地區可能與客戶 Azure 訂用帳戶中的國家/地區不同。|
| 客戶電子郵件| 客戶| 客戶提供的電子郵件地址。 電子郵件可能與客戶 Azure 訂用帳戶中的電子郵件地址不同。|
| 客戶名字| 客戶| 客戶提供的名稱。 名稱可能與客戶 Azure 訂用帳戶中提供的名稱不同。|
| 客戶識別碼 | 客戶、訂單 | 指派給客戶的唯一識別碼。 客戶可能會有零或多個 Azure Marketplace 訂用帳戶。|
| 客戶郵遞區號  | 客戶| 客戶提供的郵遞區號。 郵遞區號可能與客戶 Azure 訂用帳戶中提供的郵遞區號不同。|
| 客戶州| 客戶| 客戶提供的州 (地址)。 州可能與客戶 Azure 訂用帳戶中提供的州不同。|
| 取得日期| 客戶| 客戶購買您所發佈之任何供應項目的第一個日期。|
| 損失日期| 客戶| 客戶取消先前購買的所有供應項目中最後一個的日期。|
| 是新的客戶  | 單| 此值將會識別一或多個第一次 (或非第一次) 取得您的供應項目的新客戶。 如果是在「取得日期」的同一個日曆月份內，值會是「是」。 如果客戶已在報告的日曆月份之前購買任何您的供應項目，值將會是「否」。 |
| 是預覽 SKU| 單| 此值可讓您知道您是否已將 SKU 標記為「預覽」。 如果 SKU 已適當標記，則值為「是」，而且只有您所授權的 Azure 訂用帳戶可以部署和使用此映像。 如果 SKU 尚未識別為「預覽」，則值為「否」。 |
| 已選擇加入促銷連絡人| 客戶| 此值可讓您知道客戶是否主動選擇加入發行者的促銷連絡人。 目前，我們沒有向客戶顯示該選項，因此我們一律指出 [否]。 一旦部署了此功能，我們就會開始據以更新。|
| Marketplace 授權類型| 單| Marketplace 供應項目的計費方法。|
| Marketplace 授權類型：透過 Azure 計費| 單| Microsoft 是您對於此 Marketplace 供應項目的代理人，並且會代替您向客戶收取費用。 (PAYG 信用卡或企業發票)|
| Marketplace 授權類型：自備授權 | 單| VM 需要客戶提供的授權金鑰才能部署。 Microsoft 不會向客戶收取透過 Marketplace 以這種方式列出供應項目的費用。|
| Marketplace 授權類型：免費| 單| 供應項目設定為對所有使用者都是免費的。 Microsoft 不會針對此供應項目的使用量向客戶收取費用。|
| Marketplace 授權類型：Microsoft 為轉銷商  | 單| Microsoft 是您此 Marketplace 供應項目的轉銷商。|
| Marketplace 訂用帳戶識別碼 | 客戶、訂單 | 客戶用來購買您的 Marketplace 供應項目之 Azure 訂用帳戶的相關聯唯一識別碼。 識別碼先前是 Azure 訂用帳戶 GUID。|
| 供應項目名稱  | 單| Marketplace 供應項目的名稱。|
| 供應項目類型  | 單| Microsoft Marketplace 供應項目的類型。|
| 供應項目類型：受控應用程式  | 單 | 必須符合下列條件時，請使用「Azure 應用程式：受控應用程式」供應項目類型：您使用 VM 或整個 IaaS 型解決方案為客戶部署訂用帳戶型解決方案。 您或您的客戶要求由合作夥伴管理解決方案。 |
| 供應項目類型：Azure 應用程式| 單 | 當您的解決方案除了簡單的 VM 之外，還需要以自動化方式進行額外的部署和設定時，請使用「Azure 應用程式」解決方案範本供應項目類型。|
| 供應項目類型：諮詢服務| 單| Azure Marketplace 中的諮詢服務可協助連結客戶與服務，以支援並擴大客戶對 Azure 的使用。|
| 供應項目類型：容器 | 單| 當您的解決方案是佈建成 Kubernetes 型 Azure 容器服務的 Docker 容器映像時，請使用「容器」供應項目類型。|
| 供應項目類型：Dynamics 365 Business Central| 單| 當您的解決方案與 Dynamics 365 有財務和營運方面的整合時，請使用此供應項目類型|
| 供應項目類型：Dynamics 365 for Customer Engagement | 單| 當您的解決方案與 Dynamics 365 有客戶業務開發方面的整合時，請使用此供應項目類型。|
| 供應項目類型：IoT Edge 模組 | 單| Azure IoT Edge 模組是 IoT Edge 所管理的最小計算單位，並可包含 Microsoft 服務 (例如「Azure 串流分析」)、第三方服務，或您自己的解決方案特定程式碼。 |
| 供應項目類型：Power BI 應用程式 | 單| 當您部署已與 Power BI 整合的應用程式時，請使用 Power BI 應用程式供應項目類型。|
| 供應項目類型：SaaS 應用程式| 單| 您可以使用「SaaS 應用程式」供應項目類型，讓客戶以訂用帳戶的形式購買您的 SaaS 型技術解決方案。||
| 供應項目類型：虛擬機器 | 單 | 當您要將虛擬設備部署到與客戶相關的訂用帳戶時，請使用「虛擬機器」供應項目類型。|
| 供應項目類型：Visual Studio Marketplace  | 單| 之前可供 Azure DevOps 擴充功能開發人員使用的供應項目類型。 日後，Azure DevOps 擴充功能開發人員可以將其擴充功能直接出售給客戶。 擴充功能供應項目可以設定為付費或包含試用版。 |
| 訂單取消日期| 單| Marketplace 訂單取消日期。|
| 訂單識別碼| 單| Marketplace 服務的客戶訂單唯一識別碼。 Virtual Machine 用法型供應項目不會與訂單有關聯。|
| 訂單購買日期| 單| Marketplace 訂單建立日期。|
| 訂單狀態| 單| 在資料上次重新整理時，Marketplace 的狀態。|
| 訂單狀態：Active  | 單| 客戶已購買訂單，且未取消其訂單。|
| 訂單狀態：已取消 | 單| 客戶先前已購買訂單，但之後又取消其訂單。|
| 提供者電子郵件| 客戶| Microsoft 與終端客戶兩者關係的相關提供者的電子郵件地址。 如果客戶是透過轉銷商的企業，就會是轉銷商。 如果涉及雲端解決方案提供者 (CSP)，就會是 CSP。|
| Provider Name| 客戶| Microsoft 與終端客戶兩者關係的相關提供者的名稱。 如果客戶是透過轉銷商的企業，就會是轉銷商。 如果涉及雲端解決方案提供者 (CSP)，就會是 CSP。|
| SKU| 單| 發佈期間定義的 SKU （現在稱為方案）名稱。 供應專案可能有許多 Sku （方案），但每個都只能與單一供應專案相關聯。|
| 試用結束日期| 單| 此訂單將結束或已結束的日期。|
|||

## <a name="next-steps"></a>後續步驟

- 如需合作夥伴中心商業市集中可用的分析報告概觀，請參閱[合作夥伴中心的商業市集分析](./analytics.md)。
- 如需摘要說明供應項目市集活動的彙總資料圖表、趨勢和值，請參閱[商業市集分析中的摘要儀表板](./summary-dashboard.md)。
- 如需圖形化和可下載格式的訂單資訊，請參閱[商業市集分析中的訂單儀表板](./orders-dashboard.md)。
- 如需虛擬機器 (VM) 供應項目使用量和計量帳單計量，請參閱[商業市集分析中的使用量儀表板](./usage-dashboard.md)。
- 如需客戶的詳細資訊 (包括成長趨勢)，請參閱[商業市集分析中的客戶儀表板](./customer-dashboard.md)。
- 如需過去 30 天內的下載要求清單，請參閱[商業市集分析中的下載儀表板](./downloads-dashboard.md)。
- 若要查看 Azure Marketplace 和 AppSource 上客戶對於供應項目的意見反應彙總檢視，請參閱[商業市集分析中的評等和評論儀表板](./ratings-reviews.md)。

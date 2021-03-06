---
title: 安全性中心規劃和操作指南
description: 本文件可協助您在採用 Azure 資訊安全中心和日常作業相關考量之前進行規劃。
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 45d3ec8dc5d819464046e40bab22491a4bccde63
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2020
ms.locfileid: "89461318"
---
# <a name="planning-and-operations-guide"></a>規劃與操作指南
本指南適用于資訊技術 (IT) 專業人員、IT 架構設計人員、資訊安全性分析師，以及規劃使用 Azure 資訊安全中心的雲端系統管理員。


## <a name="planning-guide"></a>規劃指南
本指南涵蓋的工作可讓您根據組織的安全性需求和雲端管理模型，將您的資訊安全中心使用優化。 若要充分利用資訊安全中心，務必了解您組織中的不同人員或小組如何使用此服務，來滿足安全地開發和作業、監視、控管和事件回應的需求。 規劃使用資訊安全中心時所應考量的主要領域如下︰

* 安全性角色和存取控制
* 安全性原則和建議
* 資料收集和儲存
* 上架非 Azure 資源
* 持續安全性監視
* 事件回應

在下一節中，您將學習如何根據需求來規劃每個領域及套用這些建議。


> [!NOTE]
> 如需在設計和規劃階段也很好用的常見問題清單，請閱讀 [Azure 資訊安全中心常見問題集 (FAQ)](faq-general.md) 。

## <a name="security-roles-and-access-controls"></a>安全性角色和存取控制
根據您組織的大小和結構，多個個人和小組可以使用資訊安全中心來執行不同的安全性相關工作。 下圖是虛構人物與其各自角色和安全性責任的範例︰

![角色](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

資訊安全中心可讓這些人符合不同的責任。 例如：

**Jeff (工作負載擁有者)**

* 管理雲端工作負載和其相關的資源
* 負責根據公司的安全性原則來實作和維護保護

**Ellen (CISO/CIO)**

* 負責公司安全性的所有層面
* 想要了解各雲端工作負載之間公司的安全性狀態
* 需要知道有關主要攻擊和風險的資訊

**David (IT 安全性)**

* 設定公司安全性原則，以確保適當的保護已就位
* 監控安全性原則的遵循狀態
* 產生報告以供主管或稽核人員使用

**Judy (安全性作業)**

* 隨時監控並回應安全性警示
* 呈報給雲端工作負載擁有者或 IT 安全性分析師

**Sam (安全性分析師)**

* 調查攻擊
* 與雲端工作負載擁有者合作以套用補救方法

安全性中心會使用 [azure 角色型存取控制 (AZURE RBAC) ](../role-based-access-control/role-assignments-portal.md)，其提供可指派給 azure 中使用者、群組和服務的 [內建角色](../role-based-access-control/built-in-roles.md) 。 當使用者開啟資訊安全中心時，他們只能看到與自己能夠存取的資源相關的資訊。 這表示會為使用者指派資源所屬訂用帳戶或資源群組的「擁有者」、「參與者」或「讀者」角色。 除了這些角色，有兩個特定的資訊安全中心角色：

- **安全性讀取者**：屬於此角色的使用者僅能檢視資訊安全中心設定 (包括建議、警示、原則和健康情況)，但是無法進行變更。
- **安全性管理員**：與安全性讀取者相同，但它也可以更新安全性原則，解除建議和警示。

上述的資訊安全中心角色無法存取 Azure 的其他服務區域，例如儲存體、Web 和行動或物聯網。

使用在上圖中所述的人物，會需要下列 RBAC：

**Jeff (工作負載擁有者)**

* 資源群組擁有者/參與者

**Ellen (CISO/CIO)**

* 訂用帳戶擁有者/參與者或安全性系統管理員

**David (IT 安全性)**

* 訂用帳戶擁有者/參與者或安全性系統管理員

**Judy (安全性作業)**

* 可檢視警示的訂用帳戶讀取者或安全性讀取者
* 訂用帳戶擁有者/參與者或安全性系統管理員需要關閉警示

**Sam (安全性分析師)**

* 可檢視警示的訂用帳戶讀取者
* 關閉警示所需的訂用帳戶擁有者/參與者
* 可能需要存取工作區

需要考量的其他重要資訊︰

* 只有訂用帳戶擁有者/參與者和安全性管理員可以編輯安全性原則。
* 只有訂用帳戶和資源群組擁有者和參與者可以套用資源的安全性建議。

使用 RBAC 規劃資訊安全中心的存取控制時，請務必了解您的組織中有誰會使用資訊安全中心， 以及他們會執行的工作，然後據此設定 RBAC。

> [!NOTE]
> 我們建議您指派所需的最寬鬆角色，以便使用者完成其工作。 例如，只需要檢視資源安全性狀態的相關資訊，但不採取行動 (例如套用建議或編輯原則) 的使用者應被指派「讀者」角色。
>
>

## <a name="security-policies-and-recommendations"></a>安全性原則和建議
安全性原則會定義工作負載的所需設定，並且協助確保符合公司或法規安全性需求。 您可以在資訊安全中心為 Azure 訂用帳戶定義原則，可以針對工作負載類型或資料機密性訂製。

資訊安全中心原則包含下列元件：
- [資料收集](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)：代理程式佈建和資料收集設定。
- [安全性原則](https://docs.microsoft.com/azure/security-center/security-center-policies)：一種 [Azure 原則](../governance/policy/overview.md) ，可判斷哪些控制項由安全性中心監視及建議，或使用 Azure 原則來建立新的定義、定義額外的原則，以及跨管理群組指派原則。
- [電子郵件通知](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)：安全性連絡人和通知設定。
- [定價層](https://docs.microsoft.com/azure/security-center/security-center-pricing)：免費或標準定價選項，用來決定哪個資訊安全中心功能可用於範圍中的資源 (可以針對訂用帳戶、資源群組和工作區指定)。

> [!NOTE]
> 指定安全性連絡人可確保如果發生安全性事件，Azure 可以觸達您組織中適當的人員。 如需如何啟用這項建議的詳細資訊，請閱讀 [在 Azure 資訊安全中心提供安全性連絡人詳細資料](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) 。

### <a name="security-policies-definitions-and-recommendations"></a>安全性原則定義和建議
資訊安全中心會為每個 Azure 訂用帳戶自動建立預設安全性原則。 您可以在資訊安全中心編輯原則，或使用 Azure 原則來建立新的定義、定義額外的原則，以及在整個管理群組 (可以代表整個組織、其中的營業單位等等) 指派原則，並且監視這些領域中這些原則的合規性。

在設定安全性原則之前，請檢閱每個 [安全性建議](https://docs.microsoft.com/azure/security-center/security-center-recommendations)，並判斷這些原則是否適合您的各種訂用帳戶和資源群組。 此外，也務必了解該採取什麼動作才能處理安全性建議，以及您組織將負責監視新建議及採取必要步驟的人員。

## <a name="data-collection-and-storage"></a>資料收集和儲存
Azure 資訊安全中心使用 Log Analytics 代理程式–這是 Azure 監視器服務所使用的相同代理程式–用來從您的虛擬機器收集安全性資料。 從這個代理程式[收集的資料](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)會儲存在 Log Analytics 工作區中。

### <a name="agent"></a>代理程式

在安全性原則中啟用自動布建時，適用于 [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) 或 [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) 的 Log Analytics 代理程式 (會安裝在所有支援的 Azure vm 和任何新建立的虛擬機器上。 如果 VM 或電腦已經安裝 Log Analytics 代理程式，Azure 資訊安全中心將會利用目前安裝的代理程式。 代理程式的程式設計為不具侵入性，且對 VM 效能的影響極低。

適用于 Windows 的 Log Analytics 代理程式需要使用 TCP 埠443。 如需詳細資訊，請參閱[疑難排解文章](security-center-troubleshooting-guide.md)。

如果您想在某個時間點停用資料收集，您可以在安全性原則中將它關閉。 不過，因為其他 Azure 管理和監視服務可能會使用 Log Analytics 代理程式，當您在 [資訊中心] 中關閉資料收集時，將不會自動卸載代理程式。 您可以視需要手動解除安裝代理程式。

> [!NOTE]
> 若要尋找支援的 Vm 清單，請閱讀 [Azure 資訊安全中心常見問題) 的常見問題 (](faq-vms.md)。

### <a name="workspace"></a>工作區

工作區是一項 Azure 資源，作為資料的容器。 您或組織的其他成員可能會使用多個工作區來管理從所有或部分 IT 基礎結構收集而來的不同資料。

從 Log Analytics 代理程式收集而來的資料 (代表 Azure 資訊安全中心) 將會儲存在與您的 Azure 訂用帳戶相關聯的現有 Log Analytics 工作 (區中，或)  (的新工作區，將 VM 的地理位置納入考慮。

在 Azure 入口網站中，您可以瀏覽以查看 Log Analytics 工作區清單，包括 Azure 資訊安全中心所建立的任何工作區。 將會針對新的工作區建立相關的資源群組。 兩者都會遵照此命名慣例：

* 工作區：*DefaultWorkspace-[subscription-ID]-[geo]*
* 資源群組：*DefaultResourceGroup-[geo]*

若為 Azure 資訊安全中心所建立的工作區，資料會保留 30 天。 若為現有工作區，保留是以工作區定價層為基礎。 如果您想要的話，也可以使用現有的工作區。

> [!NOTE]
> Microsoft 鄭重保證會保護此資料的隱私和安全。 Microsoft 從撰寫程式碼到運作服務均遵守嚴格的規範與安全性指導方針。 如需資料處理和隱私權的詳細資訊，請閱讀 [Azure 資訊安全中心資料安全性](security-center-data-security.md)。
>

## <a name="onboarding-non-azure-resources"></a>上架非 Azure 資源

資訊安全中心可以監視非 Azure 電腦的安全性狀態，但您需要先上架這些資源。 請參閱[上架到 Azure 資訊安全中心標準版以增強安全性](https://docs.microsoft.com/azure/security-center/security-center-onboarding#onboard-non-azure-computers)，以取得如何上架非 Azure 資源的詳細資訊。

## <a name="ongoing-security-monitoring"></a>持續安全性監視
初始設定和套用資訊安全中心建議之後，下一步是考量資訊安全中心操作程序。

資訊安全中心概觀會提供跨您已連線之所有 Azure 資源和任何非 Azure 資源的整合檢視。 下列範例顯示具有許多問題必須解決的環境：

![儀表板](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig11.png)

> [!NOTE]
> 資訊安全中心不會干擾一般作業程序，它會被動地監視您的部署，並根據您啟用的安全性原則提供建議。

當您第一次選擇在目前的 Azure 環境中使用安全性中心時，請確定您已查看所有建議，其可在 **建議** 頁面中完成。

計劃造訪威脅情報選項，作為每日安全性作業的一部分。 您可以在那裡識別會對環境造成威脅的安全性威脅，例如識別特定電腦是否為殭屍網路的一份子。

### <a name="monitoring-for-new-or-changed-resources"></a>監視新的或已變更的資源

大部分的 Azure 環境都是動態的，而資源會定期建立、啟動或關閉、重新設定和變更。 資訊安全中心有助於確保您看得到這些新資源的安全性狀態。

當您將新資源 (VM、SQL DB) 新增至 Azure 環境時，資訊安全中心會自動探索這些資源並開始監視其安全性。 這也包括 PaaS Web 角色和背景工作角色。 如果已在 [安全性原則](tutorial-security-policy.md)中啟用資料收集，則會自動為您的虛擬機器啟用其他監視功能。

您也應該定期監視現有的資源，找出可能已建立安全性風險的設定變更、來自建議基準的漂移和安全性警示。 

### <a name="hardening-access-and-applications"></a>強化存取和應用程式

您也應該在安全性作業中採用預防措施，來限制對 VM 的存取，並且控制在 VM 上執行的應用程式。 藉由鎖定前往您的 Azure VM 的輸入流量，可以降低被攻擊的曝光度，同時提供簡易存取以在需要時連線到 VM。 使用 [即時 VM](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) 存取功能來強化 vm 的存取權。

您可以使用 [適應性應用](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application) 程式控制來限制哪些應用程式可以在 Azure 中的 vm 上執行。 除了其他優點之外，這也有助於強化 Vm 免于惡意程式碼的侵害。 資訊安全中心會使用機器學習來分析在 VM 中執行的進程，以協助您建立允許清單規則。


## <a name="incident-response"></a>事件回應
資訊安全中心會偵測並在發生威脅時警示您。 組織應監視新的安全性警示並視需要採取動作，進一步調查或修補攻擊。 如需資訊安全中心威脅防護運作方式的詳細資訊，請參閱 [Azure 資訊安全中心如何偵測和回應威脅](security-center-alerts-overview.md#detect-threats)。

雖然本文沒有協助您建立自己的事件回應計畫的意圖，但我們將在雲端生命週期中使用 Microsoft Azure 的安全性回應，作為事件回應階段的基礎。 這些階段如下圖所示：

![可疑的活動](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> 您可以使用美國國家標準技術局 (NIST) 的 [Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) 做為參考來協助您建置自己的計劃。
>

在下列階段期間，您可以使用資訊安全中心警示︰

* **偵測**︰識別一或多個資源中的可疑活動。
* **評估**︰執行初始評估，以取得可疑活動的詳細資訊。
* **診斷**︰使用補救步驟來進行解決問題的技術程序。

每個安全性警示都提供可用來進一步了解攻擊本質及建議可能補救措施的資訊。 有些警示也會提供更多資訊或 Azure 中其他資訊來源的連結。 您可以使用所提供的資訊進一步研究並開始補救，也可以搜尋儲存在工作區中的安全性相關資料。

下列範例說明正在發生的可疑 RDP 活動︰

![可疑的活動](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

此分頁會顯示有關攻擊發生時間、來源主機名稱、目標 VM 的詳細資訊，也會提供建議步驟。 在某些情況下，攻擊的來源資訊可能是空的。 如需有關這類行為的詳細資訊，請閱讀 [Azure 資訊安全中心警示中的缺少來源資訊](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) 。

您也可以從這個分頁開始調查，以更加了解攻擊的時間軸、攻擊如何發生、哪些系統可能會被入侵、哪些認證會被使用，以及查看整個攻擊鏈的圖形化表示法。

一旦您識別出遭入侵的系統之後，您就可以執行先前建立的 [工作流程自動化](workflow-automation.md) 。 這些是一組程式，可在警示觸發之後，從「安全性中心」執行。

在「 [如何利用 Azure 資訊安全中心 & Microsoft Operations Management Suite 事件回應](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) 影片中，您可以看到一些示範，可協助您瞭解資訊安全中心如何在這兩個階段中使用。

> [!NOTE]
> 如需有關如何使用資訊安全中心功能協助您進行事件回應程式的詳細資訊，請參閱 [Azure 資訊安全中心中的管理及回應安全性警示](security-center-managing-and-responding-alerts.md) 。
>
>

## <a name="next-steps"></a>接下來的步驟
在本文件中，您已了解如何規劃資訊安全中心的採用。 如要深入了解資訊安全中心，請參閱下列主題：

* [在 Azure 資訊安全中心中管理及回應安全性警示](security-center-managing-and-responding-alerts.md)
* [Azure 資訊安全中心中的安全性健康情況監視](security-center-monitoring.md) -瞭解如何監視 Azure 資源的健康情況。
* [使用 Azure 資訊安全中心監視合作夥伴解決方案](security-center-partner-solutions.md) — 了解如何監視合作夥伴解決方案的健全狀況。
* [AZURE 資訊安全中心常見問題](faq-general.md) ：尋找有關使用服務的常見問題。
* [Azure 安全性 blog](https://docs.microsoft.com/archive/blogs/azuresecurity/) -尋找有關 Azure 安全性與合規性的 blog 文章。

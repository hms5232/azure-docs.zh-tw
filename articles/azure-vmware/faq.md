---
title: 常見問題集
description: 提供有關 Azure VMware 解決方案的一些常見問題解答。
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: dikamath
ms.openlocfilehash: 7b4abc2d711a3da6a6df125854759e083d7e04a7
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817831"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution-preview"></a>關於 Azure VMware 解決方案預覽的常見問題

關於 Azure VMware 解決方案的常見問題的解答。

## <a name="general"></a>一般

### <a name="what-is-azure-vmware-solution"></a>什麼是 Azure VMware 解決方案？

隨著企業採用 IT 現代化策略來提升業務靈活性、降低成本並加速創新，混合式雲端平台也已成為客戶數位轉型的關鍵實現。 Azure VMware 解決方案結合 VMware 的軟體定義資料中心 (SDDC) 軟體與 Microsoft Azure 的全球雲端服務生態系統。 Azure VMware 解決方案的管理是為了符合效能、可用性、安全性和合規性需求。

## <a name="azure-vmware-solution-service"></a>Azure VMware 解決方案服務

### <a name="where-is-azure-vmware-solution-available-today"></a>Azure VMware 解決方案目前的推出位置為何？

服務會持續新增至新區域，因此如需詳細資訊，請參閱 [最新的服務可用性資訊](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) 。 

### <a name="can-workloads-running-in-an-azure-vmware-solution-instance-consume-or-integrate-with-azure-services"></a>在 Azure VMware 解決方案實例中執行的工作負載是否可以取用或與 Azure 服務整合？

Azure VMware 解決方案客戶將可使用所有 Azure 服務。 特定服務的效能和可用性限制必須以案例為基礎來解決。

### <a name="do-i-use-the-same-tools-that-i-use-now-to-manage-private-cloud-resources"></a>我是否使用此時用來管理私人雲端資源的相同工具？

是。 Azure 入口網站用於部署和一些管理作業。 vCenter 和 NSX Manager 可用來管理 vSphere 和 NSX-T 資源。

### <a name="can-i-manage-a-private-cloud-with-my-on-premises-vcenter"></a>我是否可以使用我的內部部署 vCenter 來管理私人雲端？

在啟動時，Azure VMware 解決方案不支援跨內部部署和私用雲端環境的單一管理體驗。 私人雲端叢集會使用 vCenter 和在私人雲端的本地 NSX Manager 來管理。

### <a name="can-i-use-vrealize-suite-running-on-premises"></a>我是否可以使用執行於內部部署的 vRealize Suite？ 

特定的整合和使用案例可能會以個案為基礎進行評估。

### <a name="can-i-migrate-vsphere-vms-from-on-premises-environments-to-azure-vmware-solution-private-clouds"></a>是否可將 vSphere Vm 從內部部署環境遷移至 Azure VMware 解決方案私人雲端？

可以。 如果符合標準的跨 vCenter [vMotion 需求](https://kb.vmware.com/s/article/210695) ，則可以使用 VM 遷移和 VMotion 將 vm 移至私人雲端。

### <a name="is-a-specific-version-of-vsphere-required-in-on-premises-environments"></a>內部部署環境中是否需要特定版本的 vSphere？

由於所有雲端環境都隨附 HCX、vSphere 5.5 或更新版本，適用於 VMotion 的內部部署環境。

### <a name="what-does-the-change-control-process-look-like"></a>變更控制流程看起來像什麼？

對服務本身進行的更新將遵循 Microsoft Azure 的標準變更管理流程。 客戶須負責任何工作負載管理工作和相關聯的變更管理流程。

### <a name="how-is-this-different-from-azure-vmware-solution-by-cloudsimple"></a>這與 Azure VMware Solution by CloudSimple 有何不同？

透過新的 Azure VMware 解決方案，Microsoft 和 VMware 具有直接的雲端提供者合作關係。 新的解決方案完全是由 Microsoft 設計、建立及支援，並由 VMware 背書。 在架構上，解決方案是一致的，而且 VMware 技術堆疊會在 Azure 專用基礎結構上執行。

### <a name="if-im-an-existing-azure-vmware-solution-customer-what-does-this-preview-mean-for-me"></a>如果我是現有的 Azure VMware 解決方案客戶，此預覽對我有何意義？

CloudSimple 不會對現有的 Azure VMware 解決方案進行任何變更。 我們會繼續在 Azure 上支援該解決方案。 Azure VMware 解決方案受我們的服務等級協定 [ (SLA) ](https://aka.ms/CSVMwareSLA)支援。 客戶應該繼續針對生產環境工作負載使用此服務；這是 [Microsoft 的服務條款](https://azure.microsoft.com/support/legal/)所管理的可用解決方案。

### <a name="can-i-migrate-from-azure-vmware-solution-by-cloudsimple-to-this-new-solution"></a>我是否可以透過 CloudSimple 將 Azure VMware 解決方案遷移到這個新解決方案？

是，Azure VMware 解決方案支援使用熟悉的 VMware 工具 (例如 HCX) 來進行移轉。 如果客戶想要遷移至新的解決方案，請與您的 Microsoft 帳戶團隊合作，以探索選項和可用的支援。



## <a name="compute-network-and-storage"></a>計算、網路和儲存體

### <a name="is-there-more-than-one-type-of-host-available"></a>是否有一種以上的主機可供使用？

只有一種可用的主機。

### <a name="what-are-the-cpu-specifications-in-each-type-of-host"></a>每種主機類型的 CPU 規格為何？

伺服器有兩個雙18核心 2.3 GHz Intel CPU。

### <a name="how-much-memory-is-in-each-host"></a>每部主機中有多少記憶體？

伺服器有 576 GB 的 RAM。

### <a name="what-is-the-storage-capacity-of-each-host"></a>每台主機的儲存容量是多少？

每個 ESXi 主機都有兩個 vSAN diskgroups，容量層級為 15.2 TB，3.2 TB NVMe 快取層 (1.6 TB 的每個 diskgroup) 。

### <a name="how-much-network-bandwidth-is-available-in-each-esxi-host"></a>每部 ESXi 主機有多少網路頻寬可用？

每個 ESXi 主機都是使用 4 25 Gbps 的 Nic 設定的 Azure VMware 解決方案，其中有兩個 Nic 為 ESXi 系統流量布建，以及針對工作負載流量布建兩個 nic。 

### <a name="is-data-stored-on-the-vsan-datastores-encrypted-at-rest"></a>資料會儲存在 vSAN 資料存放區上加密嗎？

是，所有 vSAN 資料預設都會使用儲存在 Azure Key Vault 中的金鑰進行加密。

## <a name="hosts-clusters-and-private-clouds"></a>主機、叢集和私人雲端

### <a name="is-the-underlying-infrastructure-shared"></a>基礎結構是否共用？

否，私人雲端主機和叢集是專用的，而且會在使用前後安全地清除。

### <a name="what-are-the-minimum-and-maximum-number-of-hosts-per-cluster"></a>每個叢集的主機數目下限和上限為何？

叢集可以在 3 到 16 個 ESXi 主機之間調整。 試用叢集限制為 3 部主機。

### <a name="can-i-scale-my-private-cloud-clusters"></a>我是否可以調整私人雲端叢集？

是，叢集會在 ESXi 主機的最小和最大數目之間進行調整。 試用叢集限制為 3 部主機。

### <a name="what-are-trial-clusters"></a>什麼是試用叢集？

試用叢集是三個主機叢集，用於 Azure VMware 解決方案私人雲端的一個月評估。

### <a name="can-i-use-high-end-hosts-for-trial-clusters"></a>我是否可以針對試用叢集使用高階主機？

否。 高階 ESXi 主機已保留供生產叢集使用。

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware 解決方案和 VMware 軟體

### <a name="what-versions-of-vmware-software-is-used-in-private-clouds"></a>哪些版本的 VMware 軟體用於私人雲端？

私人雲端使用 vSphere 6.7、vSAN 6.7、HCX 和版本2.5 的 NSX-T。  

### <a name="do-private-clouds-use-vmware-nsx"></a>私人雲端是否使用 VMware NSX？

是，NSX-T 2.5 用於 Azure VMware 解決方案私人雲端中的軟體定義網路。

### <a name="can-i-use-vmware-nsx-v-in-a-private-cloud"></a>我是否可以在私人雲端中使用 VMware NSX-V？

否。 NSX-T 是唯一支援的 NSX 版本。

### <a name="is-nsx-required-in-on-premises-environments-or-networks-that-connect-to-a-private-cloud"></a>內部部署環境或連線到私人雲端的網路中是否需要 NSX？

否，您不需要在內部部署使用 NSX。

### <a name="what-is-the-upgrade-and-update-schedule-for-vmware-software-in-a-private-cloud"></a>在私人雲端中，VMware 軟體的升級和更新排程為何？

私用雲端軟體組合升級的目的，是要將軟體從 VMware 保存在最新版本的軟體套件組合的某個版本中。 私用雲端軟體版本可能與個別軟體元件的最新版本不同 (ESXi、NSX-T、vCenter、vSAN) 。

### <a name="how-often-will-the-private-cloud-software-stack-be-updated"></a>私人雲端軟體堆疊的更新頻率為何？

私人雲端軟體會依照排程進行升級，並隨著 VMware 的軟體搭售方案發行而進行追蹤。 您的私人雲端不需要停機就能進行升級。

## <a name="connectivity"></a>連線能力

### <a name="what-network-ip-address-planning-is-required-to-incorporate-private-clouds-with-on-premises-environments"></a>若要將私人雲端與內部部署環境結合，需要何種網路 IP 位址方案？

需要私人網路/22 位址空間，才能部署 Azure VMware 解決方案私人雲端。 此私人位址空間不應與訂閱中的其他虛擬網路或內部部署網路重疊。
 
### <a name="how-do-i-connect-from-on-premises-environments-to-an-azure-vmware-solution-private-cloud"></a>如何? 從內部部署環境連接到 Azure VMware 解決方案私人雲端？

您可以透過下列兩種方法的其中一種連接到服務： 

- 將 VM 或應用程式閘道部署在透過 ExpressRoute 對等互連至私人雲端的 Azure 虛擬網路上。
- 透過 ExpressRoute Global，從內部部署資料中心到 Azure ExpressRoute 線路。

### <a name="how-do-i-connect-a-workload-vm-to-the-internet-or-an-azure-service-endpoint"></a>如何將工作負載 VM 連線到網際網路或 Azure 服務端點？

在 Azure 入口網站中，啟用私人雲端的網際網路連線能力。 使用 NSX-T 管理員，建立 NSX-T T1 路由器和邏輯交換器。 接著，您可以使用 vCenter 在邏輯交換器所定義的網路區段上部署 VM。 該 VM 將可存取網際網路和 Azure 服務的網路。

### <a name="do-i-need-to-restrict-access-from-the-internet-to-vms-on-logical-networks-in-a-private-cloud"></a>我是否需要對於私人雲端之中邏輯網路上的 VM 限制從網際網路存取的存取權？

否。 不允許從網際網路對私人雲端直接輸入的網路流量。

### <a name="do-i-need-to-restrict-internet-access-from-vms-on-logical-networks-to-the-internet"></a>我是否需限制從邏輯網路上的 VM 存取網際網路線的存取權？

是。 您必須使用 NSX-T 管理員來建立防火牆，以限制 VM 對網際網路的存取。

## <a name="accounts-and-privileges"></a>帳戶和權限

### <a name="what-accounts-and-privileges-will-i-get-with-my-new-azure-vmware-solution-private-cloud"></a>我可以使用新的 Azure VMware 解決方案私人雲端來取得哪些帳戶和許可權？

系統會為您提供 vCenter 之中 cloudadmin 使用者的認證，以及在 NSX-T 管理員上的管理員存取權。 另外還有一個 CloudAdmin 群組可用於合併 Azure Active Directory。 如需詳細資訊，請參閱[存取和身分識別概念](concepts-identity.md)。

### <a name="can-have-administrator-access-to-esxi-hosts"></a>是否可以擁有 ESXi 主機的管理員存取權？

否，ESXi 的管理員存取權限制為符合解決方案的安全性需求。

### <a name="what-privileges-and-permissions-will-i-have-in-vcenter"></a>VCenter 有哪些使用權限和存取權限？

您將擁有 CloudAdmin 群組權限。 如需詳細資訊，請參閱[存取和身分識別概念](concepts-identity.md)。

### <a name="what-privileges-and-permissions-will-i-have-on-the-nsx-t-manager"></a>我對於 NSX-T 管理員會有哪些使用權限和存取權限？

您會擁有 NSX-T 的完整管理員權限，而且可以管理角色型存取控制，和您在內部部署的資料中心一樣。 如需詳細資訊，請參閱[存取和身分識別概念](concepts-identity.md)。

> [!NOTE]
> 建立 T0 路由器，並將其設定為私人雲端部署的一部份。 對該邏輯路由器或 NSX-T 邊緣節點 VM 所做的任何修改，可能會影響到私人雲端的連線能力。

## <a name="billing-and-support"></a>帳單與支援

### <a name="how-will-i-be-billed-during-the-preview-of-azure-vmware-solution"></a>Azure VMware 解決方案預覽期間會如何計費？

在預覽期間，Azure VMware 解決方案的計費是每月以隨用隨付的方式計費。 正式發行時，會提供其他選項。

### <a name="how-will-pricing-be-structured-during-the-preview-of-azure-vmware-solution"></a>Azure VMware 解決方案預覽期間會如何將定價結構化？

如需有關價格的一般問題，請參閱 Azure VMware 解決方案[價格](https://azure.microsoft.com/pricing/details/azure-vmware)頁面。 預覽定價可在要求中取得，請與您的帳戶小組聯繫，或遵循定價頁面上的連結來聯繫銷售。

### <a name="who-supports-azure-vmware-solution"></a>誰支援 Azure VMware 解決方案？

Azure VMware 解決方案的支援是由 Microsoft 提供。 請注意，根據我們的預覽指導方針，我們將會在9到 5 pm PST 上班時間星期一到星期五提供支援。 您可以從[此連結](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)提出支援票證

### <a name="what-accounts-do-i-need-to-create-an-azure-vmware-solution-private-cloud"></a>我需要哪些帳戶才能建立 Azure VMware 解決方案私人雲端？

Azure 訂閱需要 Azure 帳戶。

### <a name="how-do-i-request-a-host-quota-increase-for-azure-vmware-solution"></a>如何? 要求增加 Azure VMware 解決方案的主機配額嗎？

* 您將需要 [Azure Enterprise 合約 (EA) ](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-agreements) 的 Microsoft。
* Azure 訂閱需要 Azure 帳戶。

在您建立 Azure VMware 解決方案資源之前，您必須提交支援票證以配置您的節點。 當支援小組收到您的要求之後，最多需要五個工作天才能確認您的要求並配置您的節點。 如果您有現有的 Azure VMware 解決方案私人雲端，並想要配置更多節點，您將會經歷相同的程式。


1. 在您的 Azure 入口網站中的 [說明 **+ 支援**] 下，建立 **[新的支援要求](https://rc.portal.azure.com/#create/Microsoft.Support)** ，並為票證提供下列資訊：
   - **問題類型：** 技術需求
   - **訂** 用帳戶：選取您的訂用帳戶
   - **服務：** 所有服務都 > Azure VMware 解決方案
   - **資源：** 一般問題 
   - **摘要：** 需要容量
   - **問題類型：** 容量管理問題
   - **問題子類型：** 其他主機配額/容量的客戶要求

1. 在支援票證的 **描述** 中，于 [ **詳細資料** ] 索引標籤上提供：

   - POC 或生產 
   - 區域名稱
   - 節點數目
   - 任何其他詳細資料

   >[!NOTE]
   >Azure VMware 解決方案建議至少有三個節點來啟動您的私用雲端和冗余 N + 1 個節點。 

1. 選取 [ **審核 + 建立** ] 以提交要求。

   支援代表需要五個工作天的時間，才能確認您的要求。

   >[!IMPORTANT] 
   >如果您已經有現有的 Azure VMware 解決方案，而且您正在要求其他節點，請注意，我們需要五個工作天來配置節點。 

1. 在您可以布建節點之前，請確定您已在 Azure 入口網站中註冊 **MICROSOFT AVS** 資源提供者。  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   如需註冊資源提供者的其他方式，請參閱 [Azure 資源提供者和類型](../azure-resource-manager/management/resource-providers-and-types.md)。

<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md

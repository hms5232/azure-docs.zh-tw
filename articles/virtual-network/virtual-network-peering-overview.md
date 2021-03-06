---
title: Azure 虛擬網路對等互連
titlesuffix: Azure Virtual Network
description: 瞭解 Azure 中的虛擬網路對等互連，包括如何讓您連接 Azure 中的網路虛擬網路。
services: virtual-network
documentationcenter: na
author: altambaw
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2019
ms.author: kumud
ms.openlocfilehash: 6ba0caa2546fdb9d4845e2c7adf87a06d906dd12
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87265155"
---
# <a name="virtual-network-peering"></a>虛擬網路對等互連

虛擬網路對等互連可讓您順暢地連接[Azure 虛擬網路](virtual-networks-overview.md)中的網路。 虛擬網路針對連線能力會顯示為一個。 虛擬機器之間的流量會使用 Microsoft 骨幹基礎結構。 就像相同網路中的虛擬機器之間的流量一樣，流量只會透過 Microsoft 的*專用*網來路由傳送。

Azure 支援下列類型的對等互連：

* 虛擬網路對等互連：連接相同 Azure 區域內的虛擬網路。
* 全域虛擬網路對等互連：跨 Azure 區域連接虛擬網路。

使用虛擬網路對等互連 (不論是本機還是全球) 的優點包括︰

* 不同虛擬網路的資源之間具有低延遲、高頻寬連線。
* 一個虛擬網路中的資源與不同虛擬網路中的資源進行通訊的能力。
* 能夠在跨 Azure 訂用帳戶、Azure Active Directory 租使用者、部署模型和 Azure 區域之間的虛擬網路之間傳輸資料。
* 對等互連透過 Azure Resource Manager 建立之虛擬網路的能力。
* 將透過 Resource Manager 建立的虛擬網路對等互連的能力，透過傳統部署模型建立。 若要深入了解 Azure 部署模型，請參閱[了解 Azure 部署模型](../azure-resource-manager/management/deployment-models.md?toc=%2fazure%2fvirtual-network%2ftoc.json)。
* 建立對等互連時或建立對等互連之後，虛擬網路中的資源沒有停機時間。

對等互連虛擬網路之間的網路流量為私用。 虛擬網路之間的流量會保留在 Microsoft 骨幹網路上。 虛擬網路之間的通訊不需要公用網際網路、閘道或加密。

## <a name="connectivity"></a>連線能力

針對對等互連虛擬網路，任一虛擬網路中的資源都可以直接與對等互連虛擬網路中的資源連線。

在相同區域的對等互連虛擬網路中，虛擬機器之間的網路延遲與單一虛擬網路中的網路延遲相同。 網路輸送量為依照虛擬機器大小，按比例允許的頻寬。 對等互連內的頻寬沒有其他額外限制。

對等互連之虛擬網路中的虛擬機器之間的流量，會透過 Microsoft 骨幹基礎結構直接路由傳送，而不會透過閘道或透過公用網際網路來傳送。

您可以將網路安全性群組套用至任一虛擬網路，以封鎖其他虛擬網路或子網的存取。
設定虛擬網路對等互連時，請開啟或關閉虛擬網路之間的網路安全性群組規則。 如果您開啟對等互連虛擬網路之間的完整連線，您可以套用網路安全性群組來封鎖或拒絕特定的存取。 預設選項是 [完整連接]。 若要深入瞭解網路安全性群組，請參閱[安全性群組](security-overview.md)。

## <a name="service-chaining"></a>服務鏈結

服務連結可讓您透過使用者定義的路由，將流量從一個虛擬網路導向至對等互連網路中的虛擬應用裝置或閘道。

若要啟用服務鏈，請設定使用者定義的路由，指向對等互連虛擬網路中的虛擬機器，做為*下一個躍點*IP 位址。 使用者定義的路由也可以指向虛擬網路閘道，以啟用服務鏈。

您可以部署中樞和輪輻網路，其中的中樞虛擬網路會裝載基礎結構元件，例如網路虛擬裝置或 VPN 閘道。 所有輪輻虛擬網路可以接著與中樞虛擬網路對等互連。 流量會流經中樞虛擬網路中的網路虛擬裝置或 VPN 閘道。

虛擬網路對等互連可讓使用者定義路由中的下一個躍點成為對等互連虛擬網路中虛擬機器的 IP 位址或 VPN 閘道。 您無法使用指定 Azure ExpressRoute 閘道做為下一個躍點類型的使用者定義路由，在虛擬網路之間進行路由。 若要深入了解使用者定義的路由，請參閱[使用者定義的路由概觀](virtual-networks-udr-overview.md#user-defined)。 若要瞭解如何建立中樞和輪輻網路拓撲，請參閱[Azure 中的中樞輪輻網路拓撲](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)。

## <a name="gateways-and-on-premises-connectivity"></a>閘道及內部部署連線能力

每個虛擬網路（包括對等互連的虛擬網路）都可以有自己的閘道。 虛擬網路可以使用其閘道連接到內部部署網路。 您也可以使用閘道來設定[虛擬網路對虛擬網路](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)連線，即使是針對對等互連虛擬網路也一樣。

當您設定虛擬網路互連能力的兩個選項時，虛擬網路之間的流量會流經對等互連設定。 流量會使用 Azure 骨幹。

您也可以將對等互連虛擬網路中的閘道設定為內部部署網路的傳輸點。 在此情況下，使用遠端閘道的虛擬網路不能有自己的閘道。 虛擬網路只有一個閘道。 閘道可以是對等互連虛擬網路中的本機或遠端閘道，如下圖所示：

![虛擬網路對等互連傳輸](./media/virtual-networks-peering-overview/local-or-remote-gateway-in-peered-virual-network.png)

虛擬網路對等互連和全域虛擬網路對等互連都支援閘道傳輸。

支援透過不同部署模型建立的虛擬網路之間的閘道傳輸。 閘道必須位於 Resource Manager 模型中的虛擬網路中。 若要深入了解如何使用閘道來進行傳輸，請參閱[設定 VPN 閘道以在虛擬網路對等互連中進行傳輸](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)。

當您將共用單一 Azure ExpressRoute 連線的虛擬網路對等互連時，它們之間的流量會經過對等互連關聯性。 該流量會使用 Azure 骨幹網路。 您依然可以在每個虛擬網路中使用本機閘道來連線內部部署線路。 否則，您可以使用共用閘道，並設定內部部署連線的傳輸。

## <a name="troubleshoot"></a>疑難排解

若要確認是否已對等互連虛擬網路，您可以檢查有效的路由。 檢查虛擬網路中任何子網的網路介面路由。 如果虛擬網路對等互連存在，則虛擬網路內的所有子網路都具有下一個躍點類型為「VNet 對等互連」** 的路由 (對每個對等互連的虛擬網路中的每個位址空間而言)。 如需詳細資訊，請參閱[診斷虛擬機器路由問題](diagnose-network-routing-problem.md)。

您也可以使用 Azure 網路監看員，針對對等互連虛擬網路中的虛擬機器連線能力進行疑難排解。 連線能力檢查可讓您查看流量如何從來源虛擬機器的網路介面路由傳送到目的地虛擬機器的網路介面。 如需詳細資訊，請參閱[使用 Azure 入口網站對 Azure 網路監看員的連線進行疑難排解](../network-watcher/network-watcher-connectivity-portal.md#check-connectivity-to-a-virtual-machine)。

您也可以嘗試針對[虛擬網路對等互連問題進行疑難排解](virtual-network-troubleshoot-peering-issues.md)。

## <a name="constraints-for-peered-virtual-networks"></a>對等互連虛擬網路的條件約束<a name="requirements-and-constraints"></a>

只有在為虛擬網路建立全域的對等互連時，會受到下列限制：

* 一個虛擬網路中的資源無法與全域對等互連虛擬網路中基本內部 Load Balancer （ILB）的前端 IP 位址通訊。
* 某些使用基本負載平衡器的服務不適用於全域虛擬網路對等互連。 如需詳細資訊，請參閱與[全域 VNet 對等互連和負載平衡器相關的條件約束為何？](virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)。

如需詳細資訊，請參閱[需求和限制](virtual-network-manage-peering.md#requirements-and-constraints)。 若要深入瞭解支援的對等互連數目，請參閱[網路限制](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)。

## <a name="permissions"></a>權限

若要瞭解建立虛擬網路對等互連所需的許可權，請參閱[許可權](virtual-network-manage-peering.md#permissions)。

## <a name="pricing"></a>定價

使用虛擬網路對等互連連線的輸入和輸出流量需付費。 如需詳細資訊，請參閱[虛擬網路定價](https://azure.microsoft.com/pricing/details/virtual-network)。

閘道傳輸是一種對等互連屬性，可讓虛擬網路利用對等互連虛擬網路中的 VPN/ExpressRoute 閘道。 閘道傳輸適用于跨內部部署和網路對網路連線能力。 對等互連虛擬網路中閘道（輸入或輸出）的流量會在輪輻 VNet （或非閘道 VNet）上產生虛擬網路對等互連費用。 如需詳細資訊，請參閱 VPN 閘道費用的[VPN 閘道定價](https://azure.microsoft.com/pricing/details/vpn-gateway/)和 expressroute 閘道費用的 Expressroute 閘道定價。

>[!NOTE]
> 本檔的先前版本指出，虛擬網路對等互連費用不適用於具有閘道傳輸的輪輻 VNet （或非閘道 VNet）。 它現在會根據定價頁面反映精確的定價。

## <a name="next-steps"></a>後續步驟

* 您可以建立兩個虛擬網路之間的對等互連。 網路可以屬於相同的訂用帳戶、相同訂用帳戶中的不同部署模型，或不同的訂閱。 完成下列其中一個案例的教學課程：

    |Azure 部署模型             | 訂用帳戶  |
    |---------                          |---------|
    |兩者皆使用 Resource Manager              |[相同](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[不同](create-peering-different-subscriptions.md)|
    |一個使用 Resource Manager、一個使用傳統部署模型  |[相同](create-peering-different-deployment-models.md)|
    |                                   |[不同](create-peering-different-deployment-models-subscriptions.md)|

* 若要瞭解如何建立中樞和輪輻網路拓撲，請參閱[Azure 中的中樞輪輻網路拓撲](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)。
* 若要深入瞭解所有虛擬網路對等互連設定，請參閱[建立、變更或刪除虛擬網路對等互連](virtual-network-manage-peering.md)。
* 如需常見的虛擬網路對等互連和全域虛擬網路對等互連問題的解答，請參閱[VNet](virtual-networks-faq.md#vnet-peering)對等互連。

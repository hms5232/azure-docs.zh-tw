---
title: 使用 PowerShell 將舊版直接對等互連轉換為 Azure 資源
titleSuffix: Azure
description: 使用 PowerShell 將舊版直接對等互連轉換為 Azure 資源
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d3e7cdf11e1e1e033b4e72b9579d8c63b28e6c88
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89071681"
---
# <a name="convert-a-legacy-direct-peering-to-an-azure-resource-by-using-powershell"></a>使用 PowerShell 將舊版直接對等互連轉換為 Azure 資源

本文說明如何使用 PowerShell Cmdlet，將現有的舊版直接對等互連轉換為 Azure 資源。

如果您想要的話，您可以使用 Azure [入口網站](howto-legacy-direct-portal.md)來完成本指南。

## <a name="before-you-begin"></a>開始之前
* 開始設定之前，請閱讀[必要條件](prerequisites.md)和[直接對等互連逐步解說](walkthrough-direct-all.md)。

### <a name="work-with-azure-powershell"></a>使用 Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-direct-peering-to-an-azure-resource"></a>將舊版直接對等互連轉換為 Azure 資源

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>登入您的 Azure 帳戶並且選取您的訂用帳戶
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-a-legacy-direct-peering-for-conversion"></a><a name= get></a>取得舊版直接對等互連以進行轉換
此範例顯示如何在西雅圖對等互連位置取得舊版直接對等互連。

```powershell
$legacyPeering = Get-AzLegacyPeering `
    -Kind Direct -PeeringLocation "Seattle"
$legacyPeering
```

以下是範例回應：
```powershell
Name                       :
Sku                        : Basic_Direct_Free
Kind                       : Direct
PeeringLocation            : Seattle
UseForPeeringService       : False
PeerAsn.Id                 :
Connection                 : ------------------------
PeeringDBFacilityId        : 71
SessionPrefixIPv4          : 4.71.156.72/30
PeerSessionIPv4Address     : 4.71.156.73
MicrosoftIPv4Address       : 4.71.156.74
SessionStateV4             : Established
MaxPrefixesAdvertisedV4    : 20000
SessionPrefixIPv6          : 2001:1900:2100::1e10/126
MaxPrefixesAdvertisedV6    : 2000
ConnectionState            : Active
BandwidthInMbps            : 0
ProvisionedBandwidthInMbps : 20000
Connection                 : ------------------------
PeeringDBFacilityId        : 71
SessionPrefixIPv4          : 4.68.70.140/30
PeerSessionIPv4Address     : 4.68.70.141
MicrosoftIPv4Address       : 4.68.70.142
SessionStateV4             : Established
MaxPrefixesAdvertisedV4    : 20000
SessionPrefixIPv6          : 2001:1900:4:3::cc/126
PeerSessionIPv6Address     : 2001:1900:4:3::cd
MicrosoftIPv6Address       : 2001:1900:4:3::ce
SessionStateV6             : Established
MaxPrefixesAdvertisedV6    : 2000
ConnectionState            : Active
BandwidthInMbps            : 0
ProvisionedBandwidthInMbps : 20000
ProvisioningState          : Succeeded
```

### <a name="convert-a-legacy-direct-peering"></a>轉換舊版直接對等互連

&nbsp;
> [!IMPORTANT]
> 當您將舊版對等互連轉換為 Azure 資源時，不支援修改。 &nbsp;

使用此命令將舊版直接對等互連轉換為 Azure 資源：

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleDirectPeering" `
    -ResourceGroupName "PeeringResourceGroup" `

```

以下是範例回應：

```powershell
Name                 : SeattleDirectPeering
Sku.Name             : Basic_Direct_Free
Kind                 : Direct
Connections          : {11, 11}
PeerAsn.Id           : /subscriptions/{subscriptionId}/providers/Microsoft.Peering/peerAsns/{asnNumber}
UseForPeeringService : False
PeeringLocation      : Seattle
ProvisioningState    : Succeeded
Location             : centralus
Id                   : /subscriptions/{subscriptionId}/resourceGroups/PeeringResourceGroup/providers/Microsoft.Peering/peerings/SeattleDirectPeering
Type                 : Microsoft.Peering/peerings
Tags                 : {}
```

## <a name="additional-resources"></a>其他資源
您可以藉由執行此命令來取得所有參數的詳細描述：

```powershell
Get-Help Get-AzPeering -detailed
```

如需詳細資訊，請參閱[網際網路對等互連常見問題集](faqs.md)。

## <a name="next-steps"></a>後續步驟

* [使用 PowerShell 建立或修改直接對等互連](howto-direct-powershell.md)

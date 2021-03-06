---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: 1b650fa5a0e9ba2f7019e6e67690d9d1fd65e72a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2020
ms.locfileid: "90894887"
---
資訊安全中心可為您的容器化環境提供即時威脅防護，並產生可疑活動的警示。 您可以使用這項資訊來快速修復安全性問題，並改善您容器的安全性。

資訊安全中心提供不同層級的威脅防護： 

* **適用于伺服器的 Azure defender 所提供的主機層級 () ** -使用資訊安全中心在其他 vm 上使用的相同 Log Analytics 代理程式，Azure Defender 會監視您的 Linux AKS 節點是否有可疑的活動，例如 web shell 偵測和與已知可疑 IP 位址的連接。 代理程式也會監視容器特定的分析，例如特殊許可權的容器建立、對 API 伺服器的可疑存取，以及在 Docker 容器內執行 (SSH) 伺服器的安全殼層。

    >[!IMPORTANT]
    > 如果您選擇不在主機上安裝此代理程式，則只會收到部分的威脅防護權益和安全性警示。 您仍會收到與網路分析以及與惡意伺服器通訊有關的警示。

    如需 AKS 主機層級警示的清單，請參閱 [警示的參考表](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-containerhost)。


* **Azure Defender For Kubernetes) 所提供的 AKS 叢集層級 (** -在叢集層級，威脅防護是以分析 Kubernetes 的 audit 記錄為基礎。 若要啟用此 **無代理** 程式監視，請啟用 Azure Defender。 為了在此層級產生警示，資訊安全中心會使用 AKS 所擷取的記錄來監視受 AKS 管理的服務。 此層級的事件範例包括公開 Kubernetes 儀表板、建立高特殊權限的角色，以及建立敏感性裝載。

    >[!NOTE]
    > 資訊安全中心會針對在訂用帳戶設定上啟用 Kubernetes 選項後所發生的 Azure Kubernetes Service 動作和部署產生安全性警示。 

    如需 AKS 叢集層級警示的清單，請參閱[警示的參考資料表](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-akscluster)。

此外，我們的全球安全性研究小組會持續監視威脅的態勢。 他們會在發現容器特有的警示和弱點時加以新增。

> [!TIP]
> 您可以依照[此部落格文章](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270)中的指示來模擬容器警示。
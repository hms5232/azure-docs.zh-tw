---
title: 已啟用 Azure Arc 的 Kubernetes 概觀
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: 本文提供已啟用 Azure Arc 的 Kubernetes 概觀。
keywords: Kubernetes, Arc, Azure, 容器
ms.custom: references_regions
ms.openlocfilehash: fb8a7b7c2e1e5b3de7d1ccdb4054e44825231458
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604796"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>什麼是已啟用 Azure Arc 的 Kubernetes 預覽？

您可以使用已啟用 Azure Arc 的 Kubernetes 預覽，在 Azure 內部或外部附加及設定 Kubernetes 叢集。 當 Kubernetes 叢集附加至 Azure Arc 時，其會出現在 Azure 入口網站中。 該叢集會具有 Azure Resource Manager 識別碼和受控識別。 叢集會附加至標準 Azure 訂用帳戶 (位於資源群組中)，且可以如同任何其他 Azure 資源接收標籤。 

若要將 Kubernetes 叢集連線到 Azure，叢集管理員必須部署代理程式。 這些代理程式會在名為 `azure-arc` 的 Kubernetes 命名空間中執行，且為標準 Kubernetes 部署。 代理程式會負責連線至 Azure、收集 Azure Arc 記錄和計量，以及監看設定要求。 

已啟用 Azure Arc 的 Kubernetes 支援業界標準 SSL 來保護傳輸中的資料。 此外，資料會以待用加密方式儲存在 Azure Cosmos DB 資料庫中，以確保資料機密性。
 
> [!NOTE]
> 已啟用 Azure Arc 的 Kubernetes 處於預覽狀態。 不建議將其用於生產工作負載。

## <a name="supported-kubernetes-distributions"></a>支援的 Kubernetes 散發套件

已啟用 Azure Arc 的 Kubernetes 適用於任何 Cloud Native Computing Foundation (CNCF) 認證的 Kubernetes 叢集，例如 Azure 上的 AKS 引擎、Azure Stack Hub 上的 AKS 引擎、GKE、EKS 和 VMware vSphere 叢集。

下列散發套件的 Arc 小組已測試過已啟用 Azure Arc 的 Kubernetes 功能：
* RedHat OpenShift 4.3
* Rancher RKE 1.0.8
* Canonical Charmed Kubernetes 1.18
* AKS 引擎
* Azure Stack Hub 上的 AKS Engine
* 叢集 API 提供者 Azure

## <a name="supported-scenarios"></a>支援的案例 

已啟用 Azure Arc 的 Kubernetes 支援下列案例： 

* 連線在 Azure 外部執行的 Kubernetes，以進行清查、分組和標記。

* 使用以 GitOps 為基礎的設定管理來部署應用程式並套用設定。 

* 使用適用於容器的 Azure 監視器來檢視及監視您的叢集。 

* 使用適用於 Kubernetes 的 Azure 原則來套用原則。 

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>支援區域 

目前支援下列區域中已啟用 Azure Arc 的 Kubernetes： 

* 美國東部 
* 西歐


## <a name="next-steps"></a>後續步驟

* [連線叢集](./connect-cluster.md)

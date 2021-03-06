---
title: 建立內部負載平衡器
titleSuffix: Azure Kubernetes Service
description: 了解如何建立和使用內部負載平衡器，透過 Azure Kubernetes Service (AKS) 公開您的服務。
services: container-service
ms.topic: article
ms.date: 03/04/2019
ms.openlocfilehash: ec8fd1f1b32d5bba6dc4dc756e1f95f4a74f9a96
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87285878"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>搭配 Azure Kubernetes Service (AKS) 使用內部負載平衡器

若要限制存取您在 Azure Kubernetes Service (AKS) 中的應用程式，您可以建立內部負載平衡器來使用。 內部負載平衡器讓 Kubernetes 服務僅供在與 Kubernetes 叢集相同之虛擬網路中執行的應用程式存取。 本文說明如何透過 Azure Kubernetes Service (AKS) 來建立和使用內部負載平衡器。

> [!NOTE]
> Azure Load Balancer 有兩種 SKU -「基本」和「標準」。 根據預設，您建立 AKS 叢集時，會使用「標準」SKU。  建立類型為 LoadBalancer 的服務時，您會得到與布建叢集時相同的 LB 類型。 如需詳細資訊，請參閱 [Azure 負載平衡器 SKU 比較][azure-lb-comparison]。

## <a name="before-you-begin"></a>開始之前

此文章假設您目前具有 AKS 叢集。 如果您需要 AKS 叢集，請參閱[使用 Azure CLI][aks-quickstart-cli] 或[使用 Azure 入口網站][aks-quickstart-portal]的 AKS 快速入門。

您也必須安裝並設定 Azure CLI 2.0.59 版或更新版本。 執行  `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱 [安裝 Azure CLI][install-azure-cli]。

如果您使用現有的子網或資源群組，則 AKS 叢集服務主體需要管理網路資源的許可權。 如需相關資訊，請參閱[在 Azure Kubernetes Service （AKS）中搭配您自己的 IP 位址範圍使用 kubenet 網路][use-kubenet]，或[在 Azure Kubernetes Service （AKS）中設定 Azure CNI 網路][advanced-networking]。 如果您要將負載平衡器設定為使用[不同子網中的 IP 位址][different-subnet]，請確定 AKS 叢集服務主體也具有該子網的讀取權限。

您也可以使用系統指派的受控識別來取得權限，取代服務主體。 如需詳細資訊，請參閱[使用受控識別](use-managed-identity.md)。 如需權限的詳細資訊，請參閱[將 AKS 存取權委派給其他 Azure 資源][aks-sp]。

## <a name="create-an-internal-load-balancer"></a>建立內部負載平衡器

若要建立內部負載平衡器，使用服務類型 LoadBalancer** 和 azure-load-balancer-internal** 註釋來建立名為 `internal-lb.yaml` 的服務資訊清單，如下列範例所示：

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

使用[kubectl apply][kubectl-apply]部署內部負載平衡器，並指定 YAML 資訊清單的名稱：

```console
kubectl apply -f internal-lb.yaml
```

系統會在節點資源群組中建立 Azure 負載平衡器，並聯機到與 AKS 叢集相同的虛擬網路。

當您檢視服務詳細資料時，內部負載平衡器的 IP 位址會顯示在 EXTERNAL-IP** 資料行中。 在此內容中，*外部*與負載平衡器的外部介面相關，而不是接收公用的外部 IP 位址。 IP 位址可能需要一或兩分鐘的時間，才能從變更 *\<pending\>* 為實際的內部 IP 位址，如下列範例所示：

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   2m
```

## <a name="specify-an-ip-address"></a>指定 IP 位址

如果您想要搭配內部負載平衡器使用特定 IP 位址，請將 loadBalancerIP** 屬性新增至負載平衡器 YAML 資訊清單中。 在此案例中，指定的 IP 位址必須位於與 AKS 叢集相同的子網中，且必須尚未指派給資源。 例如，您不應該使用 Kubernetes 子網指定範圍中的 IP 位址。

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: internal-app
```

當您部署並查看服務詳細資料時，[*外部 ip* ] 欄位中的 ip 位址會反映您指定的 ip 位址：

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

如需在不同子網中設定負載平衡器的詳細資訊，請參閱[指定不同的子網][different-subnet]

## <a name="use-private-networks"></a>使用私人網路

當您建立 AKS 叢集時，您可以指定進階網路設定。 這種方法可讓您將叢集部署到現有的 Azure 虛擬網路和子網路。 其中一個案例是將 AKS 叢集部署到已連線至內部部署環境的私人網路，並執行只能在內部存取的服務。 如需詳細資訊，請參閱使用[Kubenet][use-kubenet]或[Azure CNI][advanced-networking]設定您自己的虛擬網路子網。

不需變更先前步驟，即可在使用私人網路的 AKS 叢集中部署內部負載平衡器。 負載平衡器已建立於與您的 AKS 叢集相同的資源群組中，但會連線到您的私人虛擬網路和子網路，如下列範例所示：

```
$ kubectl get service internal-app

NAME           TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
internal-app   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> 您可能需要對 AKS 叢集的服務主體，授與 Azure 虛擬網路資源部署所在資源群組的「網路參與者」** 角色。 使用 [az aks show][az-aks-show] 檢視服務主體，例如 `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`。 若要建立角色指派，請使用 [az role assignment create][az-role-assignment-create] 命令。

## <a name="specify-a-different-subnet"></a>指定不同的子網路

若要指定負載平衡器的子網路，請將 *azure-load-balancer-internal-subnet* 註釋新增到您的服務。 指定的子網路必須位於與您的 AKS 叢集相同的虛擬網路中。 部署後，負載平衡器 *EXTERNAL-IP* 位址會是所指定子網路的一部分。

```yaml
apiVersion: v1
kind: Service
metadata:
  name: internal-app
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: internal-app
```

## <a name="delete-the-load-balancer"></a>刪除負載平衡器

刪除所有使用內部負載平衡器的服務後，負載平衡器本身也會遭到刪除。

您也可以如同刪除任何 Kubernetes 資源 (例如 `kubectl delete service internal-app`) 一樣，直接刪除該服務，然後這也會刪除基礎的 Azure 負載平衡器。

## <a name="next-steps"></a>後續步驟

在 [Kubernetes 服務文件][kubernetes-services]中深入了解 Kubernetes 服務。

<!-- LINKS - External -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb-comparison]: ../load-balancer/skus.md
[use-kubenet]: configure-kubenet.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[different-subnet]: #specify-a-different-subnet
---
title: '使用 Azure Kubernetes Service (AKS 中的 Azure 原則保護 pod) '
description: '瞭解如何使用 Azure Kubernetes Service (AKS 上的 Azure 原則來保護 pod) '
services: container-service
ms.topic: article
ms.date: 07/06/2020
author: jluk
ms.openlocfilehash: e1c5f32e8e5df69a9c4b1eeeda46caf9d8b51f6e
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/03/2020
ms.locfileid: "89440871"
---
# <a name="secure-pods-with-azure-policy-preview"></a>使用 Azure 原則 (預覽的安全 pod) 

若要改善 AKS 叢集的安全性，您可以控制要授與哪些函式，以及是否有任何針對公司原則執行的功能。 這項存取是透過 [適用于 AKS 的 Azure 原則附加][kubernetes-policy-reference]元件所提供的內建原則來定義。 藉由對 pod 規格的安全性層面提供額外的控制（例如根許可權），可讓您更嚴格地遵循安全性，並瞭解叢集中部署的內容。 如果 pod 不符合原則中指定的條件，Azure 原則可以不允許 pod 啟動或旗標違規。 本文說明如何使用 Azure 原則來限制 AKS 中的 pod 部署。

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="before-you-begin"></a>開始之前

此文章假設您目前具有 AKS 叢集。 如果您需要 AKS 叢集，請參閱[使用 Azure CLI][aks-quickstart-cli] 或[使用 Azure 入口網站][aks-quickstart-portal]的 AKS 快速入門。

### <a name="install-the-azure-policy-add-on-for-aks"></a>安裝適用于 AKS 的 Azure 原則附加元件

若要透過 Azure 原則保護 AKS pod 的安全，您必須在 AKS 叢集上安裝適用于 AKS 的 Azure 原則附加元件。 請遵循下列 [步驟來安裝 Azure 原則附加](../governance/policy/concepts/policy-for-kubernetes.md#install-azure-policy-add-on-for-aks)元件。

本檔假設您已在上面連結的逐步解說中部署下列各項。

* 使用註冊 `Microsoft.ContainerService` 和 `Microsoft.PolicyInsights` 資源提供者 `az provider register`
* 已 `AKS-AzurePolicyAutoApprove` 使用註冊預覽功能旗標 `az feature register`
* Azure CLI 使用擴充功能 `aks-preview` 版本0.4.53 或更高版本安裝
* 在支援的1.15 或更高版本上隨 Azure 原則附加元件一起安裝的 AKS 叢集

## <a name="overview-of-securing-pods-with-azure-policy-for-aks-preview"></a>概述如何使用 Azure 原則 AKS (Preview) 保護 pod

>[!NOTE]
> 本檔詳細說明如何使用 Azure 原則來保護 pod，這是 [預覽中 Kubernetes pod 安全性原則功能](use-pod-security-policies.md)的後續版本。
> **Pod 安全性原則 (preview) 和適用于 AKS 的 Azure 原則附加元件無法同時啟用。**
> 
> 如果在啟用 pod 安全性原則的叢集中安裝 Azure 原則附加元件，請 [遵循下列步驟來停用 pod 安全性原則](use-pod-security-policies.md#enable-pod-security-policy-on-an-aks-cluster)。

在 AKS 叢集中，當建立和更新資源時，會使用「許可控制器」來攔截對 API 伺服器的要求。 然後，「許可控制器」可以根據一組規則來 *驗證* 資源要求是否應建立。

先前，已透過 Kubernetes 專案啟用功能 [pod 安全性原則 (preview) ](use-pod-security-policies.md) ，以限制可部署的 pod。

藉由使用 Azure 原則附加元件，AKS 叢集可以使用內建的 Azure 原則來保護 pod 和其他 Kubernetes 資源（類似于先前的 pod 安全性原則）。 適用于 AKS 的 Azure 原則附加元件會安裝 [閘道管理員](https://github.com/open-policy-agent/gatekeeper)的受管理實例（驗證的許可控制器）。 Kubernetes 的 Azure 原則是以開放原始碼開啟原則代理程式為基礎，而此代理程式依賴 [Rego 原則語言](../governance/policy/concepts/policy-for-kubernetes.md#policy-language)。

本檔詳細說明如何使用 Azure 原則來保護 AKS 叢集中的 pod，以及指示如何從 pod 安全性原則 (preview) 進行遷移。

## <a name="limitations"></a>限制

* 在預覽期間，可在單一叢集中執行具有 20 Azure 原則 Kubernetes 原則的 200 pod 限制。
* 某些包含 AKS 受控 pod 的[系統命名空間](#namespace-exclusion)會排除在原則評估之外。
* Windows pod [不支援安全性](https://kubernetes.io/docs/concepts/security/pod-security-standards/#what-profiles-should-i-apply-to-my-windows-pods)內容，因此許多 Azure 原則僅適用于 Linux pod，例如不允許在 Windows pod 中提升許可權的根許可權。
* Pod 安全性原則和 AKS 的 Azure 原則附加元件無法同時啟用。 如果在啟用 pod 安全性原則的叢集中安裝 Azure 原則附加元件，請使用 [下列指示](use-pod-security-policies.md#enable-pod-security-policy-on-an-aks-cluster)停用 pod 安全性原則。

## <a name="azure-policies-to-secure-kubernetes-pods"></a>保護 Kubernetes pod 的 Azure 原則

安裝 Azure 原則附加元件之後，預設不會套用任何原則。

有11個 (11) 內建個別的 Azure 原則，以及兩個 (2) 內建的方案，專門保護 AKS 叢集中的 pod。
每個原則都可以自訂效果。 [這裡會列出 AKS 原則及其支援效果][policy-samples]的完整清單。 深入瞭解 [Azure 原則效果](../governance/policy/concepts/effects.md)。

您可以在管理群組、訂用帳戶或資源群組層級套用 Azure 原則。 在資源群組層級指派原則時，請確定已在原則範圍內選取目標 AKS 叢集的資源群組。 已安裝 Azure 原則附加元件的已指派範圍中的每個叢集都位於原則的範圍內。

如果您 [ (預覽) 使用 pod 安全性原則 ](use-pod-security-policies.md)，請瞭解如何 [遷移至 Azure 原則以及其他行為差異](#migrate-from-kubernetes-pod-security-policy-to-azure-policy)。

### <a name="built-in-policy-initiatives"></a>內建原則方案

Azure 原則中的計畫是一組原則定義的集合，專為達成單一的整體目標而量身打造。 使用方案可以簡化跨 AKS 叢集的原則管理和指派。 方案以單一物件的形式存在，深入瞭解 [Azure 原則計畫](../governance/policy/overview.md#initiative-definition)。

適用于 Kubernetes 的 Azure 原則提供兩個可保護 pod、 [基準](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) 和 [限制](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00)的內建方案。

這兩個內建的方案都是從 Kubernetes 中的 [pod 安全性原則](https://github.com/kubernetes/website/blob/master/content/en/examples/policy/baseline-psp.yaml)所使用的定義所建立。

|[Pod 安全性原則控制項](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#what-is-a-pod-security-policy)| Azure 原則定義連結| [基準計畫](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) | [限制的方案](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00) |
|---|---|---|---|
|不允許執行具特殊許可權的容器|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F95edb821-ddaf-4404-9732-666045e056b4)| 是 | 是
|不允許主機命名空間的共用使用方式|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F47a1ee2f-2a2a-4576-bf2a-e0e36709c2b8)| 是 | 是
|限制主機網路和埠的所有使用|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82985f06-dc18-4a48-bc1c-b9f4f0098cfe)| 是 | 是
|限制主機檔案系統的任何使用|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F098fc59e-46c7-4d99-9b16-64990e543d75)| 是 | 是
|限制[預設設定](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities)的 Linux 功能|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc26596ff-4d70-4e6a-9a30-c2506bd2f80c) | 是 | 是
|限制使用已定義的磁片區類型|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F16697877-1118-4fb1-9b65-9898ec2509ec)| - | 是-允許的磁片區類型為 `configMap` 、 `emptyDir` 、 `projected` 、 `downwardAPI` 、 `persistentVolumeClaim`|
|許可權提升至根目錄|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1c6e92c9-99f0-4e55-9cf2-0c234dc48f99) | - | 是 |
|限制容器的使用者和群組識別碼|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff06ddb64-5fa3-4b77-b166-acb36f7f6042) | - | 是|
|限制配置擁有 pod 磁片區的 FSGroup|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff06ddb64-5fa3-4b77-b166-acb36f7f6042) | - | 是-允許的規則為 `runAsUser: mustRunAsNonRoot` 、 `supplementalGroup: mustRunAs 1:65536` 、 `fsGroup: mustRunAs 1:65535` 、 `runAsGroup: mustRunAs 1:65535` 。  |
|需要 seccomp 設定檔|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F975ce327-682c-4f2e-aa46-b9598289b86c) | - | 是，allowedProfiles 是 * `docker/default` 或 `runtime/default` |

\* docker/default 在 Kubernetes 中已被取代，自 v 1.11 起

### <a name="additional-optional-policies"></a>其他選用原則

有其他 Azure 原則可在套用方案之外單獨套用。 如果內建計畫不符合您的需求，請考慮將這些原則附加至方案之外。

|[Pod 安全性原則控制項](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#what-is-a-pod-security-policy)| Azure 原則定義連結| 除了基準計畫之外還適用 | 除了限制的方案之外還適用 |
|---|---|---|---|
|定義容器所使用的 AppArmor 設定檔|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F511f5417-5d12-434d-ab2e-816901e72a5e) | 選用 | 選用 |
|允許非唯讀的裝載|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdf49d893-a74c-421d-bc95-c663042e5b80) | 選用 | 選用 |
|限制特定的 FlexVolume 驅動程式|[公用雲端](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4a8fce0-2dd5-4c21-9a36-8f0ec809d663) | 選用-如果您只想要限制 FlexVolume 驅動程式，而不是由「限制使用定義的磁片區類型」設定，請使用此選項。 | 不適用-受限制的方案包括「限制使用定義的磁片區類型」，這不允許所有 FlexVolume 驅動程式 |

### <a name="unsupported-built-in-policies-for-managed-aks-clusters"></a>受控 AKS 叢集不支援的內建原則

> [!NOTE]
> **AKS 中不支援**下列3個原則，原因是自訂 AKS 作為受控服務來管理和保護的層面。 這些原則專為具有未受管理控制平面的 Azure Arc 連線叢集所建立。

|[Pod 安全性原則控制項](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#what-is-a-pod-security-policy)|
|---|
|定義容器的自訂 SELinux 內容|
|限制容器所使用的 sysctl 設定檔|
|系統會定義預設的 Proc 掛接類型，以減少受攻擊面|

<!---
# Removing until custom initiatives are supported the week after preview

#### Custom initiative

If the built-in initiatives to address pod security do not match your requirements, you can choose your own policies to exist in a custom initiative. Read more about [building custom initatives in Azure Policy](../governance/policy/tutorials/create-and-manage#create-and-assign-an-initiative-definition).

--->

### <a name="namespace-exclusion"></a>命名空間排除

> [!WARNING]
> 系統管理員命名空間（例如 kube 系統）中的 pod 必須執行，叢集才能維持狀況良好，從預設排除的命名空間清單中移除必要的命名空間，可能會因為必要的系統 pod 而觸發原則違規。

AKS 需要在叢集上執行系統 pod，以提供重要的服務，例如 DNS 解析。 限制 pod 功能的原則可能會影響系統 pod 穩定性的能力。 因此，在 **建立、更新和原則審核期間**，會將下列命名空間排除在許可要求期間的原則評估之外。 這會強制將這些命名空間的新部署從 Azure 原則中排除。

1. kube-系統
1. 閘道管理員-系統
1. azure-arc
1. aks-periscope

您可以在建立、更新和審核期間排除其他自訂命名空間的評估。 如果您有在獲批准命名空間中執行的特製化 pod，而且想要避免觸發 audit 違規，則應使用此功能。

## <a name="apply-the-baseline-initiative"></a>套用基準計畫

> [!TIP]
> 所有原則都會預設為 audit 效果。 您可以隨時透過 Azure 原則將效果更新為拒絕。

若要套用基準計畫，我們可以透過 Azure 入口網站指派。
<!--
1. Navigate to the Policy service in Azure portal
1. In the left pane of the Azure Policy page, select **Definitions**
1. Search for "Baseline Profile" on the search pane to the right of the page
1. Select `Kubernetes Pod Security Standards Baseline Profile for Linux-based workloads` from the `Kubernetes` category
-->
1. 遵循 [Azure 入口網站的連結](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) ，以查看 pod 安全性基準計畫
1. 將 **範圍** 設定為訂用帳戶層級，或只將保存 AKS 叢集的資源群組設定為已啟用 Azure 原則附加元件
1. 選取 [ **參數** ] 頁面，並將 **效果** 從更新為， `audit` `deny` 以封鎖違反基準計畫的新部署
1. 新增其他命名空間，以在建立、更新和審核期間從評估中排除，[某些命名空間會強制排除在原則評估之外。](#namespace-exclusion) 
 ![更新效果](media/use-pod-security-on-azure-policy/update-effect.png)
1. 選取 [ **審核 + 建立** ] 以提交原則

藉由執行，確認原則會套用至您的叢集 `kubectl get constrainttemplates` 。

> [!NOTE]
> 原則 [最多可能需要20分鐘的時間，才會同步](../governance/policy/concepts/policy-for-kubernetes.md#assign-a-built-in-policy-definition) 至每個叢集。

輸出應該會類似：

```console
$ kubectl get constrainttemplate
NAME                                     AGE
k8sazureallowedcapabilities              30m
k8sazureblockhostnamespace               30m
k8sazurecontainernoprivilege             30m
k8sazurehostfilesystem                   30m
k8sazurehostnetworkingports              30m
```

## <a name="validate-rejection-of-a-privileged-pod"></a>驗證具有特殊許可權的 pod 拒絕

讓我們先測試當您排程具有安全性內容的 pod 時，會發生什麼事 `privileged: true` 。 此安全性內容會升級 pod 的許可權。 基準計畫不允許特殊許可權的 pod，因此將會拒絕要求，導致部署遭到拒絕。

建立名為的檔案 `nginx-privileged.yaml` ，並貼上下列 YAML 資訊清單：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: nginx
      securityContext:
        privileged: true
```

使用 [kubectl apply][kubectl-apply] 命令建立 pod，並指定 YAML 資訊清單的名稱：

```console
kubectl apply -f nginx-privileged.yaml
```

如預期，pod 無法排程，如下列範例輸出所示：

```console
$ kubectl apply -f privileged.yaml

Error from server ([denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}): error when creating "privileged.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}
```

Pod 不會到達排程階段，因此在您繼續之前，不需要刪除任何資源。

## <a name="test-creation-of-an-unprivileged-pod"></a>無特殊許可權 pod 的測試建立

在上述範例中，容器映射會自動嘗試使用 root 將 NGINX 系結到埠80。 基準原則方案已拒絕此要求，所以 pod 無法啟動。 現在，讓我們試著在不具特殊許可權的情況下，執行相同的 NGINX pod。

建立名為的檔案 `nginx-unprivileged.yaml` ，並貼上下列 YAML 資訊清單：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx
```

使用 [kubectl apply][kubectl-apply] 命令建立 pod，並指定 YAML 資訊清單的名稱：

```console
kubectl apply -f nginx-unprivileged.yaml
```

Pod 已成功排程。 當您使用[kubectl get][kubectl-get] pod 命令檢查 pod 的*狀態時，pod 正在執行：*

```console
$ kubectl get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          18s
```

此範例顯示的基準計畫只會影響集合中違反原則的部署。 允許的部署會繼續運作。

使用 [kubectl delete][kubectl-delete] 命令刪除 NGINX 無特殊許可權的 pod，並指定 YAML 資訊清單的名稱：

```console
kubectl delete -f nginx-unprivileged.yaml
```

## <a name="disable-policies-and-the-azure-policy-add-on"></a>停用原則和 Azure 原則附加元件

若要移除基準計畫：

1. 流覽至 Azure 入口網站上的 [原則] 窗格
1. 從左窗格選取**指派**
1. 按一下 [...]按鈕（位於基準設定檔旁）
1. 選取 [刪除指派]

![刪除指派](media/use-pod-security-on-azure-policy/delete-assignment.png)

若要停用 Azure 原則附加元件，請使用 [az aks disable-附加元件][az-aks-disable-addons] 命令。

```azurecli
az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
```

瞭解如何 [從 Azure 入口網站移除 Azure 原則附加](../governance/policy/concepts/policy-for-kubernetes.md#remove-the-add-on-from-aks)元件。

## <a name="migrate-from-kubernetes-pod-security-policy-to-azure-policy"></a>從 Kubernetes pod 安全性原則遷移至 Azure 原則

若要從 pod 安全性原則進行遷移，您必須在叢集上採取下列動作。

1. 停用叢集上的[pod 安全性原則](use-pod-security-policies.md#clean-up-resources)
1. 啟用[Azure 原則附加][kubernetes-policy-reference]元件
1. 從[可用的內建原則][policy-samples]啟用所需的 Azure 原則

以下摘要說明 pod 安全性原則和 Azure 原則之間的行為變更。

|案例| Pod 安全性原則 | Azure 原則 |
|---|---|---|
|安裝|啟用 pod 安全性原則功能 |啟用 Azure 原則附加元件
|部署原則| 部署 pod 安全性原則資源| 將 Azure 原則指派給訂用帳戶或資源群組範圍。 Kubernetes 資源應用程式需要 Azure 原則附加元件。
| 預設原則 | 在 AKS 中啟用 pod 安全性原則時，會套用預設的特殊許可權和不受限制的原則。 | 啟用 Azure 原則附加元件，不會套用任何預設原則。 您必須在 Azure 原則中明確啟用原則。
| 誰可以建立和指派原則 | 叢集系統管理員會建立 pod 安全性原則資源 | 使用者必須擁有 AKS 叢集資源群組的「擁有者」或「資源原則參與者」許可權的最小角色。 -透過 API，使用者可以在 AKS 叢集資源範圍指派原則。 使用者應具備 AKS 叢集資源的最少「擁有者」或「資源原則參與者」許可權。 -在 Azure 入口網站中，您可以在管理群組/訂用帳戶/資源群組層級指派原則。
| 授權原則| 使用者和服務帳戶需要明確的許可權，才能使用 pod 安全性原則。 | 授權原則不需要額外的指派。 一旦在 Azure 中指派原則之後，所有叢集使用者都可以使用這些原則。
| 原則適用性 | 系統管理員使用者略過 pod 安全性原則的強制執行。 |  (系統管理員 & 非系統管理員) 的所有使用者都會看到相同的原則。 沒有以使用者為基礎的特殊大小寫。 原則應用程式可以在命名空間層級中排除。
| 原則範圍 | Pod 安全性原則未命名空間 | 未命名空間 Azure 原則所使用的條件約束範本。
| 拒絕/審核/變化動作 | Pod 安全性原則僅支援拒絕動作。 您可以使用 create 要求的預設值來進行變化。 驗證可以在更新要求期間完成。| Azure 原則支援 audit & deny 動作。 尚未支援變化，但已計畫。
| Pod 安全性原則合規性 | 在啟用 pod 安全性原則之前，不會看到已存在的 pod 合規性。 在啟用 pod 安全性原則之後建立的不符合規範 pod 將會遭到拒絕。 | 套用 Azure 原則之前存在的不符合規範 pod 會在違反原則的情況下顯示。 如果設定了拒絕效果的原則，則在啟用 Azure 原則之後所建立的不符合規範 pod 將會遭到拒絕。
| 如何查看叢集中的原則 | `kubectl get psp` | `kubectl get constrainttemplate` -傳回所有原則。
| Pod 安全性原則標準-特殊許可權 | 啟用此功能時，預設會建立特殊許可權的 pod 安全性原則資源。 | 特殊許可權模式暗示沒有限制，因此它等同于沒有任何 Azure 原則指派。
| [Pod 安全性原則標準-基準/預設值](https://kubernetes.io/docs/concepts/security/pod-security-standards/#baseline-default) | 使用者安裝 pod 安全性原則基準資源。 | Azure 原則提供可對應至基準 pod 安全性原則的 [內建基準方案](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) 。
| [Pod 安全性原則標準-受限制](https://kubernetes.io/docs/concepts/security/pod-security-standards/#restricted) | 使用者安裝 pod 安全性原則限制的資源。 | Azure 原則提供 [內建的限制方案](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00) ，可對應至受限制的 pod 安全性原則。

## <a name="next-steps"></a>接下來的步驟

本文說明如何套用 Azure 原則，以限制特殊許可權 pod 的部署，以避免使用特殊許可權存取。 有許多可以套用的原則，例如限制磁片區使用的原則。 如需可用選項的詳細資訊，請參閱 [Kubernetes 參考][kubernetes-policy-reference]檔的 Azure 原則。

如需限制 pod 網路流量的詳細資訊，請參閱 [在 AKS 中使用網路原則來保護 pod 之間的流量][network-policies]。

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[kubernetes-policy-reference]: ../governance/policy/concepts/policy-for-kubernetes.md
[policy-samples]: policy-samples.md#microsoftcontainerservice
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policies]: use-network-policies.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-disable-addons]: /cli/azure/aks#az-aks-disable-addons
[az-policy-assignment-delete]: /cli/azure/policy/assignment#az-policy-assignment-delete

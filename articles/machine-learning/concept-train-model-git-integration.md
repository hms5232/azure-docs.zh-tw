---
title: Azure Machine Learning 的 Git 整合
titleSuffix: Azure Machine Learning
description: 瞭解 Azure Machine Learning 如何與本機 Git 存放庫整合。 從 Git 存放庫的本機目錄提交定型回合時，會在執行過程中追蹤存放庫、分支和目前認可的相關資訊。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.openlocfilehash: bd77af133b88e1ba93054dbb7e0f896d8d418f89
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2020
ms.locfileid: "90893551"
---
# <a name="git-integration-for-azure-machine-learning"></a>Azure Machine Learning 的 Git 整合

[Git](https://git-scm.com/) 是一種熱門的版本控制系統，可讓您在專案上共用及共同作業。 

Azure Machine Learning 完全支援用於追蹤工作的 Git 存放庫-您可以直接將儲存機制複製到共用工作區檔案系統、在本機工作站上使用 Git，或從 CI/CD 管線使用 Git。

將作業提交至 Azure Machine Learning 時，如果原始程式檔儲存在本機 git 存放庫中，則會在定型過程中追蹤存放庫的相關資訊。

由於 Azure Machine Learning 會從本機 git 存放庫追蹤資訊，因此不會系結至任何特定的中央存放庫。 您可以從 GitHub、GitLab、Bitbucket、Azure DevOps 或任何其他 git 相容的服務複製您的存放庫。

## <a name="clone-git-repositories-into-your-workspace-file-system"></a>將 Git 存放庫複製到您的工作區檔案系統
Azure Machine Learning 為工作區中的所有使用者提供共用檔案系統。
若要將 Git 儲存機制複製到此檔案共用，建議您建立計算實例 & 開啟終端機。
一旦開啟終端機，您就可以存取完整的 Git 用戶端，並可透過 Git CLI 體驗來複製和使用 Git。

建議您將儲存機制複製到您的使用者目錄，讓其他人不會直接在您的工作分支上進行衝突。

您可以將任何可驗證的 Git 存放庫複製到 (GitHub、Azure Repos、BitBucket 等 ) 

如需如何使用 Git CLI 的指南，請參閱[這裡。](https://guides.github.com/introduction/git-handbook/)

## <a name="track-code-that-comes-from-git-repositories"></a>追蹤來自 Git 存放庫的程式碼

當您從 Python SDK 或 Machine Learning CLI 提交定型回合時，定型模型所需的檔案會上傳至您的工作區。 如果 `git` 您的開發環境中有可用的命令，上傳程式會使用它來檢查檔案是否儲存在 git 存放庫中。 如果是，則您的 git 存放庫的資訊也會隨著定型回合的一部分上傳。 這項資訊會儲存在定型執行的下列屬性中：

| 屬性 | 用來取得值的 Git 命令 | 描述 |
| ----- | ----- | ----- |
| `azureml.git.repository_uri` | `git ls-remote --get-url` | 您的存放庫複製來源的 URI。 |
| `mlflow.source.git.repoURL` | `git ls-remote --get-url` | 您的存放庫複製來源的 URI。 |
| `azureml.git.branch` | `git symbolic-ref --short HEAD` | 提交執行時的作用中分支。 |
| `mlflow.source.git.branch` | `git symbolic-ref --short HEAD` | 提交執行時的作用中分支。 |
| `azureml.git.commit` | `git rev-parse HEAD` | 針對執行所提交之程式碼的認可雜湊。 |
| `mlflow.source.git.commit` | `git rev-parse HEAD` | 針對執行所提交之程式碼的認可雜湊。 |
| `azureml.git.dirty` | `git status --porcelain .` | `True`如果分支/認可已中途變更，否則為 `false` 。 |

這項資訊會傳送給使用估算器、機器學習服務管線或腳本執行的執行。

如果您的定型檔案不在您開發環境的 git 存放庫中，或 `git` 命令無法使用，則不會追蹤任何與 git 相關的資訊。

> [!TIP]
> 若要檢查您的開發環境中是否有可用的 git 命令，請開啟 shell 會話、命令提示字元、PowerShell 或其他命令列介面，然後輸入下列命令：
>
> ```
> git --version
> ```
>
> 如果已安裝，且在路徑中，您會收到類似的回應 `git version 2.4.1` 。 如需在開發環境上安裝 git 的詳細資訊，請參閱 [git 網站](https://git-scm.com/)。

## <a name="view-the-logged-information"></a>查看記錄的資訊

Git 資訊會儲存在定型回合的屬性中。 您可以使用 Azure 入口網站、Python SDK 和 CLI 來查看此資訊。 

### <a name="azure-portal"></a>Azure 入口網站

1. 從 [studio 入口網站](https://ml.azure.com)中，選取您的工作區。
1. 選取 [ __實驗__]，然後選取您的其中一個實驗。
1. 從 [ __執行編號__ ] 資料行中選取其中一個執行。
1. 選取 [ __輸出 + 記錄__]，然後展開 __記錄__ 和 __azureml__ 專案。 選取以__ ### \_ azure__開頭的連結。

記錄的資訊包含類似下列 JSON 的文字：

```json
"properties": {
    "_azureml.ComputeTargetType": "batchai",
    "ContentSnapshotId": "5ca66406-cbac-4d7d-bc95-f5a51dd3e57e",
    "azureml.git.repository_uri": "git@github.com:azure/machinelearningnotebooks",
    "mlflow.source.git.repoURL": "git@github.com:azure/machinelearningnotebooks",
    "azureml.git.branch": "master",
    "mlflow.source.git.branch": "master",
    "azureml.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "mlflow.source.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "azureml.git.dirty": "True",
    "AzureML.DerivedImageName": "azureml/azureml_9d3568242c6bfef9631879915768deaf",
    "ProcessInfoFile": "azureml-logs/process_info.json",
    "ProcessStatusFile": "azureml-logs/process_status.json"
}
```

### <a name="python-sdk"></a>Python SDK

提交定型回合之後，會傳回 [執行](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true) 物件。 `properties`此物件的屬性包含已記錄的 git 資訊。 例如，下列程式碼會捕獲認可雜湊：

```python
run.properties['azureml.git.commit']
```

### <a name="cli"></a>CLI

`az ml run`CLI 命令可用於從執行中取出屬性。 例如，下列命令會傳回名為的實驗中，最後一個執行的屬性 `train-on-amlcompute` ：

```azurecli-interactive
az ml run list -e train-on-amlcompute --last 1 -w myworkspace -g myresourcegroup --query '[].properties'
```

如需詳細資訊，請參閱 [az ml 執行](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest) 參考檔。

## <a name="next-steps"></a>下一步

* [使用計算目標來將模型定型](how-to-set-up-training-targets.md)

---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: deb2eb877743d533c5daeee8b6636edd62418fe0
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2020
ms.locfileid: "89494318"
---
若要驗證雲端式應用程式，最簡單的方法是使用受控識別；如需詳細資訊，請參閱[向 Key Vault 進行驗證](/azure/key-vault/general/authentication)。

但為了簡單起見，本快速入門會建立桌面應用程式，這需要使用服務主體和存取控制原則。 您的服務主體需要以下格式的唯一名稱："http://&lt;my-unique-service-principal-name&gt;"。

請使用 Azure CLI [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 命令來建立服務主體：

```azurecli
az ad sp create-for-rbac --skip-assignment -n "http://<my-unique-service-principal-name>" --sdk-auth
```

這項作業會傳回一系列的金鑰/值組。

```console
{
  "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
  "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
  "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
  "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

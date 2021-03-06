---
title: 快速入門 – Azure Key Vault Python 用戶端程式庫 – 管理憑證
description: 了解如何使用 Python 用戶端程式庫，從 Azure Key Vault 建立、擷取和刪除憑證
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: b9ff7397ad29ac681e21c32608ade9c6ce557c37
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2020
ms.locfileid: "89488621"
---
# <a name="quickstart-azure-key-vault-certificates-client-library-for-python"></a>快速入門：適用於 Python 的 Azure Key Vault 憑證用戶端程式庫

開始使用適用於 Python 的 Azure Key Vault 用戶端程式庫。 請遵循下列步驟來安裝套件，並試用基本工作的程式碼範例。 藉由使用 Key Vault 來儲存憑證，您可以避免將憑證儲存在您的程式碼中，如此會增加應用程式的安全性。

[API 參考文件](/python/api/overview/azure/keyvault-certificates-readme?view=azure-python) | [程式庫原始程式碼](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-certificates) | [套件 (Python Package Index)](https://pypi.org/project/azure-keyvault-certificates)

## <a name="set-up-your-local-environment"></a>設定您的本機環境

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. 安裝 Key Vault 憑證程式庫：

    ```terminal
    pip install azure-keyvault-certificates
    ```

## <a name="create-a-resource-group-and-key-vault"></a>建立資源群組和金鑰保存庫

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>對服務主體授與金鑰保存庫的存取權

執行下列 [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) 命令，以授權您的服務主體在憑證上進行取得、列出及建立作業。

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --certificate-permissions delete get list create
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --certificate-permissions delete get list create 
```

---

此命令依賴在先前步驟中建立的 `KEY_VAULT_NAME` 和 `AZURE_CLIENT_ID` 環境變數。

如需詳細資訊，請參閱[指派存取原則 - CLI](../general/assign-access-policy-cli.md)

## <a name="create-the-sample-code"></a>建立範例程式碼

適用於 Python 的 Azure Key Vault 用戶端程式庫可讓您管理憑證和相關資產，例如祕密和密碼編譯金鑰。 下列程式碼範例會示範如何建立用戶端、設定祕密、擷取祕密，以及刪除秘密。

建立名為 kv_certificates.py 的檔案，其中包含此程式碼。

```python
import os
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)

certificateName = input("Input a name for your certificate > ")

print(f"Creating a certificate in {keyVaultName} called '{certificateName}' ...")

policy = CertificatePolicy.get_default()
poller = client.begin_create_certificate(certificate_name=certificateName, policy=policy)
certificate = poller.result()

print(" done.")

print(f"Retrieving your certificate from {keyVaultName}.")

retrieved_certificate = client.get_certificate(certificateName)

print(f"Certificate with name '{retrieved_certificate.name}' was found'.")
print(f"Deleting your certificate from {keyVaultName} ...")

poller = client.begin_delete_certificate(certificateName)
deleted_certificate = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>執行程式碼

請確定上一節中的程式碼位於名為 kv_certificates.py 的檔案中。 使用下列命令來執行程式碼：

```terminal
python kv_certificates.py
```

- 如果您遇到權限錯誤，請確定您已執行 [`az keyvault set-policy` 命令](#give-the-service-principal-access-to-your-key-vault)。
- 以相同的金鑰名稱重新執行程式碼可能會產生錯誤「(衝突) 憑證 <name> 目前處於已刪除但可復原的狀態。」 使用不同的金鑰名稱。

## <a name="code-details"></a>程式碼詳細資料

### <a name="authenticate-and-create-a-client"></a>驗證並建立用戶端

在上述程式碼中，[`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) 物件會使用您為服務主體建立的環境變數。 每當您從 Azure 程式庫建立用戶端物件 (例如 [`CertificateClient`](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python))，以及要透過該用戶端使用資源的 URI 時，就會提供此認證：

```python
credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-certificate"></a>儲存憑證

取得金鑰保存庫的用戶端物件之後，您可以使用 [begin_create_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#begin-create-certificate-certificate-name--policy----kwargs-) 方法來建立憑證： 

```python
policy = CertificatePolicy.get_default()
poller = client.begin_create_certificate(certificate_name=certificateName, policy=policy)
certificate = poller.result()
```

在這裡，憑證需要使用 [CertificatePolicy.get_default](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificatepolicy?view=azure-python#get-default--) 方法取得原則。

呼叫 `begin_create_certificate` 方法會針對金鑰保存庫產生 Azure REST API 的非同步呼叫。 非同步呼叫會傳回輪詢者物件。 若要等候作業的結果，請呼叫輪詢者的 `result` 方法。

處理要求時，Azure 會使用您提供給用戶端的認證物件，來驗證呼叫者的身分識別 (服務主體)，

同時也會檢查呼叫者是否有權執行要求的動作。 您先前已使用 [`az keyvault set-policy` 命令](#give-the-service-principal-access-to-your-key-vault)，將此授權授與服務主體。

### <a name="retrieve-a-certificate"></a>擷取憑證

若要從 Key Vault 讀取憑證，請使用 [get_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#get-certificate-certificate-name----kwargs-) 方法：

```python
retrieved_certificate = client.get_certificate(certificateName)
 ```

您也可以使用 Azure CLI 命令 [az keyvault certificate show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) 來確認憑證是否已完成設定。

### <a name="delete-a-certificate"></a>刪除憑證

若要刪除憑證，請使用 [begin_delete_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#begin-delete-certificate-certificate-name----kwargs-) 方法：

```python
poller = client.begin_delete_certificate(certificateName)
deleted_certificate = poller.result()
```

`begin_delete_certificate` 方法是非同步，並會傳回輪詢者物件。 呼叫輪詢器的 `result` 方法會等待其完成。

您可以使用 Azure CLI 命令 [az keyvault certificate show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) 來確認是否已刪除憑證。

一旦刪除，憑證會保留為已刪除但可復原的狀態一段時間。 如果再次執行程式碼，請使用不同的憑證名稱。

## <a name="clean-up-resources"></a>清除資源

如果您想要實驗[祕密](../secrets/quick-create-python.md)和[金鑰](../keys/quick-create-python.md)，可以重複使用在本文中建立的 Key Vault。

否則，當您完成本文中建立的資源時，請使用下列命令來刪除資源群組及其包含的所有資源：

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>後續步驟

- [Azure 金鑰保存庫概觀](../general/overview.md)
- [Azure Key Vault 開發人員指南](../general/developers-guide.md)
- [Azure Key Vault 最佳做法](../general/best-practices.md)
- [使用 Key Vault 進行驗證](../general/authentication.md)

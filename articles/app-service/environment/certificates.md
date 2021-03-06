---
title: 憑證系結
description: 說明與 App Service 環境上憑證相關的多個主題。 瞭解憑證系結如何在 ASE 中的單一租使用者應用程式上運作。
author: ccompy
ms.assetid: 9e21a7e4-2436-4e81-bb05-4a6ba70eeaf7
ms.topic: article
ms.date: 08/29/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 306445e26e5b236b49273b9ab8888ecc610bc075
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962038"
---
# <a name="certificates-and-the-app-service-environment"></a>憑證和 App Service Environment 

App Service Environment (ASE) 是在 Azure 虛擬網路 (VNet) 內執行之 Azure App Service 的部署。 它可以使用網際網路可存取的應用程式端點，或是使用您 VNet 中的應用程式端點來部署。 如果您使用網際網路可存取的端點來部署 ASE，則該部署稱為外部 ASE。 如果您使用 VNet 中的端點來部署 ASE，則該部署稱為 ILB ASE。 若要深入了解 ILB ASE，請參閱[建立和使用 ILB ASE](./create-ilb-ase.md) 文件。

ASE 是單一租用戶系統。 因為它是單一租用戶，所以有一些功能僅提供於 ASE，而未提供於多租用戶的 App Service 中。 

## <a name="ilb-ase-certificates"></a>ILB ASE 憑證 

如果您使用外部 ASE，則在 [appname].[asename].p.azurewebsites.net 可存取您的應用程式。 根據預設會使用遵循該格式的憑證來建立所有 ASE，甚至是 ILB ASE。 當您有 ILB ASE 時，可根據建立 ILB ASE 時您指定的網域名稱來存取應用程式。 為了讓應用程式支援 TLS，您需要上傳憑證。 使用內部憑證授權單位單位、向外部簽發者購買憑證，或使用自我簽署憑證，以取得有效的 TLS/SSL 憑證。 

有兩個選項可設定您的 ILB ASE 的憑證。  您可以設定 ILB ASE 的萬用字元預設憑證，或在 ASE 中的個別 Web 應用程式上設定憑證。  無論您的選擇為何，都需要正確設定下列憑證屬性︰

- **Subject**︰針對萬用字元 ILB ASE 憑證，這個屬性必須設為 *.[your-root-domain-here]。 如果在建立應用程式的憑證，它應該是 [appname].[your-root-domain-here]
- **Subject Alternative Name**：針對萬用字元 ILB ASE 憑證，這個屬性必須包含 *.[your-root-domain-here] 和 *.scm.[your-root-domain-here]。 如果在建立應用程式的憑證，它應該是 [appname].[your-root-domain-here] 和 [appname].scm.[your-root-domain-here]。

第三種變化，您可以建立 ILB ASE 憑證，其中包含憑證 SAN 中的所有個別應用程式名稱，而不是使用萬用字元參考。 這個方法的問題在於，您必須事先知道您要放在 ASE 中的應用程式名稱，或者您需要持續更新 ILB ASE 憑證。

### <a name="upload-certificate-to-ilb-ase"></a>上傳憑證到 ILB ASE 

在入口網站中建立 ILB ASE 之後，必須設定 ILB ASE 的憑證。 在設定憑證之前，ASE 會顯示未設定憑證的橫幅。  

您上傳的憑證必須是 .pfx 檔案。 上傳憑證之後，ASE 會執行調整作業以設定憑證。 

您無法在入口網站中以一個動作建立 ASE 並上傳憑證，或甚至是在一個範本中進行。 作為個別的動作，您可以使用範本上傳憑證，如[從範本建立 ASE](./create-from-template.md) 文件中所述。  

如果您想要快速建立自我簽署的憑證以進行測試，可以使用 PowerShell 的下列位元：

```azurepowershell-interactive
$certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

$certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
$password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

$fileName = "exportedcert.pfx"
Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password
```

建立自我簽署憑證時，您必須確定主體名稱的格式為 CN = {ASE_NAME_HERE} _InternalLoadBalancingASE。

## <a name="application-certificates"></a>應用程式憑證 

在 ASE 中裝載的應用程式可以使用以應用程式為中心的憑證功能，這些功能提供於多租用戶的 App Service 中。 這些功能包括：  

- SNI 憑證 
- 以 IP 為主的 SSL，這僅適用於外部 ASE。  ILB ASE 不支援以 IP 為主的 SSL。
- KeyVault 裝載的憑證 

您可以在 [Azure App Service 中新增 TLS/SSL 憑證](../configure-ssl-certificate.md)，以取得上傳及管理這些憑證的指示。  如果您只要設定憑證，以符合您已指派給 Web 應用程式的自訂網域名稱，這些指示便已足夠。 如果您要上傳具有預設網域名稱的 ILB ASE Web 應用程式憑證，則請指定憑證 SAN 中的 scm 網站，如先前所述。 

## <a name="tls-settings"></a>TLS 設定 

您可以在應用程式層級設定 TLS 設定。  

## <a name="private-client-certificate"></a>私人用戶端憑證 

常見使用案例是將應用程式設定為用戶端伺服器模型中的用戶端。 如果您使用私人 CA 憑證保護您的伺服器，則必須將用戶端憑證上傳至您的應用程式。  下列指示會將憑證載入至應用程式執行所在背景工作角色的 truststore。 如果您將憑證載入到一個應用程式，可以將它用於相同 App Service 方案中的其他應用程式，而不需要再次上傳憑證。

將憑證上傳至 ASE 中的應用程式︰

1. 產生憑證的 *.cer* 檔案。 
2. 移至 Azure 入口網站中需要憑證的應用程式
3. 移至應用程式中的 SSL 設定。 按一下 [上傳憑證]。 選取 [公用]。 選取 [本機電腦]。 提供名稱。 瀏覽並選取您的 *.cer* 檔案。 選取上傳。 
4. 複製憑證指紋。
5. 移至 [應用程式設定]。 建立應用程式設定 WEBSITE_LOAD_ROOT_CERTIFICATES，並以憑證指紋作為值。 如果您有多個憑證，可以將它們放在相同設定中，並以逗點分隔且不含任何空白字元，例如 

    84EC242A4EC7957817B8E48913E50953552DAFA6,6A5C65DC9247F762FE17BF8D4906E04FE6B31819

憑證將可由所有應用程式使用，而這些應用程式與設定該設定的應用程式具有相同 App Service 方案。 如果您需要它可用於不同 App Service 方案中的應用程式，必須在該 App Service 方案的應用程式中，重複應用程式設定作業。 若要檢查是否已設定憑證，請移至 Kudu 主控台，並在 PowerShell 偵錯工具主控台中發出下列命令：

```azurepowershell-interactive
dir cert:\localmachine\root
```

若要執行測試，您可以建立自我簽署的憑證，並使用下列 PowerShell 產生 *.cer* 檔案： 

```azurepowershell-interactive
$certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

$certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
$password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

$fileName = "exportedcert.cer"
export-certificate -Cert $certThumbprint -FilePath $fileName -Type CERT
```
---
title: 透過 Azure PowerShell 將 Vm 部署到您的 Azure Stack Edge GPU 裝置
description: 說明如何使用 Azure PowerShell，在 Azure Stack Edge GPU 裝置上建立及管理虛擬機器 (Vm) 。
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: aa35111a2fa26b3e4fd5e80a8227b7c244f30e9f
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2020
ms.locfileid: "89461709"
---
# <a name="deploy-vms-on-your-azure-stack-edge-gpu-device-via-azure-powershell"></a>透過 Azure PowerShell 將 Vm 部署到您的 Azure Stack Edge GPU 裝置

<!--[!INCLUDE [azure-stack-edge-gateway-deploy-vm-overview](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-overview.md)]-->

本教學課程說明如何使用 Azure PowerShell 在您的 Azure Stack Edge 裝置上建立和管理 VM。

## <a name="vm-deployment-workflow"></a>VM 部署工作流程

下圖說明部署工作流程。

![VM 部署工作流程](media/azure-stack-edge-j-series-deploy-virtual-machine-powershell/vm-workflow_r.svg)

## <a name="prerequisites"></a>必要條件

[!INCLUDE [azure-stack-edge-gateway-deploy-vm-prerequisites](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-prerequisites.md)]



## <a name="query-for-built-in-subscription-on-the-device"></a>查詢裝置上的內建訂用帳戶

針對 Azure Resource Manager，只支援單一使用者可見的固定訂用帳戶。 此訂用帳戶對每個裝置而言是唯一的，且此訂用帳戶名稱或訂用帳戶識別碼無法變更

此訂用帳戶包含建立 VM 所需的所有資源。 

> [!IMPORTANT]
> 此訂用帳戶未連線或與您的 Azure 訂用帳戶相關，並在您的本機裝置上。

此訂用帳戶將用來部署 Vm。

1.  若要列出此訂用帳戶，請輸入：

    ```powershell
    Get-AzureRmSubscription
    ```
    
    下方顯示一項範例輸出。

    ```powershell
    PS C:\windows\system32> Get-AzureRmSubscription
    
    Name                 Id                 TenantId          State
    ----                 --                --------           -----
    Default Provider Subscription A4257FDE-B946-4E01-ADE7-674760B8D1A3 c0257de7-538f-415c-993a-1b87a031879d Enabled
    
    PS C:\windows\system32>
    ```
        
3.  取得在裝置上執行的已註冊資源提供者清單。 這份清單通常包含計算、網路和儲存體。

    ```powershell
    Get-AzureRMResourceProvider
    ```

    > [!NOTE]
    > 資源提供者已預先註冊，無法修改或變更。
    
    範例輸出如下所示：

    ```powershell
    Get-AzureRmResourceProvider
    ProviderNamespace : Microsoft.Compute
    RegistrationState : Registered
    ResourceTypes     : {virtualMachines, virtualMachines/extensions, locations, operations...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Network
    RegistrationState : Registered
    ResourceTypes     : {operations, locations, locations/operations, locations/usages...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Resources
    RegistrationState : Registered
    ResourceTypes     : {tenants, locations, providers, checkresourcename...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Storage
    RegistrationState : Registered
    ResourceTypes     : {storageaccounts, storageAccounts/blobServices, storageAccounts/tableServices,
                        storageAccounts/queueServices...}
    Locations         : {DBELocal}
    ZoneMappings      :
    ```
    
## <a name="create-a-resource-group"></a>建立資源群組

使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 建立 Azure 資源群組。 資源群組是一個邏輯容器，可在其中部署及管理 Azure 資源（例如儲存體帳戶、磁片、受控磁片）。

> [!IMPORTANT]
> 所有資源會建立在與裝置相同的位置中，且位置會設定為 **DBELocal**。

```powershell
New-AzureRmResourceGroup -Name <Resource group name> -Location DBELocal
```

下方顯示一項範例輸出。

```powershell
New-AzureRmResourceGroup -Name rg191113014333 -Location DBELocal 
Successfully created Resource Group:rg191113014333
```

## <a name="create-a-storage-account"></a>建立儲存體帳戶

使用上一個步驟中建立的資源群組來建立新的儲存體帳戶。 這是 **本機儲存體帳戶** ，將用來上傳 VM 的虛擬磁片映射。

```powershell
New-AzureRmStorageAccount -Name <Storage account name> -ResourceGroupName <Resource group name> -Location DBELocal -SkuName Standard_LRS
```

> [!NOTE]
> 只有本機儲存體帳戶（例如本機存放區的儲存體 (Standard_LRS 或 Premium_LRS) 可透過 Azure Resource Manager 建立。 若要建立階層式儲存體帳戶，請參閱「新增」中的步驟 [，連接到您 Azure Stack Edge 上的儲存體帳戶](azure-stack-edge-j-series-deploy-add-storage-accounts.md)。

下方顯示一項範例輸出。

```powershell
New-AzureRmStorageAccount -Name sa191113014333  -ResourceGroupName rg191113014333 -SkuName Standard_LRS -Location DBELocal

ResourceGroupName      : rg191113014333
StorageAccountName     : sa191113014333
Id                     : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Microsoft.Storage/storageaccounts/sa191113014333
Location               : DBELocal
Sku                    : Microsoft.Azure.Management.Storage.Models.Sku
Kind                   : Storage
Encryption             : Microsoft.Azure.Management.Storage.Models.Encryption
AccessTier             :
CreationTime           : 11/13/2019 9:43:49 PM
CustomDomain           :
Identity               :
LastGeoFailoverTime    :
PrimaryEndpoints       : Microsoft.Azure.Management.Storage.Models.Endpoints
PrimaryLocation        : DBELocal
ProvisioningState      : Succeeded
SecondaryEndpoints     :
SecondaryLocation      :
StatusOfPrimary        : Available
StatusOfSecondary      :
Tags                   :
EnableHttpsTrafficOnly : False
NetworkRuleSet         :
Context                : Microsoft.WindowsAzure.Commands.Common.Storage.LazyAzureStorageContext
ExtendedProperties     : {}
```

若要取得儲存體帳戶金鑰，請執行 `Get-AzureRmStorageAccountKey` 命令。 此命令的範例輸出如下所示。

```powershell
PS C:\Users\Administrator> Get-AzureRmStorageAccountKey

cmdlet Get-AzureRmStorageAccountKey at command pipeline position 1
Supply values for the following parameters:
(Type !? for Help.)
ResourceGroupName: my-resource-ase
Name:myasestoracct

KeyName Value
------- -----
key1 /IjVJN+sSf7FMKiiPLlDm8mc9P4wtcmhhbnCa7...
key2 gd34TcaDzDgsY9JtDNMUgLDOItUU0Qur3CBo6Q...
```

## <a name="add-blob-uri-to-hosts-file"></a>將 Blob URI 新增至 hosts 檔案

您已在 [ [修改主機檔案以進行端點名稱解析](azure-stack-edge-j-series-connect-resource-manager.md#step-5-modify-host-file-for-endpoint-name-resolution)] 區段中，為您要用來連線到 blob 儲存體的用戶端，在主機檔案中新增 blob URI。 這是 blob URI 的專案：

\<Azure consistent network services VIP \>\<storage name\>. blob ... \<appliance name\>\<dnsdomain\>


## <a name="install-certificates"></a>安裝憑證

如果您使用 *HTTPs*，則需要在裝置上安裝適當的憑證。 在此情況下，請安裝 blob 端點憑證。 如需詳細資訊，請參閱如何在 [管理憑證](azure-stack-edge-j-series-manage-certificates.md)中建立和上傳憑證。

## <a name="upload-a-vhd"></a>上傳 VHD

複製您在先前步驟中建立之本機儲存體帳戶中的分頁 blob 所使用的任何磁片映射。 您可以使用 [AzCopy](../storage/common/storage-use-azcopy-v10.md) 之類的工具，將 VHD 上傳至您在先前步驟中建立的儲存體帳戶。 

使用 AzCopy 之前，請確定已 [正確設定 AzCopy](#configure-azcopy) ，以便與您用來搭配 Azure Stack Edge 裝置的 blob 儲存體 REST API 版本搭配使用。

```powershell
AzCopy /Source:<sourceDirectoryForVHD> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Y /S /V /NC:32  /BlobType:page /destType:blob 
```

> [!NOTE]
> 設定 `BlobType` 為頁面，以從 VHD 建立受控磁片。 設定 `BlobType` 為使用 AzCopy 寫入階層式儲存體帳戶時封鎖。

您可以從 marketplace 下載磁片映射。 如需詳細步驟，請移至 [從 Azure Marketplace 取得虛擬磁片映射](azure-stack-edge-j-series-create-virtual-machine-image.md)。

使用 AzCopy 7.3 的範例輸出如下所示。 如需此命令的詳細資訊，請移至 [使用 AzCopy 將 VHD 檔案上傳至儲存體帳戶](../devtest-labs/devtest-lab-upload-vhd-using-azcopy.md)。


```powershell
AzCopy /Source:\\hcsfs\scratch\vm_vhds\linux\ /Dest:http://sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages /DestKey:gJKoyX2Amg0Zytd1ogA1kQ2xqudMHn7ljcDtkJRHwMZbMK== /Y /S /V /NC:32 /BlobType:page /destType:blob /z:2e7d7d27-c983-410c-b4aa-b0aa668af0c6
```


## <a name="create-managed-disks-from-the-vhd"></a>從 VHD 建立受控磁碟

從上傳的 VHD 建立受控磁片。

```powershell
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import -SourceUri "Source URL for your VHD"
```
範例輸出如下所示： 
<code>
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import –SourceUri http://</code><code>sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages/ubuntu13.vhd</code> 

```powershell
New-AzureRMDisk -ResourceGroupName <Resource group name> -DiskName <Disk name> -Disk $DiskConfig
```

下方顯示一項範例輸出。 如需此 Cmdlet 的詳細資訊，請移至 [AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk?view=azurermps-6.13.0)。

```powershell
Tags               :
New-AzureRmDisk -ResourceGroupName rg191113014333 -DiskName ld191113014333 -Disk $DiskConfig

ResourceGroupName  : rg191113014333
ManagedBy          :
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              :
TimeCreated        : 11/13/2019 1:49:07 PM
OsType             :
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 30
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micros
                     oft.Compute/disks/ld191113014333
Name               : ld191113014333
Type               : Microsoft.Compute/disks
Location           : DBELocal
Tags               : {}
```

## <a name="create-a-vm-image-from-the-image-managed-disk"></a>從映像受控磁碟建立 VM 映像

使用下列命令，從受控磁片建立 VM 映射。 將內的值取代 \< \> 為您選擇的名稱。

```powershell
$imageConfig = New-AzureRmImageConfig -Location DBELocal
$ManagedDiskId = (Get-AzureRmDisk -Name <Disk name> -ResourceGroupName <Resource group name>).Id
Set-AzureRmImageOsDisk -Image $imageConfig -OsType '<OS type>' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId 

The supported OS types are Linux and Windows.

For OS Type=Linux, for example:
Set-AzureRmImageOsDisk -Image $imageConfig -OsType 'Linux' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId
New-AzureRmImage -Image $imageConfig -ImageName <Image name>  -ResourceGroupName <Resource group name>
```

下方顯示一項範例輸出。 如需此 Cmdlet 的詳細資訊，請移至 [>new-azurermimage](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage?view=azurermps-6.13.0)。

```powershell
New-AzureRmImage -Image Microsoft.Azure.Commands.Compute.Automation.Models.PSImage -ImageName ig191113014333  -ResourceGroupName rg191113014333
ResourceGroupName    : rg191113014333
SourceVirtualMachine :
StorageProfile       : Microsoft.Azure.Management.Compute.Models.ImageStorageProfile
ProvisioningState    : Succeeded
Id                   : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micr
                       osoft.Compute/images/ig191113014333
Name                 : ig191113014333
Type                 : Microsoft.Compute/images
Location             : dbelocal
Tags                 : {}
```

## <a name="create-vm-with-previously-created-resources"></a>使用先前建立的資源建立 VM

建立及部署 VM 之前，您必須先建立一個虛擬網路，並建立虛擬網路介面的關聯。

> [!IMPORTANT]
> 建立虛擬網路和虛擬網路介面時，適用下列規則：
> - 您只可以在資源群組)  (建立一個 Vnet，而且必須完全符合該邏輯網路的位址空間。
> -   Vnet 中只允許一個子網。 子網必須與 Vnet 完全相同的位址空間。
> -   Vnic 建立期間只允許靜態配置方法，使用者必須提供私人 IP 位址。

 
**建立 Vnet**

```powershell
$subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name <Subnet name> -AddressPrefix <Address Prefix>
$aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName <Resource group name> -Name <Vnet name> -Location DBELocal -AddressPrefix <Address prefix> -Subnet $subNetId
```

**使用 Vnet 子網識別碼建立 Vnic**

```powershell
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <IP config Name> -SubnetId $aRmVN.Subnets[0].Id -PrivateIpAddress <Private IP>
$Nic = New-AzureRmNetworkInterface -Name <Nic name> -ResourceGroupName <Resource group name> -Location DBELocal -IpConfiguration $ipConfig
```

這些命令的範例輸出如下所示：

```powershell
PS C:\Users\Administrator> $subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name my-ase-subnet -AddressPrefix "5.5.0.0/16"

PS C:\Users\Administrator> $aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName Resource-my-ase -Name my-ase-virtualnetwork -Location DBELocal -AddressPrefix "5.5.0.0/16" -Subnet $subNetId
WARNING: The output object type of this cmdlet will be modified in a future release.
PS C:\Users\Administrator> $ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name my-ase-ip -SubnetId $aRmVN.Subnets[0].Id
PS C:\Users\Administrator> $Nic = New-AzureRmNetworkInterface -Name my-ase-nic -ResourceGroupName Resource-my-ase -Location DBELocal -IpConfiguration $ipConfig
WARNING: The output object type of this cmdlet will be modified in a future release.

PS C:\Users\Administrator> $Nic

PS C:\Users\Administrator> (Get-AzureRmNetworkInterface)[0]

Name                        : nic200108020444
ResourceGroupName           : rg200108020444
Location                    : dbelocal
Id                          : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Network/networ
                              kInterfaces/nic200108020444
Etag                        : W/"f9d1759d-4d49-42fa-8826-e218e0b1d355"
ResourceGuid                : 3851ae62-c13e-4416-9386-e21d9a2fef0f
ProvisioningState           : Succeeded
Tags                        :
VirtualMachine              : {
                                "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Compu
                              te/virtualMachines/VM200108020444"
                              }
IpConfigurations            : [
                                {
                                  "Name": "ip200108020444",
                                  "Etag": "W/\"f9d1759d-4d49-42fa-8826-e218e0b1d355\"",
                                  "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Net
                              work/networkInterfaces/nic200108020444/ipConfigurations/ip200108020444",
                                  "PrivateIpAddress": "5.5.166.65",
                                  "PrivateIpAllocationMethod": "Static",
                                  "Subnet": {
                                    "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/DbeSystemRG/providers/Microsoft.Netw
                              ork/virtualNetworks/vSwitch1/subnets/subnet123",
                                    "ResourceNavigationLinks": [],
                                    "ServiceEndpoints": []
                                  },
                                  "ProvisioningState": "Succeeded",
                                  "PrivateIpAddressVersion": "IPv4",
                                  "LoadBalancerBackendAddressPools": [],
                                  "LoadBalancerInboundNatRules": [],
                                  "Primary": true,
                                  "ApplicationGatewayBackendAddressPools": [],
                                  "ApplicationSecurityGroups": []
                                }
                              ]
DnsSettings                 : {
                                "DnsServers": [],
                                "AppliedDnsServers": []
                              }
EnableIPForwarding          : False
EnableAcceleratedNetworking : False
NetworkSecurityGroup        : null
Primary                     : True
MacAddress                  : 00155D18E432                :
```

（選擇性）建立 VM 的 Vnic 時，您可以傳遞公用 IP。 在此情況下，公用 IP 會傳回私人 IP。 

```powershell
New-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <ResourceGroupName> -AllocationMethod Static -Location DBELocal
$publicIP = (Get-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <Resource group name>).Id
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <ConfigName> -PublicIpAddressId $publicIP -SubnetId $subNetId
```


**建立 VM**

您現在可以使用 VM 映射來建立 VM，並將它連接到您稍早建立的虛擬網路。

```powershell
$pass = ConvertTo-SecureString "<Password>" -AsPlainText -Force;
$cred = New-Object System.Management.Automation.PSCredential("<Enter username>", $pass)

You will use this username, password to login to the VM, once it is created and powered up.

$VirtualMachine = New-AzureRmVMConfig -VMName <VM name> -VMSize "Standard_D1_v2"

$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -<OS type> -ComputerName <Your computer Name> -Credential $cred

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name <OS Disk Name> -Caching "ReadWrite" -CreateOption "FromImage" -Linux -StorageAccountType StandardLRS

$nicID = (Get-AzureRmNetworkInterface -Name <nic name> -ResourceGroupName <Resource Group Name>).Id

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nicID

$image = (Get-AzureRmImage -ResourceGroupName <Resource Group Name> -ImageName $ImageName).Id

$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -Id $image

New-AzureRmVM -ResourceGroupName <Resource Group Name> -Location DBELocal -VM $VirtualMachine -Verbose
```

## <a name="connect-to-a-vm"></a>連接到 VM

根據您建立的是 Windows 或 Linux VM，連接的步驟可能會不同。

### <a name="connect-to-linux-vm"></a>連接至 Linux VM

遵循下列步驟以連線至 Linux VM。

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-linux.md)]

### <a name="connect-to-windows-vm"></a>連接到 Windows VM

遵循下列步驟以連線至 Windows VM。

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-windows.md)]


<!--Connect to the VM using the private IP that you passed during the VM creation.

Open an SSH session to connect with the IP address.

`ssh -l <username> <ip address>`

When prompted, provide the password that you used when creating the VM.

If you need to provide the SSH key, use this command.

ssh -i c:/users/Administrator/.ssh/id_rsa Administrator@5.5.41.236

If you used a public IP address during VM creation, you can use that IP to connect to the VM. To get the public IP: 

```powershell
$publicIp = Get-AzureRmPublicIpAddress -Name <Public IP> -ResourceGroupName <Resource group name>
```
The public IP in this case will be the same as the private IP that you passed during virtual network interface creation.-->


## <a name="manage-vm"></a>管理 VM

下一節說明您將在 Azure Stack Edge 裝置上建立之 VM 的一些一般作業。

### <a name="list-vms-running-on-the-device"></a>列出在裝置上執行的 Vm

若要傳回 Azure Stack Edge 裝置上執行的所有 Vm 清單，請執行下列命令。


`Get-AzureRmVM -ResourceGroupName <String> -Name <String>`
 

### <a name="turn-on-the-vm"></a>開啟 VM

執行下列 Cmdlet 以開啟在您的裝置上執行的虛擬機器：


`Start-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>`


如需此 Cmdlet 的詳細資訊，請移至 [new-azurermvm](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm?view=azurermps-6.13.0)。

### <a name="suspend-or-shut-down-the-vm"></a>暫止或關閉 VM

執行下列 Cmdlet 來停止或關閉在裝置上執行的虛擬機器：


```powershell
Stop-AzureRmVM [-Name] <String> [-StayProvisioned] [-ResourceGroupName] <String>
```


如需此 Cmdlet 的詳細資訊，請移至 [new-azurermvm Cmdlet](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm?view=azurermps-6.13.0)。

### <a name="add-a-data-disk"></a>新增資料磁碟

如果 VM 上的工作負載需求增加，您可能需要新增資料磁片。

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" -VhdUri "https://contoso.blob.core.windows.net/vhds/diskstandard03.vhd" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty 
 
Update-AzureRmVM -ResourceGroupName "<Resource Group Name string>" -VM $VirtualMachine
```

### <a name="delete-the-vm"></a>刪除 VM

執行下列 Cmdlet，以從您的裝置移除虛擬機器：

```powershell
Remove-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>
```

如需此 Cmdlet 的詳細資訊，請移至 [new-azurermvm Cmdlet](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm?view=azurermps-6.13.0)。


## <a name="supported-vm-sizes"></a>支援的 VM 大小

VM 大小會決定可供 VM 使用的計算資源 (例如 CPU、GPU 和記憶體) 數量。 虛擬機器必須適用於工作負載的 VM 大小來建立。 即使所有電腦都在相同的硬體上執行，電腦大小還是會有不同的磁片存取限制，這可協助您管理整個 Vm 的整體磁片存取。 如果工作負載增加，可以調整現有虛擬機器的大小。

以下是支援在 Azure Stack Edge 裝置上建立的標準 Dv2 系列 Vm。

### <a name="dv2-series"></a>Dv2 系列
|大小     |vCPU     |記憶體 (GiB) | 暫存儲存體 (GiB)  | 最大 OS 磁碟輸送量 (IOPS) | 最大暫存儲存體輸送量 (IOPS) | 最大資料磁碟/輸送量 (IOPS) | 最大 NIC |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3.5 |50   |500 |3000  |4 / 4x500   |2 |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8 / 8x500   |2 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16 / 16x500 |4 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32 / 32x500 |8 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64 / 64x500 |8 |

### <a name="dsv2-series"></a>DSv2 系列
|大小     |vCPU     |記憶體 (GiB) | 暫存儲存體 (GiB)  | 最大 OS 磁碟輸送量 (IOPS) | 最大暫存儲存體輸送量 (IOPS) | 最大資料磁碟/輸送量 (IOPS) | 最大 NIC |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3.5 |7   |1000 |4000  |4 / 4x2300   |2 |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8 / 8x2300   |2 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16 / 16x2300 |4 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32 / 32x2300 |8 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64 / 64x2300 |8 |

### <a name="dv2-series"></a>Dv2 系列
|大小     |vCPU     |記憶體 (GiB) | 暫存儲存體 (GiB)  | 最大 OS 磁碟輸送量 (IOPS) | 最大暫存儲存體輸送量 (IOPS) | 最大資料磁碟/輸送量 (IOPS) | 最大 NIC |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8 / 8x500    |2 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16 / 16x500  |4 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32 / 32x500  |8 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64 / 64x500  |8 |


### <a name="dsv2-series"></a>DSv2 系列
|大小     |vCPU     |記憶體 (GiB) | 暫存儲存體 (GiB)  | 最大 OS 磁碟輸送量 (IOPS) | 最大暫存儲存體輸送量 (IOPS) | 最大資料磁碟/輸送量 (IOPS) | 最大 NIC |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |4 / 4x2300    |2 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |8 / 8x2300    |4 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |16 / 16x2300  |8 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |32 / 32x2300  |8 |

如需詳細資訊，請移至 [一般用途 VM 大小的 Dv2 系列](../virtual-machines/dv2-dsv2-series.md#dv2-series)。

## <a name="unsupported-vm-operations-and-cmdlets"></a>不支援的 VM 作業和 Cmdlet

不支援擴充功能、擴展集、可用性設定組、快照集。

## <a name="configure-azcopy"></a>設定 AzCopy

當您安裝最新版的 AzCopy 時，您將需要設定 AzCopy，以確保它符合 Azure Stack Edge 裝置 REST API 版本的 blob 儲存體。

在用來存取 Azure Stack Edge 裝置的用戶端上，設定全域變數以符合 blob 儲存體 REST API 版本。

### <a name="on-windows-client"></a>在 Windows 用戶端上 

`$Env:AZCOPY_DEFAULT_SERVICE_API_VERSION = "2017-11-09"`

### <a name="on-linux-client"></a>在 Linux 用戶端上

`export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09`

若要確認是否已正確設定 AzCopy 的環境變數，請執行下列步驟：

1. 執行 "azcopy env"
2. 尋找 `AZCOPY_DEFAULT_SERVICE_API_VERSION` 參數。 這應該會有您在先前步驟中設定的值。


## <a name="next-steps"></a>接下來的步驟

[Azure Resource Manager Cmdlet](https://docs.microsoft.com/powershell/module/azurerm.resources/?view=azurermps-6.13.0)

---
title: Azure VMware Solution by CloudSimple-為 Oracle RAC 優化您的 CloudSimple 私用雲端
description: 說明如何部署新的叢集，並針對 Oracle Real Application cluster （RAC）安裝和設定將 VM 優化
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/06/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2cc2f954f4255c00b7c3549ab5d33d71b240fb70
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86507662"
---
# <a name="optimize-your-cloudsimple-private-cloud-for-installing-oracle-rac"></a>優化您的 CloudSimple 私人雲端以安裝 Oracle RAC

您可以在 CloudSimple 私用雲端環境中部署 Oracle Real 應用程式叢集（RAC）。 本指南說明如何部署新的叢集，並將 Oracle RAC 解決方案的 VM 優化。 完成本主題中的步驟之後，您就可以安裝和設定 Oracle RAC。

## <a name="storage-policy"></a>儲存體原則

成功地執行 Oracle RAC 需要在叢集中有足夠數目的節點。  在 vSAN 儲存原則中，可容忍的失敗（FTT）會套用至用來儲存資料庫、記錄和重做磁片的資料磁片。  若要有效容忍失敗，所需的節點數目是 2N + 1，其中 N 是 FTT 的值。

範例：如果所需的 FTT 為2，則叢集中的節點總數必須是 2 * 2 + 1 = 5。

## <a name="overview-of-deployment"></a>部署概觀

下列各節說明如何為 Oracle RAC 設定 CloudSimple 私用雲端環境。

1. 磁片設定的最佳做法
2. 部署 CloudSimple 私用雲端 vSphere 叢集
3. 設定 Oracle RAC 的網路功能
4. 設定 vSAN 儲存原則
5. 建立 Oracle Vm 並建立共用 VM 磁片
6. 設定 VM 對主機的親和性規則

## <a name="best-practices-for-disk-configuration"></a>磁片設定的最佳做法

Oracle RAC 虛擬機器有多個磁片，用於特定功能。  共用磁片會掛接在 Oracle RAC 叢集所使用的所有虛擬機器上。  作業系統和軟體安裝磁片只會裝載在個別的虛擬機器上。  

![Oracle RAC 虛擬機器磁片總覽](media/oracle-vm-disks-overview.png)

下列範例會使用下表中定義的磁片。

| 磁碟                                      | 目的                                       | 共用磁碟 |
|-------------------------------------------|-----------------------------------------------|-------------|
| OS                                        | 作業系統磁碟                         | 否          |
| 格                                      | 安裝 Oracle Grid 軟體的位置     | 否          |
| DATABASE                                  | Oracle 資料庫軟體的安裝位置 | 否          |
| ORAHOME                                   | Oracle 資料庫二進位檔的基底位置    | 否          |
| DATA1、DATA2、DATA3、DATA4                | 儲存 Oracle 資料庫檔案的磁片   | 是         |
| REDO1, REDO2, REDO3, REDO4, REDO5, REDO6  | 重做記錄磁片                                | 是         |
| OCR1, OCR2, OCR3, OCR4, OCR5              | 投票磁片                                  | 是         |
| FRA1, FRA2                                | 快速復原區域磁片                      | 是         |

![Oracle 虛擬機器磁片設定](media/oracle-vmdk.png)

### <a name="virtual-machine-configuration"></a>虛擬機器組態

* 每部虛擬機器都設定了四個 SCSI 控制器。
* [SCSI 控制器類型] 設定為 [VMware paravirtual]。
* 建立多個虛擬磁片（.vmdk）。
* 磁片裝載于不同的 SCSI 控制器上。
* 已針對共用叢集磁片設定多重寫入器共用類型。
* 已定義 vSAN 儲存體原則，以確保磁片的高可用性。

### <a name="operating-system-and-software-disk-configuration"></a>作業系統和軟體磁片設定

每部 Oracle 虛擬機器都會針對主機作業系統、交換、軟體安裝和其他作業系統功能，設定多個磁片。  這些磁片不會在虛擬機器之間共用。  

* 每部虛擬機器都有三個磁片設定為虛擬磁片，並掛接在 Oracle RAC 虛擬機器上。
    * OS 磁碟
    * 用於儲存 Oracle 方格的磁片會安裝檔案
    * 用於儲存 Oracle 資料庫安裝檔案的磁片
* 磁片可以設定為**精簡布建**。
* 每個磁片會掛接在第一個 SCSI 控制器（SCSI0）上。  
* [共用] 設定為 [**無共用**]。
* 使用 vSAN 原則在儲存體上定義冗余。  

![Oracle RAC 資料磁片群組設定](media/oracle-vm-os-disks.png)

### <a name="data-disk-configuration"></a>資料磁片設定

資料磁片主要是用來儲存資料庫檔案。  

* 四個磁片會設定為虛擬磁片，並掛接在所有 Oracle RAC 虛擬機器上。
* 每個磁片會掛接在不同的 SCSI 控制器上。
* 每個虛擬磁片都會設定為「粗布建」立即**清空**。  
* [共用] 設定為 [**多重寫入器**]。  
* 磁片必須設定為自動存放裝置管理（ASM）磁片群組。  
* 使用 vSAN 原則在儲存體上定義冗余。  
* ASM 冗余設定為 [**外部**冗余]。

![Oracle RAC 資料磁片群組設定](media/oracle-vm-data-disks.png)

### <a name="redo-log-disk-configuration"></a>重做記錄磁片設定

重做記錄檔是用來儲存對資料庫所做的變更複本。  當資料需要在任何失敗之後復原時，會使用記錄檔。

* 重做記錄磁片必須設定為多個磁片群組。  
* 在所有 Oracle RAC 虛擬機器上，會建立並掛接六個磁片。
* 磁片裝載于不同的 SCSI 控制器上
* 每個虛擬磁片都會設定為「粗布建」立即**清空**。
* [共用] 設定為 [**多重寫入器**]。  
* 磁片必須設定為兩個 ASM 磁片群組。
* 每個 ASM 磁片群組都包含三個磁片，它們位於不同的 SCSI 控制器上。  
* ASM 冗余設定為**正常**的冗余。
* 同時在 ASM 重做記錄檔群組上建立五個重做記錄檔

```
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 1 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
SQL > alter database add logfile thread 2 ('+ORCLRAC_REDO1','+ORCLRAC_REDO2') size 1G;
```

![Oracle RAC 重做記錄磁片群組設定](media/oracle-vm-redo-log-disks.png)

### <a name="oracle-voting-disk-configuration"></a>Oracle 投票磁片設定

投票磁片提供仲裁磁碟功能作為額外的通道，以避免任何分裂的情況。

* 系統會在所有 Oracle RAC 虛擬機器上建立並掛接五個磁片。
* 磁片裝載于一個 SCSI 控制器上
* 每個虛擬磁片都會設定為「粗布建」立即**清空**。
* [共用] 設定為 [**多重寫入器**]。  
* 磁片必須設定為 ASM 磁片群組。  
* ASM 冗余設定為**高**冗余。

![Oracle RAC 投票磁片群組設定](media/oracle-vm-voting-disks.png)

### <a name="oracle-fast-recovery-area-disk-configuration-optional"></a>Oracle 快速復原區域磁片設定（選擇性）

快速復原區域（FRA）是由 Oracle ASM 磁片群組管理的檔案系統。  FRA 提供備份和復原檔案的共用存放位置。 Oracle 會在 [快速修復] 區域中建立封存的記錄檔和閃記錄。 Oracle 復原管理員（RMAN）可以選擇性地將其備份組和映射複本儲存在 [快速復原] 區域中，並在媒體復原期間還原檔案時使用。

* 系統會在所有 Oracle RAC 虛擬機器上建立和掛接兩個磁片。
* 磁片裝載于不同的 SCSI 控制器上
* 每個虛擬磁片都會設定為「粗布建」立即**清空**。
* [共用] 設定為 [**多重寫入器**]。  
* 磁片必須設定為 ASM 磁片群組。  
* ASM 冗余設定為 [**外部**冗余]。

![Oracle RAC 投票磁片群組設定](media/oracle-vm-fra-disks.png)

## <a name="deploy-cloudsimple-private-cloud-vsphere-cluster"></a>部署 CloudSimple 私用雲端 vSphere 叢集

若要在您的私人雲端上部署 vSphere 叢集，請遵循此程式：

1. 從 CloudSimple 入口網站，[建立私人雲端](create-private-cloud.md)。 CloudSimple 會在新建立的私用雲端中，建立名為 ' cloudowner ' 的預設 vCenter 使用者。 如需預設私用雲端使用者和許可權模型的詳細資訊，請參閱[瞭解私用雲端許可權模型](learn-private-cloud-permissions.md)。  此步驟會為您的私人雲端建立主要管理叢集。

2. 從 CloudSimple 入口網站，使用新的叢集[展開私人雲端](expand-private-cloud.md)。  此叢集將用來部署 Oracle RAC。  根據所需的容錯（最少三個節點）選取節點的數目。

## <a name="set-up-networking-for-oracle-rac"></a>設定 Oracle RAC 的網路功能

1. 在您的私用雲端中，[建立兩個 vlan](create-vlan-subnet.md)，一個用於 oracle 公用網路，一個用於 oracle 私人網路，並指派適當的子網 CIDRs。
2. 建立 Vlan 之後，請[在私人雲端 vCenter 上建立分散式通訊埠群組](create-vlan-subnet.md#use-vlan-information-to-set-up-a-distributed-port-group-in-vsphere)。
3. 在您的管理叢集上，為 Oracle 環境設定[DHCP 和 DNS 伺服器虛擬機器](dns-dhcp-setup.md)。
4. 在安裝于私人雲端中[的 dns 伺服器上設定 dns 轉送](on-premises-dns-setup.md#create-a-conditional-forwarder)。

## <a name="set-up-vsan-storage-policies"></a>設定 vSAN 儲存原則

vSAN 原則會針對儲存在 VM 磁片上的資料，定義容許的失敗和磁片等量。  建立 VM 時，必須在 VM 磁片上套用所建立的儲存體原則。

1. 登入私人雲端[的 vSphere 用戶端](./vcenter-access.md)。
2. 從頂端功能表中，選取 [**原則和設定檔**]。
3. 從左側功能表中，選取 [ **VM 儲存體原則**]，然後選取 [**建立 Vm 存放裝置原則**]。
4. 為原則輸入有意義的名稱，然後按 **[下一步]**。
5. 在 [**原則結構**] 區段中，選取 [**啟用 vSAN 儲存體的規則**]，然後按 **[下一步]**。
6. 在 [ **vSAN**  >  **可用性**] 區段中，針對 [網站嚴重容錯] 選取 [**無**]。 針對 [可容忍的失敗]，選取所需 FTT 的 [ **RAID-鏡像**] 選項。
    ![vSAN 設定 ](media/oracle-rac-storage-wizard-vsan.png) 。
7. 在 [ **Advanced** ] 區段中，選取每個物件的磁片條紋數目。 針對 [物件空間保留]，選取 [已布**建**]。 選取 [**停用物件總和檢查**碼]。 按 **[下一步]**。
8. 依照畫面上的指示來觀看相容 vSAN 資料存放區的清單、檢查設定，然後完成安裝程式。

## <a name="create-oracle-vms-and-create-shared-vm-disks-for-oracle"></a>建立 Oracle Vm 並建立 Oracle 的共用 VM 磁片

若要建立 Oracle 的 VM，請複製現有的 VM，或建立一個新的 VM。  本節說明如何建立新的 VM，然後再加以複製，以在安裝基礎作業系統之後建立第二個。  建立 Vm 之後，您可以為其建立新增磁片。  Oracle 叢集會使用共用磁片來儲存、資料、記錄和重做記錄。

### <a name="create-vms"></a>建立 VM

1. 在 vCenter 中，按一下 [**主機和**叢集] 圖示。 選取您為 Oracle 建立的叢集。
2. 在叢集上按一下滑鼠右鍵，然後選取 [**新增虛擬機器**]。
3. 選取 [**建立新的虛擬機器**]，然後按 **[下一步]**。
4. 將電腦命名為，選取 Oracle VM 的位置，然後按 **[下一步]**。
5. 選取叢集資源，然後按 **[下一步]**。
6. 選取叢集的 vSAN 資料存放區，然後按 **[下一步]**。
7. 保留預設的 [ESXi 6.5 相容性] 選項，然後按 **[下一步]**。
8. 選取您要建立之 VM 的 ISO 的 [客體作業系統]，然後按 **[下一步]**。
9. 選取安裝 OS 所需的硬碟大小。
10. 若要將應用程式安裝在不同的裝置上，請按一下 [新增**裝置**]。
11. 選取 [網路選項]，並指派針對公用網路建立的分散式通訊埠群組。
12. 若要新增額外的網路介面，請按一下 [**新增裝置**]，然後選取針對私人網路所建立的分散式埠群組。
13. 針對新的 DC/DVD 光碟機，請選取包含適用于慣用作業系統安裝之 ISO 的資料存放區 ISO 檔案。 選取您先前上傳至 [Iso 和範本] 資料夾的檔案，然後按一下 **[確定]**。
14. 檢查設定，然後按一下 **[確定]** 以建立新的 VM。
15. 在 VM 上開啟電源。 安裝作業系統和所需的任何更新

安裝作業系統之後，您可以複製第二個 VM。 以滑鼠右鍵按一下 VM 專案，然後選取 [複製] 選項。

### <a name="create-shared-disks-for-vms"></a>建立 Vm 的共用磁片

Oracle 使用共用磁片來儲存資料、記錄和重做記錄檔。  您可以在 vCenter 上建立共用磁片，並將它掛接在這兩個 Vm 上。  如需更高的效能，請將資料磁片放在不同的 SCSI 控制器步驟底下，說明如何在 vCenter 上建立共用磁片，然後將它連結至虛擬機器。 vCenter Flash client 用於修改 VM 屬性。

#### <a name="create-disks-on-the-first-vm"></a>在第一個 VM 上建立磁片

1. 在 vCenter 中，以滑鼠右鍵按一下其中一個 Oracle Vm，然後選取 [**編輯設定**]。
2. 在 [新增裝置] 區段中，選取 [ **SCSI 控制器**]，**然後按一下 [新增]**。
3. 在 [新增裝置] 區段中，選取 [**新增硬碟**]，**然後按一下 [新增]**。
4. 展開 [新硬碟] 的屬性。
5. 指定硬碟的大小。
6. 將 VM 存放裝置原則指定為您稍早定義的 vSAN 儲存原則。
7. 選取 [位置] 做為 vSAN 資料存放區上的資料夾。 此位置可協助您流覽磁片，並將其連接到第二個 VM。
8. 針對 [磁片布建]，選取 [粗布建立即**清空**]。
9. 針對 [共用]，指定**多個寫入器**。
10. 在 [虛擬裝置] 節點中，選取在步驟2中建立的新 SCSI 控制器。

    ![在第一個 VM 上建立磁片](media/oracle-rac-new-hard-disk.png)

針對 Oracle 資料、記錄和重做記錄檔所需的所有新磁片，重複步驟2–10。

#### <a name="attach-disks-to-second-vm"></a>將磁片連結至第二個 VM

1. 在 vCenter 中，以滑鼠右鍵按一下其中一個 Oracle Vm，然後選取 [**編輯設定**]。
2. 在 [新增裝置] 區段中，選取 [ **SCSI 控制器**]，**然後按一下 [新增]**。
3. 在 [新增裝置] 區段中，選取 [**現有硬碟**]，**然後按一下 [新增]**。
4. 流覽至為第一個 VM 建立磁片的位置，然後選取 [VMDK] 檔案。
5. 將 VM 存放裝置原則指定為您稍早定義的 vSAN 儲存原則。
6. 針對 [磁片布建]，選取 [粗布建立即**清空**]。
7. 針對 [共用]，指定**多個寫入器**。
8. 在 [虛擬裝置] 節點中，選取在步驟2中建立的新 SCSI 控制器。

    ![在第一個 VM 上建立磁片](media/oracle-rac-existing-hard-disk.png)

針對 Oracle 資料、記錄和重做記錄檔所需的所有新磁片，重複步驟2到7。

## <a name="set-up-vm-host-affinity-rules"></a>設定 VM 主機親和性規則

VM 對主機親和性規則可確保 VM 會在所需的主機上執行。  您可以在 vCenter 上定義規則，以確保 Oracle VM 在具有適當資源的主機上執行，並符合任何特定的授權需求。

1. 在 CloudSimple 入口網站中，呈報 cloudowner 使用者的[許可權](escalate-private-cloud-privileges.md)。
2. 登入私人雲端的 vSphere 用戶端。
3. 在 vSphere 用戶端中，選取部署 Oracle Vm 的叢集，然後按一下 [**設定**]。
4. 在 [設定] 底下，選取 [ **VM/主機群組**]。
5. 按一下 **+** 。
6. 新增 VM 群組。 選取 [ **VM 群組**] 作為類型。 輸入群組的名稱。 選取 Vm，然後按一下 **[確定]** 以建立群組。
6. 新增主機群組。 選取 [**主機群組**] 做為類型。 輸入群組的名稱。 選取將執行 Vm 的主機，然後按一下 **[確定]** 以建立群組。
7. 若要建立規則，請按一下 [ **VM/主機規則**]。
8. 按一下 **+** 。
9. 輸入規則的名稱，然後勾選 [**啟用**]。
10. 針對 [規則類型]，選取 [**虛擬機器裝載**]。
11. 選取包含 Oracle Vm 的 VM 群組。
12. 選取 [**必須在此群組中的主機上執行**]。
13. 選取您建立的主機群組。
14. 按一下 **[確定]** 以建立規則。

## <a name="references"></a>參考資料

* [關於 vSAN 原則](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.virtualsan.doc/GUID-08911FD3-2462-4C1C-AE81-0D4DBC8F7990.html)
* [適用于共用 Vmdk 的 VMware 多重寫入器屬性](https://docs.vmware.com/en/VMware-Cloud-on-AWS/solutions/VMware-Cloud-on-AWS.df6735f8b729fee463802083d46fdc75/GUID-A7642A82B3D6C5F7806DB40A3F2766D9.html)

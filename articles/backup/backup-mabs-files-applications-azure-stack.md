---
title: 備份 Azure Stack Vm 中的檔
description: 使用 Azure 備份可備份 Azure Stack 檔案和應用程式，並復原到 Azure Stack 環境。
ms.topic: conceptual
ms.date: 06/05/2018
ms.openlocfilehash: cae95c10c510969cc0553a54a506789d6be427d7
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89180978"
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>備份 Azure Stack 上的檔案和應用程式

您可以使用 Azure 備份在 Azure Stack 上保護 (或備份) 檔案和應用程式。 若要備份檔案和應用程式，請將 Microsoft Azure 備份伺服器安裝成為在 Azure Stack 上執行的虛擬機器。 您可以保護在相同虛擬網路中任何 Azure Stack 伺服器上的檔案。 安裝 Azure 備份伺服器之後，請新增 Azure 磁片，以增加可用於短期備份資料的本機儲存體。 Azure 備份伺服器會使用 Azure 存放區進行長期保存。

> [!NOTE]
> 雖然 Azure 備份伺服器和 System Center Data Protection Manager (DPM) 很類似，但 DPM 不支援搭配 Azure Stack 使用。
>

本文未涵蓋在 Azure Stack 環境中安裝 Azure 備份伺服器。 若要將 Azure 備份伺服器安裝在 Azure Stack 上，請參閱[安裝 Azure 備份伺服器](backup-mabs-install-azure-stack.md)。

## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>將 Azure Stack VM 中的檔案和資料夾備份至 Azure

若要設定 Azure 備份伺服器來保護 Azure Stack 虛擬機器中的檔案，請開啟 Azure 備份伺服器主控台。 您將使用主控台來設定保護群組，以及保護虛擬機器上的資料。

1. 在 Azure 備份伺服器主控台中，選取 [ **保護** ]，然後在工具列中選取 [ **新增** ]，以開啟 [ **建立新保護組** 嚮導]。

   ![在 Azure 備份伺服器主控台中設定保護](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    可能需要幾秒鐘的時間才能開啟精靈。 開啟嚮導之後，請選取 [ **下一步** ]，前進到 [ **選取保護群組類型** ] 畫面。

   ![開啟新的保護群組精靈](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. 在 [ **選擇保護群組類型** ] 畫面上選擇 [ **伺服器** ]，然後選取 **[下一步]**。

    ![選取保護群組類型](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    [選取群組成員]**** 畫面隨即開啟。

    ![選擇群組成員](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. 在 [ **選擇群組成員** ] 畫面中，選取 **+** 以展開子清單。 針對您想要保護的所有項目，選取核取方塊。 選取所有專案之後，請選取 **[下一步]**。

    ![選取您要保護的每個專案](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    Microsoft 建議將所有共用保護原則的資料置於一個保護群組中。 如需規劃和部署保護群組的完整資訊，請參閱 System Center DPM 文章：[部署保護群組](/system-center/dpm/create-dpm-protection-groups)。

4. 在 [選取資料保護方式]**** 畫面中，鍵入保護群組的名稱。 選取 [我想要使用下列項目進行短期保護：]**** 和 [我想要線上保護]**** 的核取方塊。 選取 [下一步]  。

    ![選擇資料保護方式](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    若要選取 [我想要線上保護]****，您必須先選取 [我想要使用下列項目進行短期保護：磁碟]****。 Azure 備份伺服器不會保護磁帶，因此磁片是短期保護的唯一選擇。

5. 在 [指定短期目標]**** 畫面上，選擇保留儲存至磁碟之復原點的時間長度，以及儲存增量備份的時機。 選取 [下一步]  。

    > [!IMPORTANT]
    > 您**不應該**將作業復原 (備份) 資料保留在 Azure 備份伺服器連結的磁碟上超過五天。
    >

    ![指定短期目標](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png)

    若要在每個排程的復原點之前執行快速完整備份，而不是選取增量備份的間隔，請選取 [ **恰好在復原點之前**]。 如果您要保護應用程式工作負載，Azure 備份伺服器就會根據同步處理頻率排程建立復原點 (前提是應用程式支援增量備份)。 如果應用程式不支援增量備份，Azure 備份伺服器將會執行快速完整備份。

    對於 [檔案復原點]****，指定建立復原點的時機。 選取 [ **修改** ] 以設定建立復原點的時間和星期幾。

6. 在 [檢閱磁碟配置]**** 畫面中，檢閱針對保護群組所配置的儲存集區磁碟空間。

    [資料大小總計]**** 是您想要備份的資料大小，而 [要在 Azure 備份伺服器上佈建的磁碟空間]**** 是建議的保護群組空間。 Azure 備份伺服器會根據這些設定選擇理想的備份磁碟區。 不過，您也可以在 [磁碟配置詳細資料] 中編輯備份磁碟區選項。 對於工作負載，請在下拉式功能表中選取慣用的存放裝置。 您的編輯內容會變更 [可用磁碟儲存體] 窗格中 [儲存體總計] 和 [可用儲存體] 的值。 佈建不足的空間是 Azure 備份伺服器為了能夠在將來繼續順利備份，而建議您在磁碟區中新增的儲存體數量。

7. 在 [選擇複本的建立方式] 中，選取您要如何處理首次的完整資料複寫。 如果您決定透過網路進行複寫，Azure 建議您選擇離峰時間。 對於大量資料或較差的網路狀況，請考慮使用卸除式媒體來複寫資料。

8. 在 [選擇一致性檢查選項]  頁面中，選取您想要自動執行一致性檢查的方式。 啟用一致性檢查，使其只在資料複寫變得不一致時執行，或是根據排程執行。 如果您不想要設定自動一致性檢查，請透過下列方式隨時執行手動檢查：
    * 在 Azure 備份伺服器主控台的 [保護]**** 區域中，以滑鼠右鍵按一下保護群組，然後選取 [執行一致性檢查]****。

9. 如果您選擇備份到 Azure，請確定已在 [指定線上保護資料]**** 頁面上選取要備份到 Azure 的工作負載。

10. 在 [指定線上備份排程]**** 中，指定應進行 Azure 增量備份的時機。

    您可以排程應該執行備份的頻率：每日/週/月/年以及日期/時間。 一天最多可以備份兩次。 系統每次執行備份作業時，都會透過 Azure 備份伺服器磁碟上儲存的備份資料複本，在 Azure 中建立復原點。

11. 在 [Specify online retention policy] \(指定線上保留原則\)**** 中，指定如何在 Azure 中保留透過每日、每週、每月、每年備份所建立的復原點。

12. 在 [選擇線上複寫]**** 中，指定如何進行首次的資料完整複寫。

13. 在 [摘要]**** 上檢閱您的設定。 當您選取 [ **建立群組**] 時，就會進行初始資料複寫。 當資料複寫完成時，[狀態]**** 頁面上的保護群組狀態會顯示為 [正常]****。 初始備份作業會根據保護群組設定進行。

## <a name="recover-file-data"></a>復原檔案資料

使用 Azure 備份伺服器主控台，將資料復原到您的虛擬機器。

1. 在 Azure 備份伺服器主控台的巡覽列上，選取 [復原 **]，然後** 流覽至您想要復原的資料。 在 [結果] 窗格中選取資料。

2. 在 [復原點] 區段的行事曆中，粗體的日期表示復原點可用。 選取要復原的日期。

3. 在 [可復原項目]**** 窗格中，選取您想要復原的項目。

4. 在 [ **動作** ] 窗格中，選取 [復原] 以開啟 [ **復原** 嚮導]。

5. 您可以如下所示復原資料：

    * **復原到原始位置** - 如果用戶端電腦透過 VPN 連線，則這個選項無法運作。 請改用替代位置，然後從該位置複製資料。
    * **復原到替代位置**

6. 指定復原選項：

    * 對於 [現有的版本復原行為]****，選取 [建立複本]****、[略過]**** 或 [覆寫]****。 覆寫只有在復原到原始位置時才可用。
    * 對於 [還原安全性]****，選擇 [套用目的地電腦的設定]**** 或 [套用復原點版本的安全性設定]****。
    * 針對 [ **網路頻寬使用節流**設定]，選取 [ **修改** ] 以啟用網路頻寬使用節流設定。
    * **通知** 選取 **[復原完成時傳送電子郵件**]，然後指定將接收通知的收件者。 使用逗號分隔電子郵件地址。
    * 進行選取之後，請選取 **[下一步]**

7. 檢查您的復原設定，然後選取 [ **復原**]。

    >[!Note]
    >正在進行復原工作時，會取消所選取之復原項目的所有同步處理工作。

如果您使用的是 Modern Backup Storage (MBS)，則不支援檔案伺服器的使用者復原 (EUR)。 檔案伺服器 EUR 相依於 Modern Backup Storage 不會使用的磁碟區陰影複製服務 (VSS)。 如果已啟用 EUR，請使用下列步驟來復原資料：

1. 巡覽至受保護的檔案，並以滑鼠右鍵按一下檔案名稱，然後選取 [內容]****。

2. 在 [ **屬性** ] 功能表上選取 [ **舊版** ]，然後選擇您想要復原的版本。

## <a name="view-azure-backup-server-with-a-vault"></a>使用保存庫來檢視 Azure 備份伺服器

若要在 Azure 入口網站中查看 Azure 備份伺服器實體，您可以遵循下列步驟：

1. 開啟復原服務保存庫。
2. 選取 [備份基礎結構]。
3. 檢視備份管理伺服器。

## <a name="next-steps"></a>後續步驟

如需使用 Azure 備份伺服器保護其他工作負載的資訊，請參閱下列文章：

* [備份 SharePoint 伺服器陣列](./backup-mabs-sharepoint-azure-stack.md)
* [備份 SQL Server](./backup-mabs-sql-azure-stack.md)

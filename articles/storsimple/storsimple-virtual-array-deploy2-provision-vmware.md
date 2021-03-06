---
title: 在 VMware 中布建 StorSimple Virtual Array
description: 部署 StorSimple Virtual Array 的第二個教學課程，內容為如何在 VMware 中佈建虛擬裝置。
author: alkohli
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9810a34021aa039354aad24f84aff373229c0190
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87021472"
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>部署 StorSimple Virtual Array：在 VMware 中佈建
![此圖顯示部署虛擬陣列所需的步驟。第二個步驟的第二個部分是在 VMware 上標示為 [布建]，並反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>概觀

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

本教學課程說明如何在執行 VMware ESXi 5.0、5.5、6.0 或 6.5 的主機系統上佈建及連線到 StorSimple 虛擬陣列。 本文適用於在 Azure 入口網站及 Microsoft Azure 政府服務雲端部署 StorSimple Virtual Array。

您需要有系統管理員權限，才能佈建並連接至虛擬裝置。 佈建及初始安裝程序可能需要大約 10 分鐘的時間才能完成。

## <a name="provisioning-prerequisites"></a>佈建的必要條件
在執行 VMware ESXi 5.0、5.5、6.0 或 6.5 的主機系統上，佈建虛擬裝置的必要條件如下。

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple 裝置管理員服務
在您開始前，請確定：

* 您已完成 [準備入口網站以使用 StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md)中的所有步驟。
* 您已經從 Azure 入口網站下載適用於 VMware 的虛擬裝置映像。 如需詳細資訊，請參閱[準備入口網站以使用 StorSimple Virtual Array 指南](storsimple-virtual-array-deploy1-portal-prep.md)的**步驟 3︰下載虛擬裝置映像**。

### <a name="for-the-storsimple-virtual-device"></a>對於 StorSimple 虛擬裝置
在您部署虛擬裝置之前，請確定：

* 您可以存取執行 Hyper-V (2008 R2 或更新版本)，且可用來佈建裝置的主機系統。
* 主機系統能夠把下列資源專門用來佈建虛擬裝置：

  * 至少 4 顆核心。
  * 至少 8 GB 的 RAM。 若計畫將虛擬陣列設定為檔案伺服器，8 GB 支援 2 百萬個以下的檔案。 您需要 16 GB RAM 才能支援 2 - 4 百萬個檔案。
  * 一個網路介面。
  * 供系統資料使用的 500 GB 虛擬磁碟。

### <a name="for-the-network-in-datacenter"></a>對於資料中心的網路
在您開始前，請確定：

* 您已經檢閱部署 StorSimple 虛擬裝置的網路需求，且已經根據需求設定資料中心的網路。 

## <a name="step-by-step-provisioning"></a>佈建的逐步指示
若要佈建並連接至虛擬裝置，您必須執行下列步驟：

1. 確認主機系統有足夠的資源來符合最低的虛擬裝置需求。
2. 在 Hypervisor 中佈建虛擬裝置。
3. 啟動虛擬裝置，並取得 IP 位址。

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>步驟 1：確認主機系統符合最低的虛擬裝置需求
若要建立虛擬裝置，您將需要：

* 能夠存取執行 VMware ESXi 伺服器 5.0、5.5、6.0 或 6.5 的主機系統。
* 您系統上的 VMware vSphere 用戶端，以便管理 ESXi 主機。

  * 至少 4 顆核心。
  * 至少 8 GB 的 RAM。 若計畫將虛擬陣列設定為檔案伺服器，8 GB 支援 2 百萬個以下的檔案。 您需要 16 GB RAM 才能支援 2 - 4 百萬個檔案。
  * 網路介面，且已連線到能夠將流量路由至網際網路的網路。 網際網路頻寬應該至少要有 5 Mbps，以便讓裝置能夠發揮最大功能。
  * 供資料使用的 500 GB 虛擬磁碟。

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>步驟 2：在 Hypervisor 中佈建虛擬裝置
請執行下列步驟，以便在您的 Hypervisor 中佈建虛擬裝置。

1. 複製您系統中的虛擬裝置映像。 您已透過 Azure 入口網站下載這個虛擬映像。

   1. 請確定您已下載最新的映像檔。 如果您稍早下載過映像，請再下載一次，確定您擁有最新映像。 最新映像具有兩個檔案 (而不是一個)。
   2. 請記下您複製映像的位置，因為稍後會在程序中使用此映像。

2. 使用 vSphere 用戶端登入 ESXi 伺服器。 您需要有系統管理員權限，才能建立虛擬機器。

   ![VSphere 用戶端登入頁面的螢幕擷取畫面。 [IP 位址]、[使用者名稱] 和 [密碼] 方塊包含值，並反白顯示 [登入] 按鈕。](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. 在 vSphere 用戶端中，選取左窗格 [詳細目錄] 區段中的 [ESXi 伺服器]。

   ![VSphere 用戶端主頁面的螢幕擷取畫面。 在 [清查] 區段中，ESXi 伺服器會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. 將 VMDK 上傳至 ESXi 伺服器。 瀏覽至右窗格中的 [組態] **** 索引標籤。 選取 [硬體]**** 下方的 [儲存體]****。

   ![顯示 vSphere 用戶端之 [設定] 索引標籤的螢幕擷取畫面。 在 [硬體] 區段中，[存放裝置] 會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. 在右窗格的 [資料存放區] 下方，選取您要上傳 VMDK 的資料存放區。 資料存放區必須要有足夠的可用空間來容納作業系統和資料磁碟。

   ![顯示 vSphere 用戶端 [儲存體] 頁面的螢幕擷取畫面。 [資料存放區] 索引標籤隨即開啟，並包含資料存放區清單。 已選取一個資料存放區。](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. 按一下滑鼠右鍵並選取 [瀏覽資料存放區]。

   ![顯示所選資料存放區快捷方式功能表的螢幕擷取畫面。 已選取 [流覽資料存放區] 專案。](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. 畫面會出現 [資料存放區瀏覽器]  視窗。

   ![資料存放區瀏覽器的螢幕擷取畫面。 資料存放區中的資料夾是可見的。](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. 在工具列上，按一下 :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png"::: 圖示來建立新資料夾。 然後指定資料夾的名稱，並把該名稱記下來。 您稍後在建立虛擬機器時，將會使用該資料夾名稱 (建議的最佳做法)。 按一下 [確定]  。

   ![已反白顯示新資料夾圖示的資料存放區瀏覽器螢幕擷取畫面。 對話方塊中會填入一個資料夾名稱，並反白顯示 [確定] 按鈕。](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. [資料存放區瀏覽器] 的左窗格中會出現新的資料夾。

   ![資料存放區瀏覽器的螢幕擷取畫面，其中包含在資料夾階層中可見的新資料夾。](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. 按一下 [上傳] 圖示 :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png"::: ，然後選取 [**上傳**檔案]。

    ![螢幕擷取畫面，顯示上傳圖示的快捷方式功能表。 已選取 [上傳檔案] 專案。](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. 瀏覽並指向您已下載的 VMDK 檔案。 有兩個檔案。 選取要上傳的檔案。

    ![對話方塊的螢幕擷取畫面，其中顯示資料夾和兩個 V M D K 檔案。 其中一個檔案會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. 按一下 **[開啟]** 。 就會開始將 VMDK 檔案上傳至指定的資料存放區。 檔案可能需要幾分鐘的時間才能上傳完畢。
13. 上傳完成之後，檔案就會出現在資料存放區裡您所建立的資料夾中。

    ![資料存放區瀏覽器的螢幕擷取畫面。 資料夾階層中的新資料夾會反白顯示，而且上傳的檔案會顯示在該資料夾中。](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    現在將第二個 VMDK 檔案上傳至相同的資料存放區。
14. 返回 vSphere 用戶端視窗。 在選取 ESXi 伺服器之後按一下滑鼠右鍵，然後選取 [新增虛擬機器] ****。

    ![ESXi 伺服器快捷方式功能表的螢幕擷取畫面。 已選取 [新增虛擬機器] 專案。](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. 畫面會出限 [建立新虛擬機器] **** 視窗。 在 [設定]**** 頁面上，選取 [自訂]**** 選項。 按 [下一步]  。
    ![[建立新的虛擬機器] 視窗中 [設定] 頁面的螢幕擷取畫面。 選取 [自訂] 選項，[下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. 在 [名稱和位置]**** 頁面上，指定虛擬機器的名稱。 這個名稱應該與您之前在步驟 8 中指定的資料夾名稱 (建議的最佳做法) 相同。

    ![[建立新的虛擬機器] 視窗的 [名稱] 和 [位置] 頁面的螢幕擷取畫面。 [名稱] 方塊會填入，而且 [下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. 在 [儲存體] **** 頁面上，選取您要用來佈建虛擬機器的資料存放區。

    ![[建立新的虛擬機器] 視窗中 [儲存體] 頁面的螢幕擷取畫面。 [資料存放區] 已選取，而且 [下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. 在 [虛擬機器版本]**** 頁面上，選取 [虛擬機器版本: 8]****。

    ![[虛擬機器版本] 頁面的螢幕擷取畫面。 [虛擬機器第8版] 選項已選取，[下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. 在 [客體作業系統]**** 頁面上，將 [客體作業系統]**** 選取為 [Windows]****。 而對於 [版本]****，請從下拉式清單中選取 [Microsoft Windows Server 2012 (64 位元)]****。

    ![[客體作業系統] 頁面的螢幕擷取畫面，其中已選取 [Windows]、[版本] 設定為 [Microsoft Windows Server 2012 （64位）] 及 [下一步] 反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. 在 [CPU]**** 頁面上，調整 [虛擬通訊端的數目]**** 和 [每個虛擬通訊端的核心數目]****，以便讓 [核心總數]**** 至少為 4。 按 [下一步]  。

    ![[Cpu] 頁面的螢幕擷取畫面，其中顯示一個虛擬插槽、每個虛擬通訊端四個核心，以及四個核心總數。 [下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. 在 [記憶體] **** 頁面上，將 RAM 指定為至少為 8 GB。 按 [下一步]  。

    ![[記憶體] 頁面的螢幕擷取畫面。 記憶體大小會填入 8 GB 的值。](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. 在 [網路] **** 頁面上，指定網路介面的數目。 最低需求是一個網路介面。

    ![[網路] 頁面的螢幕擷取畫面。 網路介面的數目設定為一，[下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. 在 [SCSI 控制器]**** 頁面上，接受預設的 [LSI Logic SAS 控制器]****。

    ![[SCSI 控制器] 頁面的螢幕擷取畫面。 已選取 L S I 邏輯 S 選項，[下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. 在 [選取磁碟]**** 頁面上，選擇 [使用現有的虛擬硬碟]****。 按 [下一步]  。

    ![[選取磁片] 頁面的螢幕擷取畫面，其中已選取 [使用現有的虛擬磁片] 選項，並反白顯示 [下一步] 按鈕。](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. 在 [選取現有的磁碟]**** 頁面的 [磁碟檔案路徑]**** 下方，按一下 [瀏覽]****。 這會開啟 [瀏覽資料存放區] **** 對話方塊。 瀏覽至您之前上傳 VMDK 的位置。 因為您最初上傳回的兩個檔案已合併，所以您現在於資料存放區中只會看到一個檔案。 選取檔案，然後按一下 [確定] ****。 按 [下一步]  。

    ![[選取現有磁片] 頁面的螢幕擷取畫面。 [流覽] 按鈕會反白顯示，而對話方塊會包含一個檔案和反白顯示的 [確定] 按鈕。](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. 在 [進階選項]**** 頁面上，接受預設值並按一下 [下一步]****。

    ![[Advanced Options] 頁面的螢幕擷取畫面。 [下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. 在 [準備完成]  頁面上，檢閱與新的虛擬機器相關的所有設定。 選取 [在完成之前編輯虛擬機器的設定]****。 按一下 [繼續] 。

    ![[準備完成] 頁面的螢幕擷取畫面，其中具有反白顯示的 [繼續] 按鈕。 核取 [完成前編輯虛擬機器設定] 選項。](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. 在 [虛擬機器屬性]**** 頁面的 [硬體]**** 索引標籤上尋找裝置的硬體。 選取 [新增硬碟] ****。 按一下 [新增] 。

    ![[虛擬機器屬性] 頁面之 [硬體] 索引標籤的螢幕擷取畫面。 硬體清單中已選取 [新增硬碟]。 [新增] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. 您會看到 [新增硬體]**** 視窗。 在 [**裝置類型**] 頁面的 **[選擇您想要新增的裝置類型**] 底下，選取 [**硬碟**]，然後按 **[下一步]**。

    ![[新增硬體] 視窗的 [裝置類型] 頁面的螢幕擷取畫面。 選取硬碟裝置，[下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. 在 [選取磁碟]**** 頁面上，選擇 [建立新的虛擬磁碟]****。 按 [下一步]  。

    ![[選取磁片] 頁面的螢幕擷取畫面。 [建立新的虛擬磁片] 選項已選取，而且 [下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. 在 [建立磁碟]**** 頁面上，將 [磁碟大小]**** 變更為至少 500 GB。 500 GB 是最低需求，您永遠可以佈建更大的磁碟。 請注意，佈建之後您無法擴充或縮小磁碟。 如需布建磁片大小的詳細資訊，請參閱[最佳作法檔](storsimple-ova-best-practices.md)中的調整大小一節。 在 [磁碟佈建]**** 下方，選取 [精簡佈建]****。 按 [下一步]  。

    ![[建立磁片] 頁面的螢幕擷取畫面。 [磁片大小] 設為 500 GB，已選取 [精簡布建] 選項，[下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. 在 [進階選項] **** 頁面上，接受預設值。

    ![[Advanced Options] 頁面的螢幕擷取畫面。 [虛擬裝置] 節點設定為 [SCSI （0:0）]，[下一步] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. 在 [準備完成] **** 頁面上，檢閱磁碟的各個選項。 按一下 [完成] 。

    ![[準備完成] 頁面的螢幕擷取畫面。 磁片選項的摘要是可見的，而且 [完成] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. 返回 [虛擬機器屬性] 頁面。 您的虛擬機器已新增一個硬碟。 按一下 [完成] 。

    ![虛擬機器屬性頁面的螢幕擷取畫面。 硬體清單包含新的硬碟，而且 [完成] 按鈕會反白顯示。](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. 在右窗格中選取您的虛擬機器後，流覽至 [**摘要**] 索引標籤。檢查虛擬機器的設定。

    ![[VSphere 用戶端摘要] 索引標籤的螢幕擷取畫面。系統會反白顯示新的虛擬機器，並顯示其 [資源] 和 [一般] 屬性。](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

您的虛擬機器已成功佈建。 下一個步驟是啟動該虛擬機器，然後取得 IP 位址。

> [!NOTE]
> 我們建議您不要在您的虛擬陣列上安裝 VMware 工具 (如同上面所佈建)。 安裝 VMware 工具將導致不支援的組態。

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>步驟 3：啟動虛擬裝置，並取得 IP 位址
請執行下列步驟來啟動您的虛擬裝置，並連線到該虛擬裝置。

#### <a name="to-start-the-virtual-device"></a>啟動虛擬裝置
1. 啟動虛擬裝置。 在 vSphere 設定管理員中，選取左窗格中您的裝置，然後按一下滑鼠右鍵來開啟操作功能表。 選取 [電源]，然後選取 [開啟電源]。 此時您的虛擬機器應該會開機。 您可以在 vSphere 用戶端的 [最近的工作] **** 窗格底部檢視狀態。

   ![裝置快捷方式功能表的螢幕擷取畫面。 已選取 [電源專案]。 當您選取 [開啟電源] 時，就會顯示連續的功能表。](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. 安裝工作將需要幾分鐘的時間才能完成。 裝置執行之後，流覽至 [**主控台**] 索引標籤。傳送 Ctrl + Alt + Delete 以登入裝置。 或者，您可以讓游標指向主控台視窗，然後按下 Ctrl+Alt+Insert。 預設使用者為 *StorSimpleAdmin*，預設密碼為 *Password1*。

   ![[VSphere 用戶端主控台] 索引標籤的螢幕擷取畫面。[密碼] 方塊是空的。](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. 基於安全性理由，裝置系統管理員密碼會在第一次登入時過期。 系統會提示您變更密碼。

   ![[VSphere 用戶端主控台] 索引標籤的螢幕擷取畫面。頁面上的文字說明必須變更密碼。](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. 請輸入至少包含 8 個字元的密碼。 密碼必須包含下列 4 個需求中的 3 個：大寫、小寫、數字和特殊字元。 請重新輸入密碼來加以確認。 系統將會通知您密碼已經變更。

   ![[VSphere 用戶端主控台] 索引標籤的螢幕擷取畫面。頁面上的文字會指出密碼已變更。](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. 密碼變更成功之後，裝置可能會重新開機。 請等待裝置開機完畢。 畫面可能會出現裝置的 Windows PowerShell 主控台及進度列。

   ![螢幕擷取畫面，顯示具有進度列的主控台視窗。 視窗中的文字會指出初始設定正在進行，並要求使用者等待。](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. 步驟 6 至 8 僅適用於在非 DHCP 環境中開機的情況。 如果您是在 DHCP 環境中，請略過這些步驟並前往步驟 9。 如果您是在非 DHCP 環境中讓裝置開機，您會看到下列畫面。

   ![顯示主控台視窗的螢幕擷取畫面，其中包含描述裝置的文字。 命令提示字元會讀取「控制器」，並顯示為可供輸入。](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   接著，設定網路。
7. 使用 `Get-HcsIpAddress` 命令來列出虛擬裝置上已啟用的網路介面。 如果您的裝置有已啟用的單一網路介面，系統指派給該介面的預設名稱會是 `Ethernet`。

   ![顯示主控台視窗的螢幕擷取畫面，其中包含 h 命令的輸出。 「Ethernet」會列為裝置的名稱。](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. 使用 `Set-HcsIpAddress` Cmdlet 來設定網路。 範例如下所示：

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![顯示主控台視窗的螢幕擷取畫面，其中包含 Get-help Set-h 命令的輸出和 h 命令的正確用法。](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. 初始安裝程序完成，且裝置已開機之後，您將會看到裝置橫幅文字。 請記下 IP 位址及橫幅文字中的 URL，以便管理裝置。 您將使用此 IP 位址連線到虛擬裝置的 Web UI，以及完成本機安裝和註冊程序。

   ![顯示主控台視窗的螢幕擷取畫面，其中包含裝置橫幅文字。 該文字包含裝置 IP 位址和 URL。](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (選擇性) 只有當您要在政府服務雲端部署裝置時，才執行這個步驟。 您現在將在您的裝置上啟用美國聯邦資訊處理標準 (FIPS) 模式。 FIPS 140 標準定義核准美國聯邦政府電腦系統所使用的密碼編譯演算法來保護機密資料。

    1. 若要啟用 FIPS 模式，請執行下列 Cmdlet：

        `Enable-HcsFIPSMode`
    2. 在您啟用 FIPS 模式之後，重新啟動您的裝置，讓密碼編譯驗證生效。

       > [!NOTE]
       > 您可以在您的裝置上啟用或停用 FIPS 模式。 不支援在 FIPS 和非 FIPS 模式之間替換裝置。
       >
       >

如果裝置不符合最低設定需求，橫幅文字中會出現錯誤訊息 (如下所示)。 您必須修改裝置設定，讓裝置有足夠的資源來符合最低需求。 然後您就可以將裝置重新啟動，並連線到該裝置。 請參閱 [步驟 1：確認主機系統符合最低的虛擬裝置需求](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements)中的最低組態需求。

![顯示主控台視窗的螢幕擷取畫面，其中包含裝置橫幅文字。 該文字包含一則錯誤訊息，可提供疑難排解問題的 URL。](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

如果您在使用本機 Web UI 進行初始設定時碰到其他任何錯誤，請參閱下列工作流程：

* 執行診斷測試來[疑難排解 WEB UI 安裝程式](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)。
* [產生記錄檔封裝及檢視記錄檔](storsimple-ova-web-ui-admin.md#generate-a-log-package)。

## <a name="next-steps"></a>後續步驟
* [Set up your StorSimple Virtual Array as a file server (將 StorSimple 虛擬陣列設定為檔案伺服器)](storsimple-virtual-array-deploy3-fs-setup.md)
* [Set up your StorSimple Virtual Array as an iSCSI server (將 StorSimple 虛擬陣列設定為 iSCSI 伺服器)](storsimple-virtual-array-deploy3-iscsi-setup.md)

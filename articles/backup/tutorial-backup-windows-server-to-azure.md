---
title: 教學課程 - 將 Windows Server 備份至 Azure
description: 本教學會詳細說明如何將內部部署 Windows Server 備份至復原服務保存庫。
ms.topic: tutorial
ms.date: 08/22/2018
ms.custom: mvc
ms.openlocfilehash: d2990b5950cf8812367c3a59c6cace39e4085e2a
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88261900"
---
# <a name="back-up-windows-server-to-azure"></a>將 Windows Server 備份到 Azure

您可以使用 Azure 備份來保護您的 Windows Server，使其免受損毀、攻擊以及災害的威脅。 Azure 備份提供了一種輕量型工具，稱為「Microsoft Azure 復原服務 (MARS) 代理程式」。 MARS 代理程式會安裝在 Windows Server 上，保護檔案及資料夾，並透過 Windows Server 系統狀態保護伺服器設定。 本教學會解釋如何使用 MARS 代理程式來將您的 Windows Server 備份到 Azure。 在本教學課程中，您將了解如何：

> [!div class="checklist"]
>
> * 下載並設定 MARS 代理程式
> * 為您的伺服器備份設定備份時間和保留排程
> * 執行隨選備份

## <a name="sign-in-to-azure"></a>登入 Azure

在 <https://portal.azure.com> 登入 Azure 入口網站。

## <a name="create-a-recovery-services-vault"></a>建立復原服務保存庫

在您備份 Windows Server 之前，您必須先為備份建立一個位置，或建立一個要還原的還原點。 [復原服務保存庫](backup-azure-recovery-services-vault-overview.md)是 Azure 中儲存 Windows Server 備份的容器。 請遵循下列步驟來在 Azure 入口網站上建立復原服務保存庫。

1. 在左側功能表中，選取 [所有服務]  ，然後在服務清單中鍵入**復原服務**。 選取 [復原服務保存庫]。

   ![開啟復原服務保存庫](./media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)

2. 在 [復原服務保存庫]**** 功能表上，選取 [新增]****。

   ![提供保存庫的資訊](./media/tutorial-backup-windows-server-to-azure/provide-vault-detail-2.png)

3. 在 [復原服務保存庫]  功能表中，

    * 在 [名稱] 中鍵入 *myRecoveryServicesVault*。
    * 目前的訂用帳戶識別碼會出現在 [訂用帳戶]  中。
    * 針對**資源群組**：選取 [使用現有]  並選擇 [myResourceGroup]  。 如果 [myResourceGroup]  不存在，請選取 [新建]  並鍵入 *myResourceGroup*。
    * 從 [位置]  下拉式功能表中，選擇 [西歐]  。
    * 選取 [建立] 建立您的復原服務保存庫。

保存庫一旦建立好，就會出現在 [復原服務保存庫] 的清單中。

## <a name="download-recovery-services-agent"></a>下載復原服務代理程式

Microsoft Azure 復原服務 (MARS) 代理程式可在您的 Windows Server 及您的復原服務保存庫間建立關聯。 下列程序說明如何將代理程式下載到您的伺服器。

1. 在復原服務保存庫清單中，選取 [myRecoveryServicesVault]  來開啟其儀表板。

   ![選取保存庫以開啟儀表板](./media/tutorial-backup-windows-server-to-azure/open-vault-from-list.png)

2. 在保存庫儀表板功能表上，選取 [備份]****。

3. 在 [備份目標]  功能表中：

   * 針對 [工作負載的執行位置?]  ，選取 [內部部署] 
   * 針對 [您要備份什麼?]  ，選取 [檔案和資料夾]  ，以及 [系統狀態] 

   ![備份目標功能表](./media/tutorial-backup-windows-server-to-azure/backup-goal.png)

4. 選取 [準備基礎結構] 以開啟 [準備基礎結構] 功能表。

5. 在 [準備基礎結構] 功能表上，選取 [下載 Windows Server 或 Windows Client 的代理程式] 以下載 *MARSAgentInstaller.exe*。

    ![下載 Windows Server 或 Windows Client 的代理程式](./media/tutorial-backup-windows-server-to-azure/prepare-infrastructure.png)

    安裝程式會開啟另一個瀏覽器視窗，並下載 **MARSAgentInstaller.exe**。

6. 在您執行下載的檔案之前，請在 [準備基礎結構] 功能表上選取 [下載]，並儲存**保存庫認證**檔案。 必須要有保存庫認證，才能連接 MARS 代理程式與復原服務保存庫。

    ![下載保存庫認證](./media/tutorial-backup-windows-server-to-azure/download-vault-credentials.png)

## <a name="install-and-register-the-agent"></a>安裝和註冊代理程式

1. 找出並按兩下已下載的 **MARSagentinstaller.exe**。
2. **Microsoft Azure 復原服務代理程式安裝精靈** 便會出現。 當您執行精靈時，請在程式提示的時候提供下列資訊，然後選取 [註冊]。
   * 安裝及快取資料夾位置。
   * Proxy 伺服器資訊 (若您使用 Proxy 伺服器來連線到網際網路的話)。
   * 您的使用者名稱及密碼詳細資料 (若您使用驗證 Proxy 的話)

     ![Microsoft Azure 復原服務代理程度設定精靈](./media/tutorial-backup-windows-server-to-azure/mars-installer.png)

3. 在精靈結束時，選取 [繼續註冊] 並提供您在先前步驟中下載的**保存庫認證**檔案。

4. 出現提示時，請提供加密複雜密碼來加密 Windows Server 的備份。 將複雜密碼儲存在安全的位置。一旦不慎遺失，Microsoft 將無法復原複雜密碼。

5. 選取 [完成]。

## <a name="configure-backup-and-retention"></a>設定備份和保留

您可以使用 Microsoft Azure 復原服務代理程式來排程 Windows Server 何時要備份至 Azure。 在您下載代理程式的伺服器上執行下列步驟。

1. 開啟 Microsoft Azure 復原服務代理程式。 您可以透過在您的電腦中搜尋 **Microsoft Azure 備份**來找出備份。

2. 在 [復原服務代理程式] 主控台中，選取 [動作窗格] 下的 [排程備份]。

    ![排程備份](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

3. 選取 [下一步] 來巡覽至 [elect Items to Back up] \(選取要備份之項目) 頁面。

4. 選取 [新增項目]，並在開啟的對話方塊中，選取 [系統狀態] 及您要備份的檔案和資料夾。 然後選取 [確定]。

5. 選取 [下一步]  。

6. 在 [指定備份排程 (系統狀態)] 頁面上，為系統狀態指定要觸發備份的一天或一週當中的時間，然後選取 [下一步]。

7. 在 [選取保留原則 (系統狀態)] 頁面上，為系統狀態的備份複本選取保留原則，然後選取 [下一步]。

8. 同樣地，為選取的檔案和資料夾選取備份排程及保留原則。

9. 在 [選擇初始備份類型] 頁面上，選取 [自動透過網路]，然後選取 [下一步]。

10. 在 [確認] 頁面上檢閱資訊，然後選取 [完成]。

11. 當精靈建立好備份排程時，請選取 [關閉] 。

## <a name="perform-an-on-demand-backup"></a>執行隨選備份

您已建立備份工作執行的排程。 然而，您尚未備份至伺服器。 災害復原的最佳做法是執行隨選備份，以確保您伺服器的資料復原。

1. 在 Microsoft Azure 復原服務代理程式主控台中，選取 [立即備份]。

    ![立即備份](./media/tutorial-backup-windows-server-to-azure/backup-now.png)

2. 在 [立即備份] 上，從您要備份的 [檔案和資料夾] 或 [系統狀態] 中選取一個項目，然後選取 [下一步]
3. 在 [確認]  頁面上，檢閱 [立即備份精靈]  要用來備份您伺服器的設定。 然後選取 [備份]。
4. 選取 [關閉]  即可關閉精靈。 如果您在備份程序完成之前關閉精靈，精靈會繼續在背景中執行。
5. 完成初始備份之後，MARS 代理程式主控台中的 [工作]  窗格會顯示 [工作已完成]  狀態。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已使用 Azure 入口網站來：

> [!div class="checklist"]
>
> * 建立復原服務保存庫
> * 下載 Microsoft Azure 復原服務代理程式
> * 安裝代理程式
> * 設定 Windows Server 的備份
> * 執行隨選備份

繼續下一個教學課程，從 Azure 復原檔案到 Windows Server

> [!div class="nextstepaction"]
> [將檔案從 Azure 復原至 Windows Server](./tutorial-backup-restore-files-windows-server.md)

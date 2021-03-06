---
title: Azure 檔案共用備份的支援矩陣
description: 摘要說明備份 Azure 檔案共用時的支援設定和限制。
ms.topic: conceptual
ms.date: 5/07/2020
ms.custom: references_regions
ms.openlocfilehash: 6381170df93fdf52c2d0dc7059ad47bbff734025
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89378027"
---
# <a name="support-matrix-for-azure-file-share-backup"></a>Azure 檔案共用備份的支援矩陣

您可以使用 [Azure 備份服務](./backup-overview.md)來備份 Azure 檔案共用。 本文將摘要說明使用 Azure 備份來備份 Azure 檔案共用時的支援設定與限制。

## <a name="supported-regions"></a>支援區域

### <a name="ga-regions-for-azure-file-shares-backup"></a>Azure 檔案共用備份的 GA 區域

Azure 檔案共用備份適用于所有區域， **除了** ：德國中部 (主權) 、德國東北部 (主權) 、中國東部、中國東部2、中國北部、中國北部 2 US Gov 愛荷華州

## <a name="supported-storage-accounts"></a>支援的儲存體帳戶

| 儲存體帳戶詳細資料 | 支援                                                      |
| ------------------------ | ------------------------------------------------------------ |
| 帳戶種類            | Azure 備份支援存在於一般用途 v1、一般用途 v2 和檔案儲存體類型儲存體帳戶中的 Azure 檔案共用 |
| 效能              | Azure 備份支援標準和進階儲存體帳戶中的檔案共用 |
| 複寫              | 支援具有任何複寫類型之儲存體帳戶中的 Azure 檔案共用 |
| 已啟用防火牆         | 支援儲存體帳戶中的 Azure 檔案共用，具有允許 Microsoft Azure 服務存取儲存體帳戶的防火牆規則|

## <a name="supported-file-shares"></a>支援的檔案共用

| 檔案共用類型                                   | 支援   |
| -------------------------------------------------- | --------- |
| 標準                                           | 支援 |
| 大型                                              | 支援 |
| Premium                                            | 支援 |
| 與 Azure 檔案同步服務連線的檔案共用 | 支援 |

## <a name="protection-limits"></a>保護限制

| 設定                                                      | 限制 |
| ------------------------------------------------------------ | ----- |
| 每個保存庫每天可以保護的檔案共用數目上限| 200   |
| 每個保存庫每天可註冊的儲存體帳戶數目上限 | 50    |
| 每個保存庫可以保護的檔案共用數目上限 | 2000   |
| 每個保存庫可以註冊的儲存體帳戶數目上限 | 200   |

## <a name="backup-limits"></a>備份限制

| 設定                                      | 限制 |
| -------------------------------------------- | ----- |
| 每天的隨選備份數目上限 | 10   |
| 每天的排程備份數目上限 | 1     |

## <a name="restore-limits"></a>還原限制

| 設定                                                      | 限制   |
| ------------------------------------------------------------ | ------- |
| 每天的還原數目上限                           | 10      |
| 每個還原的檔案數目上限                         | 10      |
| 大型檔案共用每個還原建議的還原大小上限 | 15 TiB |

## <a name="retention-limits"></a>保留期限

| 設定                                                      | 限制    |
| ------------------------------------------------------------ | -------- |
| 每個檔案共用在任何時間點的復原點總計上限 | 200      |
| 隨選備份所建立的復原點保留期上限 | 10 年 |
| 每個檔案共用的每日復原點 (快照集) 的保留期上限| 200 天 |
| 每個檔案共用的每週復原點 (快照集) 的保留期上限 | 200 週 |
| 每個檔案共用的每月復原點 (快照集) 的保留期上限 | 120 個月 |
| 每個檔案共用的每年復原點 (快照集) 的保留期上限 | 10 年 |

## <a name="supported-restore-methods"></a>支援的還原方法

| 還原方法     | 詳細資料                                                      |
| ------------------ | ------------------------------------------------------------ |
| 完整共用還原 | 您可以將完整的檔案共用還原至原始位置或替代位置 |
| 項目層級還原 | 您可以將個別檔案和資料夾還原到原始位置或替代位置 |

## <a name="next-steps"></a>後續步驟

* 了解如何[備份 Azure 檔案共用](backup-afs.md)
* 了解如何[還原 Azure 檔案共用](restore-afs.md)
* 了解如何[管理 Azure 檔案共用備份](manage-afs-backup.md)

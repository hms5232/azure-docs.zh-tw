---
title: Azure Arc 啟用的伺服器 (preview) 代理程式的新功能
description: 本文提供 Azure Arc 啟用的伺服器 (preview) 代理程式的版本資訊。 針對許多摘要問題，有其他詳細資料的連結。
ms.topic: conceptual
ms.date: 09/14/2020
ms.openlocfilehash: 3b739401603f6dc18b9f48fb3cb6e28023a051ab
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532166"
---
# <a name="whats-new-with-azure-arc-enabled-servers-preview-agent"></a>Azure Arc 啟用的伺服器 (preview) 代理程式的新功能

Azure Arc 啟用的伺服器 (預覽版) 連線的機器代理程式會持續獲得改進。 為了讓您隨時掌握最新的開發訊息，本文提供下列相關資訊：

- 最新版本
- 已知問題
- 錯誤修正

## <a name="september-2020"></a>2020 年 9 月

版本： 1.0 (正式推出) 

### <a name="plan-for-change"></a>針對變更做規劃

- 在未來的服務更新中，將會移除對預覽代理程式 (所有早于 1.0) 的版本的支援。
- 已移除對 fallback 端點的支援 `.azure-automation.net` 。 如果您有 proxy，則需要允許端點 `*.his.arc.azure.com` 。
- 如果連線的機器代理程式是安裝在裝載于 Azure 的虛擬機器上，則無法從已啟用 Arc 的伺服器資源安裝或修改 VM 擴充功能。 這是為了避免從虛擬機器的 **Microsoft. Compute** 和 **HybridCompute** 資源執行的延伸模組作業發生衝突。 使用適用于機器的 **Microsoft 計算** 資源來進行所有擴充作業。
- 來賓設定程式的名稱已變更，從 *gcd* 至 Linux 上的 *gcad* ，然後 *gcservice* 至 Windows 上的 *gcarcservice* 。

### <a name="new-feature"></a>新功能

- 已新增 `azcmagent logs` 選項來收集支援的資訊。
- 已新增 `azcmagent license` 顯示 EULA 的選項。
- 已新增 `azcmagent show --json` 可輕鬆剖析或是格式的輸出代理程式狀態選項。
- 在輸出中新增旗 `azcmagent show` 標，以指出伺服器是否位於 Azure 中託管的虛擬機器上。
- 已新增 `azcmagent disconnect --force-local-only` 選項，以允許在無法連線到 Azure 服務時重設本機代理程式狀態。
- 已新增 `azcmagent connect --cloud` 支援其他雲端的選項。 在此版本中，代理程式發行時，服務只支援 Azure。
- 代理程式已當地語系化為支援 Azure 的語言。

### <a name="fixed"></a>固定式

- 連線能力檢查的增強功能。
- 已更正升級 Linux 上的代理程式時遺失的 proxy 伺服器設定問題。
- 解決嘗試在執行 Windows Server 2012 R2 的伺服器上安裝代理程式時所發生的問題。
- 擴充功能安裝可靠性的增強功能

## <a name="august-2020"></a>2020 年 8 月

版本：0.11

- 此版本先前宣佈支援 Ubuntu 20.04。 因為某些 Azure VM 擴充功能不支援 Ubuntu 20.04，所以會移除對此版本 Ubuntu 的支援。

- 延伸模組部署的可靠性改進。

### <a name="known-issues"></a>已知問題

如果您使用舊版的 Linux 代理程式並將它設定為使用 proxy 伺服器，則需要在升級之後重新設定 proxy 伺服器設定。 若要這樣做，請執行 `sudo azcmagent_proxy add http://proxyserver.local:83`。

## <a name="next-steps"></a>後續步驟

在為多部混合式電腦評估或啟用已啟用 Arc 的伺服器 (預覽) 之前，請參閱[連線的電腦代理程式概觀](agent-overview.md)來了解需求、代理程式的相關技術詳細資料，以及部署方法。
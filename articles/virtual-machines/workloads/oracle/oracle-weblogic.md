---
title: Azure 解決方案上的 Oracle WebLogic Server
description: 了解如何在 Microsoft Azure 上執行 Oracle WebLogic Server。
services: virtual-machines-linux
documentationcenter: ''
author: rezar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2020
ms.author: rezar
ms.openlocfilehash: e408f9e245fb78b475a194bc0db6f1edfdf85b41
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90069711"
---
# <a name="solutions-for-running-oracle-weblogic-server-on-azure"></a>在 Azure 上執行 Oracle WebLogic Server 的解決方案

本頁面說明在 Azure 虛擬機器上執行 Oracle WebLogic Server (WLS) 的解決方案。 這些解決方案是由 Oracle 和 Microsoft 共同開發。

WLS 是領先業界的 JAVA 應用程式伺服器，可執行全球最多工關鍵性企業 JAVA 應用程式。 WLS 形成 Oracle software suite 的中介軟體基礎。 Oracle 和 Microsoft 致力於讓 WLS 客戶享有選擇和彈性，以便在 Azure 上以領先的雲端平臺執行工作負載。

Azure WLS 解決方案的目標是讓您能夠輕鬆地將您的 JAVA EE 應用程式移至 Azure 虛擬機器，並將大部分的未定案作業自動化。 解決方案會自動布建虛擬網路、儲存體、JAVA 和 Linux 資源。 WebLogic Server 會以極短的時間來安裝。 解決方案可以設定網路安全性群組的安全性、使用 Azure App 閘道進行負載平衡，以及使用 Azure Active Directory 進行驗證。 您也可以自動連接到您現有的資料庫，包括 Azure 于 postgresql、Azure SQL，以及 Oracle 雲端或 Azure 上的 Oracle DB。 解決方案的藍圖包含透過 Oracle 連貫性啟用分散式記錄和分散式快取的功能。 Microsoft 和 Oracle 合作，為 WebLogic 和 Azure Kubernetes Service (AKS) 啟用類似的功能。

:::image type="content" source="media/oracle-weblogic/wls-on-azure.gif" alt-text="您可以使用 Azure 入口網站在 Azure 上部署 WebLogic Server":::

有四個供應專案可滿足不同的案例：單一節點沒有管理伺服器、具有管理伺服器、叢集和動態叢集的單一節點。 供應專案免費提供。 這些供應專案的描述與連結如下。

_這些供應項目是自備授權_。 他們假設您已經使用 Oracle 取得適當的授權，並已獲得適當授權，可在 Azure 中執行供應專案。

這些供應專案透過基礎映射 (（例如 WebLogic Server 14 以及 Oracle Linux 7.6) 上的 JDK 11）來支援各種作業系統、JAVA 及 WLS 版本。 這些基底映射也可在 Azure 上使用。 基底映射適用于需要複雜自訂 Azure 部署的客戶。 您可以在 [這裡](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=WebLogic%20Server%20Base%20Image&page=1)找到目前的基底映射集。

_如果您想要與開發這些供應專案的工程小組密切合作來處理您的遷移案例， [CONTACT ME](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview)請_在[marketplace 供應專案總覽頁面](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview)上選取 [連絡人] 按鈕。 計畫經理、架構設計人員和工程師會馬上接觸您，並開始密切合作。 當供應專案正在進行開發時，在遷移案例上共同作業的機會是免費的。

## <a name="oracle-weblogic-server-single-node"></a>Oracle WebLogic Server 單一節點

此供應專案會布建單一虛擬機器，並在其上安裝 WLS。 它不會建立網域或啟動管理伺服器。 單一節點供應專案適用于具有高度自訂網域設定的案例。

## <a name="oracle-weblogic-server-with-admin-server"></a>具管理伺服器的 Oracle WebLogic Server

此供應專案會布建單一虛擬機器，並在其上安裝 WLS。 它會建立一個網域，並啟動管理伺服器。 您可以立即管理網域，並立即開始使用應用程式部署。

## <a name="oracle-weblogic-server-cluster"></a>Oracle WebLogic Server 叢集

這項供應專案會建立 WLS 虛擬機器的高可用性叢集。 系統會根據預設啟動管理伺服器和所有受管理的伺服器。 您可以立即管理叢集，並立即開始使用高可用性應用程式。

## <a name="oracle-weblogic-server-dynamic-cluster"></a>Oracle WebLogic Server 動態叢集

這項供應專案會建立高可用性且可擴充的 WLS 虛擬機器動態叢集。 系統會根據預設啟動管理伺服器和所有受管理的伺服器。

這些解決方案將可讓您以相對簡單的方式啟用各式各樣的生產環境就緒部署架構。 您可以藉由允許專注于商務應用程式開發，以最有效率的方式來滿足大部分的遷移案例。

:::image type="content" source="media/oracle-weblogic/weblogic-architecture-vms.png" alt-text="在 Azure 上啟用複雜的 WebLogic 伺服器部署":::

除了解決方案自動布建的功能之外，客戶還可以完整彈性地自訂其部署。 可能是在部署應用程式的最上層，客戶會將進一步的 Azure 資源與其部署整合。 建議客戶提供進一步改進解決方案的意見反應。

## <a name="next-steps"></a>後續步驟

探索 Azure 上的優惠。

> [!div class="nextstepaction"]
> [Oracle WebLogic Server 單一節點](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls)

> [!div class="nextstepaction"]
> [具管理伺服器的 Oracle WebLogic Server](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server 叢集](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server 動態叢集](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster)

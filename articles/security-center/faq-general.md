---
title: Azure 資訊安全中心常見問題-一般問題
description: 關於 Azure 資訊安全中心的常見問題，這是可協助您預防、偵測及回應威脅的產品
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 782884f53f0ec17c47b79cf66504d0e3ad3fc29d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84783209"
---
# <a name="faq---general-questions-about-azure-security-center"></a>常見問題-關於 Azure 資訊安全中心的一般問題

## <a name="what-is-azure-security-center"></a>什麼是 Azure 資訊安全中心？
Azure 資訊安全中心可協助您透過更深入的可見度及控制資源的安全性，來防止、偵測及回應威脅。 它提供您訂用帳戶之間的整合式安全性監視和原則管理，協助您偵測可能會忽略的威脅，且適用於廣泛的安全性解決方案生態系統。

資訊安全中心使用 Log Analytics 代理程式來收集和儲存資料。 如需深入詳細資訊，請參閱[Azure 資訊安全中心中的資料收集](security-center-enable-data-collection.md)。


## <a name="how-do-i-get-azure-security-center"></a>我要如何取得 Azure 資訊安全中心？
「Azure 資訊安全中心」是藉由您的 Microsoft Azure 訂用帳戶啟用，並可從 [Azure 入口網站](https://azure.microsoft.com/features/azure-portal/)存取。 若要存取它，請登[入入口網站](https://portal.azure.com)，選取 **[流覽]**，然後滾動到**資訊安全中心**。


## <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Azure 資訊安全中心會監視哪些 Azure 資源？
Azure 資訊安全中心會監視下列 Azure 資源：

* 虛擬機器（Vm）（包括[雲端服務](../cloud-services/cloud-services-choose-me.md)）
* 虛擬機器擴展集
* 與您的 Azure 訂用帳戶整合的合作夥伴解決方案，例如 VM 和 App Service 環境上的 Web 應用程式防火牆
* [產品總覽中列出的許多 Azure PaaS 服務](features-paas.md)


## <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>我要如何查看 Azure 資源目前的安全性狀態？
[**資訊安全中心總覽**] 頁面會顯示您環境的整體安全性狀態，並依計算、網路、儲存體 & 資料和應用程式細分。 每個資源類型都有指示器，顯示已識別的安全性弱點。 按一下每個圖格都會顯示資訊安全中心所識別的安全性問題，以及訂用帳戶內資源的詳細目錄。



## <a name="what-is-a-security-policy"></a>什麼是安全性原則？
安全性原則可針對指定之訂用帳戶內的資源，定義一組建議的控制機制。 在「Azure 資訊安全中心」中，您可以根據公司的安全性需求，以及每個訂用帳戶中應用程式的類型或資料的機密性，為您的 Azure 訂用帳戶定義原則。

「Azure 資訊安全中心」中啟用的安全性原則將會決定安全性建議與監視。 若要深入了解安全性原則，請參閱 [Azure 資訊安全中心的安全性健康情況監視](security-center-monitoring.md)。


## <a name="who-can-modify-a-security-policy"></a>誰可以修改安全性原則？
若要修改安全性原則，您必須是**安全性系統管理員**或該訂用帳戶的**擁有**者。

若要了解如何設定安全性原則，請參閱[在 Azure 資訊安全中心設定安全性原則](tutorial-security-policy.md)。


## <a name="what-is-a-security-recommendation"></a>什麼是安全性建議？
「Azure 資訊安全中心」會分析 Azure 資源的安全性狀態。 當發現潛在的安全性弱點時，就會產生相關建議。 這些建議會引導您完成設定所需控制項的程序。 範例包括：

* 佈建反惡意程式碼，以協助識別及移除惡意軟體
* [網路安全性群組](../virtual-network/security-overview.md)與規則，以控制對虛擬機器的流量
* 佈建 Web 應用程式防火牆，以協助抵禦以 Web 應用程式為目標的攻擊
* 部署遺漏的系統更新
* 處理不符合建議基準的作業系統組態

這裡只會顯示 [安全性原則] 中已啟用的建議。



## <a name="what-triggers-a-security-alert"></a>什麼會觸發安全性警示？
「Azure 資訊安全中心」會自動收集、分析及整合下列來源的記錄檔資料：Azure 資源、網路，以及反惡意程式碼和防火牆等合作夥伴解決方案。 偵測到威脅時，會建立安全性警示。 偵測範例包括：

* 已受到危害的虛擬機器與已知的惡意 IP 位址進行通訊
* 使用 Windows 錯誤報告偵測到進階的惡意程式碼
* 針對虛擬機器的暴力密碼破解攻擊
* 來自已整合的合作夥伴安全性解決方案 (例如「反惡意程式碼」或「Web 應用程式防火牆」) 的安全性警示


## <a name="why-did-secure-score-values-change"></a>為什麼安全分數值會變更？ <a name="secure-score-faq"></a>
從2019年2月開始，資訊安全中心調整一些建議的分數，以更符合其嚴重性。 由於這項調整，整體安全分數值可能會有所變更。  如需安全分數的詳細資訊，請參閱[Azure 資訊安全中心中的增強型安全分數](secure-score-security-controls.md)。


## <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>在 Microsoft Security Response Center 與 Azure 資訊安全中心，偵測到威脅和收到警示有何差異？
Microsoft Security Response Center (MSRC) 執行 Azure 網路和基礎結構的選取安全性監視，並接收來自協力廠商的威脅情報和濫用客訴。 當 MSRC 察覺到有非法或未經授權的合作對象存取客戶資料，或有客戶不遵守可接受用途的條款使用 Azure，安全性事件管理員就會通知客戶。 通知方式通常是傳送電子郵件給 Azure 資訊安全中心中指定的安全性連絡人，如未指定安全性連絡人，則通知 Azure 訂用帳戶擁有者。

資訊安全中心是一項 Azure 服務，它會持續監視客戶的 Azure 環境，以及套用分析自動偵測廣泛的潛在惡意活動。 這些偵測會顯示為資訊安全中心儀表板中的安全性警示。
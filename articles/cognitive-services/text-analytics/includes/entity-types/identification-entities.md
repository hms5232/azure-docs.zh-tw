---
title: 識別實體
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: 8e0798f75aaa79031ca7cc03814282daa049fbfe
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89662817"
---
此實體類別包含財務資訊和官方形式的識別。 從模型版本開始提供 `2019-10-01` 。 子類型如下所示。 

### <a name="financial-account-identification"></a>財務帳戶識別

| 子類型名稱               | 描述                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA 銀行代號        | 美國銀行家協會 (ABA) 傳輸路線號碼。                  |
| SWIFT 代碼                 | 付款指示資訊的 SWIFT 代碼。                           |
| 信用卡                | 信用卡號碼。                                                       |
| 國際銀行帳戶號碼 (IBAN)                  | 付款指示資訊的 IBAN 代碼。                            |


### <a name="government-and-countryregion-specific-identification"></a>政府和國家/地區特定的識別

> [!NOTE]
> 下列財務和國家特定的實體不會隨參數一起傳回 `domain=phi` ：
> * Passport 號碼
> * 稅務識別碼

下列實體會依國家/地區分組和列出：

阿根廷
* 阿根廷國家身分識別 (DNI) 號碼

奧地利
* 奧地利身分識別卡
* 奧地利稅務識別編號
* 奧地利加值稅 (加值稅) Number

澳洲
* 澳大利亞銀行帳戶號碼
* 澳大利亞商業編號
* 澳大利亞公司編號
* 澳大利亞駕駛的授權號碼
* 澳大利亞醫療帳戶號碼
* 澳大利亞 Passport 號碼
* 澳大利亞稅檔案編號

比利時
* 比利時全國電話號碼
* 已新增比利時加值稅號碼

巴西 
* 巴西法律實體編號 (CNPJ) 
* 巴西 CPF 號碼
* 巴西國家識別碼卡 (RG) 

保加利亞
* 保加利亞統一的民事號碼

加拿大
* 加拿大銀行帳戶號碼
* 加拿大駕駛的授權號碼
* 加拿大健全狀況服務號碼
* 加拿大 Passport 號碼
* 加拿大個人健康識別號碼 (PHIN) 
* 加拿大社會保險編號

智利
* 身分識別卡號碼 

中國
* 中國居民身分識別卡 (PRC) 號碼

克羅埃西亞
* 克羅地亞身分識別卡號碼
* 克羅地亞國家識別碼卡號碼
* 克羅地亞個人標識 (OIB) 號碼

賽普勒斯
* 賽普勒斯身分識別卡編號
* 賽普勒斯稅務識別編號

捷克共和國
* 捷克文個人標識編號

丹麥
* 丹麥個人識別碼

愛沙尼亞
* 愛沙尼亞個人識別碼

歐盟 (EU) 
* EU 轉帳卡編號
* 歐盟駕照號碼
* 歐盟國家/地區身分證字號
* 歐盟護照號碼
* 歐盟社會安全號碼 (SSN) 或對等識別碼
* 歐盟稅務識別編號 (TIN)

芬蘭
* 芬蘭歐洲醫療保險編號
* 芬蘭國識別碼
* 芬蘭 Passport 號碼

法國
* 法國駕駛的授權號碼
* 法國醫療保險編號
* 法國國家（地區）識別碼卡 (CNI) 
* 法國 Passport 號碼
* 法國社會安全號碼 (INSEE) 
* 法國稅務識別編號 (Numéro SPI) 
* 法國加值稅號碼

德國
* 德文駕駛的授權號碼
* 德國身分識別卡編號
* 德文 Passport 號碼
* 德國稅務識別編號
* 德國加值稅號碼

希臘 
* 希臘國家識別碼卡片編號
* 希臘稅識別編號

香港
* 香港特別行政區身分識別卡 (HKID) 號碼

匈牙利
* 匈牙利國辨識編號
* 匈牙利稅識別編號
* 已新增匈牙利加值稅號碼

印度
* 印度永久帳戶號碼 (平移) 
* 印度唯一識別 (Aadhaar) 號碼

印尼
* 印尼身分識別卡 (KTP) 號碼

愛爾蘭
* 愛爾蘭個人公用服務 (PPS) 號碼

以色列
* 以色列國家識別碼
* 以色列銀行帳戶號碼

義大利
* 義大利駕駛的授權識別碼
* 義大利會計代碼
* 義大利加值稅號

日本
* 日本銀行帳戶號碼
* 日本駕駛的授權號碼
* 日文我的個人號碼
* 日文 My Number 公司
* 日本居民註冊號碼
* 日本居留證卡號
* 日本社會保險號碼 (SIN) 
* 日本 Passport 號碼

拉脫維亞
* 拉脫維亞個人代碼

立陶宛
* 立陶宛個人代碼

盧森堡
* 盧森堡身分識別 (自然人員) 
* 盧森堡 (非自然人員的國家/地區識別碼) 

馬來西亞
* 馬來西亞身分識別卡號碼

馬爾他
* 馬爾他身分識別卡編號
* 馬爾他稅識別編號

荷蘭
* 荷蘭公民服務 (BSN) 號碼
* 荷蘭稅務識別編號
* 荷蘭加值稅號

紐西蘭
* 紐西蘭銀行帳戶號碼
* 新的紐西蘭駕駛授權號碼
* 紐西蘭 Inland 收入號碼
* 新的紐約的健康情況號碼
* 紐西蘭社會福利號碼

挪威
* 挪威識別編號

菲律賓
* 菲律賓統一的多用途識別碼

波蘭
* 波蘭身分識別卡
* 波蘭國家識別碼 (PESEL) 
* 波蘭 Passport 號碼
* 波蘭 REGON 號碼
* 波蘭稅務識別編號

葡萄牙 
* 葡萄牙公民卡片編號
* 葡萄牙稅識別編號

羅馬尼亞
*  (CNP) 羅馬尼亞個人的數位代碼

俄羅斯
* 俄文 Passport 號碼 (國內) 
* 俄文 Passport 號碼 (國際) 

沙烏地阿拉伯
* 沙烏地阿拉伯國識別碼

新加坡
* 新加坡國家註冊識別碼卡 (NRIC) 號碼

斯洛伐克 
* 斯洛伐克個人號碼

斯洛維尼亞
* 斯洛維尼亞稅務識別編號
* 斯洛維尼亞唯一主要公民號碼

南非
* 南非識別碼

南韓
* 韓國居民註冊號碼

西班牙 
* 西班牙 DNI
* 西班牙社會安全號碼 (SSN) 
* 西班牙稅務識別編號

瑞典
* 瑞典國識別碼
* 瑞典 Passport 號碼
* 瑞典稅識別編號

瑞士
* 瑞士社會安全號碼 AHV

台灣 
* 臺灣國家識別碼
* 臺灣居民憑證 (ARC/TARC) 
* 臺灣 Passport 號碼

泰國
* 泰國人口識別碼

土耳其
* 土耳其文的國家/地區識別碼

烏克蘭
*  (國內) 的烏克蘭版 Passport 號碼
*  (國際) 的烏克蘭版 Passport 號碼

英國
* 英國。 驅動程式的授權號碼
* 英國。 選舉的滾動編號
* 英國。 國家健全狀況服務 (NHS) 號碼
* 英國。 全國保險號碼 (NINO) 
* 英國。 Passport 號碼
* 英國。 唯一的納稅人參考編號

美國
* 美國社會安全號碼 (SSN) 
* 美國駕駛的授權號碼
* 美國 Passport 號碼
* 美國個別的納稅人識別碼 (ITIN) 
* 美國藥品機關 (DEA) 號碼
* 美國銀行帳戶號碼

---
title: Microsoft Authenticator 應用程式的問題與答案 - Azure AD
description: Microsoft Authentication 應用程式與雙因素驗證的常見問題集 (FAQ)。
services: active-directory
author: curtand
manager: daveba
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 07/16/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 051d88494049662891e1891f900aa580a005ffe4
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2020
ms.locfileid: "88799463"
---
# <a name="frequently-asked-questions-faq-about-the-microsoft-authenticator-app"></a>常見問題 (有關 Microsoft Authenticator 應用程式的常見問題) 

本文將回答有關 Microsoft Authenticator 應用程式的常見問題。 如果您沒有看到您問題的解答，請移至 [Microsoft 驗證器應用程式論壇](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp)。

Microsoft Authenticator 應用程式取代了 Azure 驗證器應用程式，當您使用 Azure Multi-Factor Authentication 時，這是建議的應用程式。 Microsoft Authenticator 應用程式適用於 [Android](https://app.adjust.com/e3rxkc_7lfdtm?fallback=https%3A%2F%2Fplay.google.com%2Fstore%2Fapps%2Fdetails%3Fid%3Dcom.azure.authenticator) 和 [iOS](https://app.adjust.com/e3rxkc_7lfdtm?fallback=https%3A%2F%2Fitunes.apple.com%2Fus%2Fapp%2Fmicrosoft-authenticator%2Fid983156458)。

## <a name="frequently-asked-questions"></a>常見問題集

| 問題 | Answer |
| -------- | ------ |
| 註冊裝置是否即表示同意公司或服務存取我的裝置？ | 註冊裝置可讓您的裝置存取您組織的服務，但不允許您組織存取您的裝置。 |
| 什麼是應用程式鎖定，如何使用它來協助保護我的安全？ | 應用程式鎖定有助於保持單次密碼、應用程式資訊和應用程式設定的安全。 當應用程式鎖定啟用時，系統會要求您在每次開啟驗證器時，使用您的裝置 PIN 或生物特徵辨識進行驗證。 應用程式鎖定也可以在您核准登入通知時，藉由提示您的 PIN 或生物特徵辨識，來確保您是唯一可以核准通知的人。 您可以在 [驗證器設定] 頁面上開啟或關閉應用程式鎖定。 依預設，當您在裝置上設定 PIN 或生物特徵辨識時，應用程式鎖定會開啟。<br><br>可惜的是，不保證應用程式鎖定將會阻止某人存取驗證器。 這是因為裝置註冊可能會發生在驗證器以外的其他位置，例如 Android 帳戶設定或在公司入口網站應用程式中。 |
| 我有 Windows Mobile 裝置，而且 Windows Mobile 上的 Microsoft Authenticator 已被取代。 我可以繼續使用應用程式進行驗證嗎？ | 使用 Windows Mobile Microsoft Authenticator 的所有驗證將于2020年7月15日之後淘汰。 強烈建議您使用替代的驗證方法，以避免您的帳戶遭到鎖定。<br>企業使用者的替代選項包括：<br><ul><li>設定 [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator) 或 [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458)的 Microsoft Authenticator。</li><li>[設定 SMS](multi-factor-authentication-setup-phone-number.md) 以接收驗證碼。</li><li>設定電話號碼來接收 [電話以驗證其身分識別](multi-factor-authentication-setup-office-phone.md)。</li></ul><br>個人 Microsoft 帳戶使用者的替代選項包括：<br><ul><li>設定 [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator) 或 [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458)的 Microsoft Authenticator。</li><li>藉由從 [Microsoft 帳戶安全性頁面](https://account.microsoft.com/security/)更新您的安全性資訊，設定 (SMS 或電子郵件) 的替代登入方法。</li></ul> |
| 我可以在 Android 驗證器上取得我的單次密碼 (OTP) 代碼的螢幕擷取畫面嗎？ | 從驗證器 Android 的 release 6.2003.1704 開始，每當取得驗證器的螢幕擷取畫面時，預設會隱藏所有的 OTP 程式碼。 如果您想要在螢幕擷取畫面中查看 OTP 程式碼，或允許其他應用程式抓取驗證器畫面，您可以。 只需開啟驗證器中的 [ **螢幕** 快照] 設定，然後重新開機應用程式。 |
| Authenticator 會為我儲存何種資料，而我該如何刪除它？ | 驗證器應用程式會收集三種類型的資訊：<ul><li>您新增帳戶時提供的帳戶資訊。 這項資料可以藉由移除帳戶來移除。</li><li>在您選取應用程式之 [說明] 功能表中的 [傳送記錄]，將紀錄傳送給 Microsoft 之前，診斷記錄資料只會存留在應用程式中。 這些記錄可包含個人資料，例如電子郵件地址、伺服器位址或 IP 位址。 它們也可以包含裝置資料，例如裝置名稱和作業系統版本。 任何收集到的個人資料都僅限於協助疑難排解應用程式問題所需的資訊。 您可以隨時在應用程式中流覽這些記錄檔，以查看所收集的資訊。 如果您傳送記錄檔，驗證應用程式工程師只會使用這些檔案來針對客戶回報的問題進行疑難排解。</li><li>非個人的可識別使用量資料，例如「已啟動的新增帳戶流程/已成功新增的帳戶」或「已核准的通知。」 這項資料是我們工程決策不可或缺的一部分。 您的使用方式可協助我們判斷如何以對您很重要的方式來改善應用程式。 當您第一次使用應用程式時，您會看到此資料收集的通知。 它會通知您，然後您可以在應用程式的 [ **設定** ] 頁面上關閉它。 您可以隨時開啟或關閉此設定。</li></ul> |
| 應用程式中的程式碼有什麼作用？ | 當您開啟驗證器時，將會看到您新增的帳戶為圖格。 您的工作或學校帳戶及個人 Microsoft 帳戶將會在帳戶的全螢幕視圖中看到六個或八位數的數位， (藉由) 帳戶磚來存取。 針對其他帳戶，您會在應用程式的 [ **帳戶** ] 頁面中看到六個或八位數的數位。<br>您將使用這些程式碼作為單一使用密碼，以確認您是否為您所說的身分。 使用您的使用者名稱與密碼登入之後，您將鍵入與該帳戶相關聯的驗證碼。 例如，如果您要 Katy 登入您的 Contoso 帳戶，您可以按一下 [帳戶] 磚，然後使用驗證碼895823。 針對 Outlook 帳戶，您必須遵循相同的步驟。<br>點擊 [Contoso 帳戶] 磚。<br>![驗證器應用程式中的帳戶磚](media/user-help-auth-app-faq/katy-signin.png)<br>當您按一下 [Contoso 帳戶] 圖格之後，就可以在全螢幕中看到驗證碼。<br>![驗證器帳戶磚中的驗證碼](media/user-help-auth-app-faq/verification-code.png) |
| 為什麼驗證碼旁邊的數字會持續倒數計時？ | 您可能會看到 30 秒的計時器在作用中的驗證碼旁邊倒數計時。 此計時器讓您永遠不會使用同一個驗證碼登入兩次。 與密碼不同，我們不希望您記得此號碼。 其概念是，只有可存取您手機的人才會知道您的驗證碼。 |
| 為什麼我的帳戶磚會呈現灰色？ | 某些組織要求驗證器使用單一登入，並保護組織資源。 在此情況下，就無法使用帳戶進行雙步驟驗證，而且帳戶會顯示為灰色或非作用中狀態。 此類型的帳戶通常稱為「訊息代理程式」帳戶。 |
| 什麼是裝置註冊？ | 您的組織可能會要求您註冊裝置，以追蹤對受保護資源（例如檔案和應用程式）的存取。 其也可能會開啟 [條件式存取]，以降低存取這些資源時不想要發生的風險。 您可以在 [設定] 中取消註冊裝置，但您可能會無法存取 Outlook 的電子郵件及 OneDrive 的檔案，而且無法使用手機登入。 |
| 我是否需要連線到網際網路或我的網路，以取得並使用驗證碼？ | 驗證碼不需要您位於網際網路上或連線到資料，因此您不需要使用手機服務來登入。 此外，由於應用程式會在您關閉它之後立即停止執行，因此，它將不會耗盡您的電池。 |
| 我只有在開啟應用程式時才會收到通知。 不會在應用程式關閉時收到通知。 | 如果您會收到通知，但不會收到警示，即使已開啟響鈴，還是應該檢查您的應用程式設定。 請確定應用程式已開啟來針對通知使用音效或震動。 如果您沒有收到通知，您應該檢查下列條件：<ul><li>您的電話處於「請勿打擾」還是無訊息模式？ 這些模式讓應用程式無法傳送通知。</li><li>您可以收到來自其他應用程式的通知嗎？ 如果收不到，可能就是手機上的網路連線問題，或是來自 Android 或 Apple 的通知通道問題。 您可以嘗試透過電話設定來解析您的網路連線。 您可能需要與您的服務提供者討論，以協助 Android 或 Apple 通知通道。</li><li>您可以在應用程式上收到某些帳戶的通知，但收不到其他帳戶的通知嗎？ 如果是，請從您的應用程式移除有問題的帳戶、再次新增它以允許通知，然後查看是否能解決問題。</li></ul>如果您嘗試了上述所有步驟，但仍發生問題，建議您傳送記錄檔以進行診斷。 開啟應用程式、移至 [說明]，然後選取 [傳送記錄]。 之後，請移至 [Microsoft Authenticator 應用程式論壇](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) ，並告訴我們您所看到的問題和您嘗試的步驟。 |
| 我正在應用程式中使用驗證碼，但該如何切換到推播通知？ | 您可以設定您的工作或學校帳戶的通知 (如果您的系統管理員允許) 或您的個人 Microsoft 帳戶。 通知不適用於 Google 或 Facebook 這類協力廠商帳戶。<br>若要將您的個人帳戶切換到通知，您必須使用該帳戶重新註冊您的裝置。 移至 [新增帳戶]，選取 [個人 Microsoft 帳戶]，然後以使用者名稱和密碼登入。<br>您的組織會決定是否允許您的工作或學校帳戶使用單鍵通知。|
|通知適用於非 Microsoft 帳戶嗎 | 不適用，通知只適用於 Microsoft 帳戶與 Azure Active Directory 帳戶。 如果您的公司或學校使用 Azure AD 帳戶，他們可能會關閉此功能。 |
| 我取得新的裝置或已從備份還原我的裝置。 如何? 在驗證器中再次設定我的帳戶？ | 如果您在舊裝置上開啟 **雲端備份** ，您可以使用舊的備份，在新的 IOS 或 Android 裝置上復原您的帳號憑證。 如需詳細資訊，請參閱 [使用驗證器的備份和復原帳戶](user-help-auth-app-backup-recovery.md) 認證文章。 |
| 我遺失了我的裝置，或已移動至新的裝置。 如何確定通知不會繼續傳送到舊裝置？ | 將驗證器新增至新的裝置並不會自動從舊裝置移除應用程式。 縱使從舊裝置上刪除應用程式也不能完全刪除。 您必須從舊裝置中刪除應用程式，並告知 Microsoft 或您的組織忘記並取消註冊舊的裝置。<ul><li>**從使用個人 Microsoft 帳戶的裝置中移除應用程式。** 移至您[帳戶安全性](https://account.microsoft.com/security) 頁面上的雙步驟驗證區域，然後選擇關閉舊裝置的驗證。</li><li>**從使用公司或學校 Microsoft 帳戶的裝置中移除應用程式。** 移至您的 [MyApps 頁面](https://myapps.microsoft.com/) 或組織自訂入口網站的雙步驟驗證區域，關閉舊裝置的驗證。</li></ul> |
| 如何從應用程式移除帳戶？ | 針對您想要從應用程式移除的帳戶，請按一下 [帳戶] 磚，以全螢幕方式查看帳戶。 請按一下 [ **移除帳戶** ]，從應用程式中移除帳戶。<br>如果您有已向組織註冊的裝置，您可能需要額外的步驟來移除您的帳戶。 在這些裝置上，驗證器會自動註冊為裝置系統管理員。 如果您想要完全解除安裝應用程式，則需要先在應用程式設定中取消註冊應用程式。 |
| 為什麼應用程式要求這麼多權限？ | 以下是應用程式可能要求的完整權限清單，以及其使用該清單的方式。 您看到的特定權限會隨您的手機類型而異。<ul><li>**使用生物特徵辨識硬體。** 驗證身分識別時，部分公司和學校帳戶需要額外的 PIN。 應用程式需要您同意才能使用生物特徵辨識或臉部辨識，而不是輸入 PIN。</li><li>**相機。** 當您新增公司、學校或非 Microsoft 帳戶時，可用來掃描 QR 代碼。</li><li>**連絡人和手機。** 應用程式需要此許可權，才能在您的手機上搜尋公司或學校 Microsoft 帳戶，並將其新增至應用程式。</li><li>**SMS。** 用來確保您的電話號碼符合您第一次使用個人 Microsoft 帳戶登入時的記錄號碼。 我們會將文字訊息傳送到您安裝了應用程式的電話，其中包含6-8 位數的驗證碼。 您不需要尋找此程式碼並加以輸入，因為驗證器會自動在文字訊息中找到它。</li><li>**透過其他應用程式進行。** 您收到的通知會驗證您的身分識別，也會顯示在任何其他執行中的應用程式上。</li><li>**從網際網路接收資料。** 需要有此權限，才能傳送通知。</li><li>**防止手機進入休眠。** 如果您向組織註冊裝置，則組織可以在您的手機上變更這個原則。</li><li>**控制震動。** 您可以選擇是否要在收到驗證身分識別的通知時震動。</li><li>**使用指紋硬體。** 驗證身分識別時，部分公司和學校帳戶需要額外的 PIN。 為了簡化此程序，我們可讓您使用指紋，而不是輸入 PIN。</li><li> **檢視網路連線。** 當您新增 Microsoft 帳戶時，應用程式需要網路/網際網路連線。</li><li>**讀取儲存體的內容**。 只有在您透過應用程式設定回報技術問題時，才會使用這個權限。 會收集您儲存體中的部分資訊來診斷問題。</li><li>**完整的網路存取權。** 需要有此權限，才能傳送驗證您身分識別的通知。</li><li>**在啟動時執行。** 如果您重新啟動手機，此權限可確保您會繼續收到驗證身分識別的通知。</li></ul> |
| 為什麼驗證器可讓您在不解除鎖定裝置的情況下核准要求？ | 您不需解除鎖定裝置來核准驗證要求，因為您只需要證明您帶著您的電話。 雙步驟驗證需要證明兩件事 - 一件是您知道的，另一件則是您擁有的。 您知道的就是密碼。 您所擁有的是您的電話 (設定驗證器，並將其註冊為 MFA 證明。 ) 因此，讓電話和核准要求符合第二個驗證因素的準則。 |
| 為什麼我在我的 Apple Watch 開啟驗證器時，不會顯示我的所有帳戶？ | 驗證器僅支援在 Apple Watch 附屬應用程式上使用推播通知的 Microsoft 個人或學校或公司帳戶。 針對您的其他帳戶（例如 Google 或 Facebook），您必須在手機上開啟驗證器應用程式，以查看您的驗證碼。 |
| 為什麼我不能在 Apple Watch 上核准或拒絕通知？ | 首先，請確定您已在 iPhone 上升級至驗證器、版本6.0.0 或更高版本。 在那之後，在您的 Apple Watch 上開啟 Microsoft Authenticator 隨附應用程式，然後使用位於任何帳戶下方的 [設定] 按鈕來尋找它們。 完成設定流程，以核准這些帳戶的通知。 |
| 我的 Apple Watch 和手機發生通訊錯誤。 我能做什麼來進行疑難排解？ | 若在您的 Watch 跟手機完成通訊之前，畫面先進入睡眠，會出現此項錯誤。<br><b>如果在安裝期間發生錯誤：</b><br>試著重新設定，確保在過程完成之前，您的 Watch 都沒有進入睡眠。 同時請開啟您手機上的 APP，在出現任何提示時進行回應。<br>如果您的手機和觀賞仍未進行通訊，您可以嘗試下列動作：<ol><li>在 iPhone 上強制結束 Microsoft Authenticator 手機應用程式並再次開啟。</li><li>在 Apple Watch 上強制結束隨附應用程式。<ol><li> 在 Watch 上開啟 Microsoft Authenticator 隨附應用程式</li><li>按住側邊按鈕，直到 [關機] 畫面出現為止。</li><li>放開側邊按鈕，然後按住 Digital Crown 以強制結束作用中的應用程式。</li></ol></li><li>針對您的手機和 Watch 關閉藍芽和 Wi-Fi，然後重新開啟它們。</li><li>重新啟動您的 iPhone 和 Watch。</li></ol><b>如果您嘗試核准通知時發生錯誤：</b><br>下次您在 Apple Watch 上確認通知時，確保畫面不要進入睡眠，直到完成要求且聽見提示確認成功之音效。 |
| 為什麼 Apple Watch 的 Microsoft Authenticator 隨附應用程式無法在我的手錶上進行同步處理或顯示呢？ | 如果您的 Watch 上未顯示應用程式，請嘗試下列動作： <ol><li>確定您的 Watch 正在執行 watchOS 4.0 或更新版本。</li><li>再次同步處理您的 Watch。</li></ol> |
| 我的 Apple Watch 附屬應用程式當機。 可以將我的當機記錄傳送給您，讓您可以進行調查嗎？ | 首先，您必須確定已選擇與我們共用您的分析。 若為 TestFlight 使用者，則您已經註冊。 否則，您可以移至 [設定] > [隱私權] > [分析]，然後同時選取 [共用 iPhone 與 Watch 分析] 和 [與應用程式開發人員共用] 選項。<br>註冊之後，您可以嘗試重現當機狀況，如此一來，您的當機記錄就會自動傳送給我們以利調查。 不過，若您無法重現當機狀況，您可以手動複製記錄檔傳送給我們。<ol><li>在手機上開啟 Watch 應用程式、移至 [設定] > [一般]，然後按一下 [複製 Watch 分析]。</li><li>在 [設定] > [隱私權] > [分析] > [分析資料] 下方尋找對應的當機，然後手動複製整個文字。</li><li>在手機上開啟驗證器，並在 [**傳送記錄**] 頁面上，將複製的文字貼到 [**與應用程式開發人員共用**] 文字方塊中。</li></ol> |
| 什麼是應用程式鎖定功能，其如何協助提升我使用裝置時的安全性？ | 若要讓您的一次性密碼、應用程式資訊和應用程式設定更安全，您可以在驗證器中開啟應用程式鎖定功能。 從 [驗證器**設定**] 頁面開啟**應用程式鎖定**，會要求您在每次開啟驗證器時，使用您的 PIN 或生物特徵辨識進行驗證。 這項功能提供額外的保護，您在驗證器中核准通知的方式不會變更。<br>**注意**<br>可惜的是，不保證應用程式鎖定將會阻止某人存取驗證器。 這是因為裝置註冊可能會發生在驗證器以外的其他位置，例如在公司入口網站應用程式或 Android 帳戶設定中。 |
| 為什麼我會收到關於帳戶活動的通知？ | 當您的個人 Microsoft 帳戶發生變更時，就會立即傳送活動通知給驗證器，以協助保護您的安全。 我們先前只會透過電子郵件和 SMS 傳送這些通知。 如需這些活動通知的詳細資訊，請參閱[若您的帳戶出現異常登入，將會如何](https://support.microsoft.com/help/13967/microsoft-account-unusual-sign-in)。 若要變更您收到通知的位置，請登入您帳戶的[如何連絡您有關非重大帳戶警示](https://account.live.com/SecurityNotifications/Update)頁面。 |
| 使用 iOS 隨附的預設郵件應用程式登入我的工作或學校帳戶時，驗證者會收到安全性驗證資訊的提示。 輸入該資訊並返回郵件應用程式之後，卻收到錯誤。 我該怎麼辦？ | 這很可能是因為登入與郵件應用程式跨兩個不同的應用程式，導致初始背景的登入處理序停止運作並失敗。 若要嘗試修正此問題，建議您在登入郵件應用程式時，選取畫面右下方的 **Safari** 圖示。 移至 Safari 後，整個登入處理序會在單一應用程式中進行，讓您成功登入應用程式。 |
| 我的一次性密碼 (OTP) 程式碼無法運作。 我該怎麼辦？ | 請確定您裝置上的日期與時間正確，而且會自動同步。 若日期與時間錯誤或不同步，則該程式碼將無法運作。 |
| Windows 10 行動裝置版作業系統已於 2019 年 12 月被取代。 Windows Mobile 作業系統上的 Microsoft Authenticator 也會被取代嗎？ | 2020年2月28日之後，將不支援所有 Windows Mobile 作業系統上的驗證器。 使用者在上述日期之後，將不符合接收任何新應用程式更新的資格。 2020 年 2 月 28 日之後，目前在所有 Windows Mobile 作業系統上使用 Microsoft Authenticator 支援驗證的 Microsoft 服務，都將開始淘汰其支援。 為能在 Microsoft 服務中進行驗證，我們強烈建議所有使用者，在此日期之前更換為替代的驗證機制。 |

## <a name="next-steps"></a>後續步驟

- 若您在取得個人 Microsoft 帳戶的驗證碼時遇到問題，請參閱 [Microsoft 帳戶安全性資訊及驗證碼](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes)一文中的**排解驗證碼問題**一節。

- 如果您需要雙步驟驗證的詳細資訊，請參閱[對我的帳戶進行雙步驟驗證設定](multi-factor-authentication-end-user-first-time.md)

- 如需安全性資訊的詳細資訊，請參閱[安全性資訊 (預覽) 概觀](./security-info-setup-signin.md)

- 如果您無法在此處找到解答，我們希望能夠聆聽您的意見。 請移至 [Microsoft Authenticator 應用程式論壇 (英文)](https://social.technet.microsoft.com/Forums/en-us/home?forum=MicrosoftAuthenticatorApp) 張貼您的問題並從社群取得協助，或在此頁面上留下您的意見。